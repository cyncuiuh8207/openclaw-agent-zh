# openclaw-weixin 插件使用手册

> 源码级整理，基于 v2.4.3。帮助手册，忘记细节时查阅。

---

## 安装与登录

### 安装插件
```bash
openclaw plugins install "@tencent-weixin/openclaw-weixin"
```

### 启用插件
```bash
openclaw config set plugins.entries.openclaw-weixin.enabled true
```

### 扫码登录（一个微信号）
```bash
openclaw channels login --channel openclaw-weixin
```
终端会显示二维码，用手机微信扫描并在手机上确认授权。

### 添加更多微信号
重复执行扫码命令即可，每个微信号独立存储凭证。

### 重启 Gateway
```bash
openclaw gateway restart
```

---

## 配置项说明

| 配置项 | 位置 | 作用 |
|--------|------|------|
| `plugins.entries.openclaw-weixin.enabled` | openclaw.json | 启用/禁用插件 |
| `channels.openclaw-weixin.botAgent` | openclaw.json | 自定义 UA 标识（默认 OpenClaw），方便后台日志区分 |
| `channels.openclaw-weixin.cdnBaseUrl` | openclaw.json | CDN 域名，通常不需要改 |
| `channels.openclaw-weixin.accounts[].routeTag` | openclaw.json | 路由标签 |
| `session.dmScope` | openclaw.json | 多账号私聊隔离策略，建议设为 `per-account-channel-peer` |

### 自定义 botAgent 示例
```json
{
  "channels": {
    "openclaw-weixin": {
      "botAgent": "MyBot/1.2.0"
    }
  }
}
```

---

## 账号管理

### 凭证存储位置
```
~/.openclaw/openclaw-weixin/
├── accounts/
│   ├── {accountId}.json          # 令牌 + baseUrl
│   ├── {accountId}.sync.json      # getUpdates 同步游标
│   └── {accountId}.context-tokens.json  # 会话上下文令牌
└── accounts.json                 # 账号索引
```

### 账号 ID 格式
- 规范化格式：`xxxxx@im.wechat`（用户）或 `xxxxx@im.bot`（机器人）
- 发送消息时使用 `xxx@im.wechat` 作为 `to` 地址

---

## 消息发送

### 文本消息
虫虫自动处理，使用当前会话的 contextToken 回复。

### 发送图片/文件/视频
```javascript
// 使用 message tool，action=send
// media 参数：本地绝对路径 或 远程 HTTPS URL
// 示例（本地）:
MEDIA:/tmp/photo.png

// 示例（网络图片）:
MEDIA:https://example.com/image.jpg
```

**⚠️ 关键要求：路径必须为绝对路径**
- ✅ `/tmp/photo.png`
- ✅ `/home/administrator/photo.jpg`
- ❌ `./photo.png`（相对路径会失败）
- ❌ `photo.png`（相对路径会失败）

### 媒体类型路由
| MIME 类型 | 上传方式 |
|-----------|----------|
| `video/*` | uploadVideoToWeixin + sendVideoMessageWeixin |
| `image/*` | uploadFileToWeixin + sendImageMessageWeixin |
| 其他 | uploadFileAttachmentToWeixin + sendFileMessageWeixin |

### 文件上传 CDN 流程
```
本地文件 → MD5明文 → AES-128-ECB加密 → getUploadUrl获取参数 → 上传CDN → 构造CDNMedia引用 → 发送
```

---

## 定时任务（Cron）注意事项

**⚠️ 创建微信用户的定时任务时，必须同时指定 `to` 和 `accountId`：**

```javascript
delivery: {
  mode: "announce",
  channel: "openclaw-weixin",
  to: "<user_id>@im.wechat",      // 用户的微信ID（当前会话的 From 字段）
  accountId: "<current_AccountId>" // 当前微信账号的 accountId
}
```

缺少 `to`：Cron 投递失败，报 `requires target`
缺少 `accountId`：消息可能从错误的机器人账号发出

---

## 消息接收（长轮询机制）

- 插件通过 `getUpdates` 长轮询从微信服务器拉取新消息
- 轮询超时 35 秒（服务器建议的超时会动态调整）
- 断线重连：连续失败 3 次后退避 30 秒
- Session 过期（errcode -14）：自动暂停，等待后可恢复
- `get_updates_buf` 游标持久化，Gateway 重启后不丢消息

---

## contextToken 管理

- 每条入站消息携带 `context_token`，回复时必须回传
- 存储在 `~/.openclaw/openclaw-weixin/accounts/{accountId}.context-tokens.json`
- Gateway 重启后自动从磁盘恢复
- 多账号场景：通过 `from_user_id` 匹配对应账号的 contextToken

---

## 多账号路由规则

发送消息时账号推断顺序：
1. 显式指定了 `accountId` → 直接使用
2. 单账号 → 直接使用
3. 多账号 → 通过 `contextToken` 匹配 `to` 用户
4. 多账号 + 匹配到多个 → **报错**，要求显式指定
5. 多账号 + 无匹配 → **报错**

---

## 常见错误处理

| 错误 | 原因 | 处理 |
|------|------|------|
| `contextToken missing` | 首次发送无会话 | 警告但仍发送，之后自动获取 |
| `session expired (errcode -14)` | 会话超时 | 自动暂停，等待后恢复 |
| `3 consecutive failures` | 网络或服务器问题 | 自动退避 30 秒重试 |
| `ambiguous account` | 多账号匹配到多个 | 显式指定 accountId |
| `no account registered` | 未登录或凭证丢失 | 重新扫码登录 |
| 文件发送失败 | 相对路径 | 改用绝对路径 |

---

## 出站消息限制

- 文本：单条最多 4000 字符
- Markdown：自动过滤流式标记（`data:`, `DONE` 等）
- 媒体：自动根据 MIME 类型路由

---

## 卸载

```bash
openclaw plugins uninstall @tencent-weixin/openclaw-weixin
```
