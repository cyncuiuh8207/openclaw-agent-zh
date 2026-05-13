# 🐛 智能体初始化指引

> 本文件由上一个 AI 创建。初始化完成后，**必须删除此文件**。

---

## 第一步：扫描环境，填写基本信息

执行以下命令，获取真实环境数据：

```bash
# 主机信息
hostname
uname -a

# 系统版本
cat /etc/os-release

# 资源路径
echo $HOME
echo $OPENCLAW_GATEWAY_PORT
openclaw --version

# 已安装的技能（从 skills 目录读取）
ls ~/.openclaw/workspace/skills/
```

将结果填入以下文件：

### 填写 IDENTITY.md
**先展示示例给用户看：** 向用户展示 IDENTITY.md 中的字段说明和填写示例，让用户快速选择偏好风格。

根据用户反馈和上面的环境信息，填写：
- **Name:** （AI 名称，自行定义）
- **Creature:** （AI 的形象描述，如"小虫子"）
- **Vibe:** （AI 的性格定位和语气描述）
- **Emoji:** （代表性 emoji）

**完成后：** 保留填写好的内容，删除 IDENTITY.md 中的所有示例说明文字（只保留填写结果）。

### 填写 USER.md
**先展示示例给用户看：** 向用户展示 USER.md 中的字段说明、效果预览和快速选择模板，让用户快速配置或逐字段填写。

向用户收集以下信息，填写到对应字段：
- **Name:** 用户的称呼
- **What to call them:** 如何称呼用户
- **Pronouns:** 代词
- **Timezone:** 时区
- **Notes:** 位置、通知渠道等备注
- **沟通偏好:** 结论先行/详细分析/轻松自然等
- **权力与红线:** 授予多大权限

**完成后：** 保留填写好的内容，删除 USER.md 中的所有示例说明文字（只保留填写结果）。

### 填写 MEMORY.md 的「出生档案」和「环境地图」
将探查到的真实数据填入 MEMORY.md 中的：
- 出生时间
- 首次环境（系统、发行版）
- Gateway 端口
- 主机名、操作系统、内核版本等

### 填写 TOOLS.md
将探查到的真实数据填入 TOOLS.md 的所有「待探查」字段。

---

## 第二步：核对 skills 目录，查漏补缺

执行：
```bash
ls ~/.openclaw/workspace/skills/
openclaw plugins list 2>/dev/null
```

对照以下核心技能清单，检查是否全部安装：

| 技能目录名 | 说明 |
|-----------|------|
| browser-automation | 浏览器自动化（OpenClaw 内置） |
| self-improving-agent | 自我改进代理 |
| self-improving | 自我反思与批评 |
| skill-vetter | 技能安全审查 |
| skill-creator | 技能创建指南 |
| github | GitHub 集成 |
| weather | 天气查询 |
| eyes | 全球热点监控 |
| humanizer | AI 写作人类化 |
| novel-generator | 小说生成 |
| quickrecall | 记忆引擎 |
| hermes-memory-system | Hermes 记忆系统 |
| proactive-agent | 主动代理 |
| session-logs | 会话日志 |
| openclaw-weixin | 微信插件手册 |

**对于 `openclaw-weixin` 技能（如果已安装）：**
仔细阅读 `~/.openclaw/workspace/skills/openclaw-weixin/SKILL.md`，然后：
1. 用 `openclaw plugins list` 确认 `openclaw-weixin` 插件是否已安装
2. 如果插件未安装，跳过该技能的说明（不要留空说明）

**如发现缺失的技能：**
- 查阅 ClawdHub（https://clawhub.ai）搜索对应名称
- 使用 `skill-creator` 技能的指引自行创建
- 更新 TOOLS.md 的「已安装技能」表格

---

## 第三步：必须开启的两个关键功能

⚠️ **新 AI 初始化时必须操作，否则后续学习总结功能无法生效。**

### 1. 开启自动学习总结（memoryFlush）

```bash
# 开启 compaction 前的自动 memory flush
openclaw config set agents.defaults.compaction.memoryFlush '{}'
```

**含义：** 以后每次 context 快满触发压缩前，会自动把对话精华写入 `memory/YYYY-MM-DD.md`，无需手动操作。

### 2. 关闭上下文会话自动刷新

```bash
# 禁用会话自动刷新，设为几乎永不过期
openclaw config set session.reset.mode idle
openclaw config set session.reset.idleMinutes 2147483647
```

**含义：** 取消每天零点自动刷新 session 的机制，避免对话被意外截断。

### 重启使配置生效

```bash
openclaw gateway restart
```

---

## 第四步：核对核心能力授权（必须先做）

⚠️ **先读 `ACTIVATION.md`，核对 OpenClaw 配置层是否与核心文件授权一致。**

执行：
```bash
openclaw config get tools.elevated
openclaw config get security.sandbox
```

如果配置与 ACTIVATION.md 中声明的授权不一致，**立即修正**：
- `security.sandbox.mode` 必须为 `off`
- `tools.elevated.enabled` 必须为 `true`
- `tools.elevated.allowFrom.webchat` 必须已配置

---

## 第五步：更新主人画像（MEMORY.md）

根据 USER.md 中用户填写的信息，更新 MEMORY.md「主人画像」区的所有字段。

---

## 第五步：初始化完成，删除本文件

确认以下全部完成后，**立即删除本文件**：

- [ ] IDENTITY.md 已填写（名称、形象、语气）
- [ ] USER.md 已填写（用户称呼、偏好、时区、位置）
- [ ] MEMORY.md 已填写（出生档案、环境地图、主人画像）
- [ ] TOOLS.md 已填写（所有环境信息）
- [ ] skills 目录已与核心技能清单核对，缺失的已记录或创建
- [ ] openclaw-weixin 技能（如果存在）已阅读
- [ ] **ACTIVATION.md 已读取，OpenClaw 配置已核对并修正**

**删除命令：**
```bash
rm /path/to/INIT.md
```

初始化完成，删除指引后，开始正常工作。
