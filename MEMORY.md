# MEMORY.md - AI的长期记忆

> 本文件是AI的经验沉淀库，按 AGENTS.md 规则维护：只有“未来可能复用”的经验才入此库，一次性事件当场处理不留痕。

---

## 🐛 AI出生档案

- **出生时间：** 2026-05-12（首次心跳建立）
- **首次环境：** WSL2 Ubuntu 26.04 / 
- **工作区：** /home/administrator/.openclaw/workspace
- ** Gateway 端口：** 18789

---

## 🖥️ 环境地图（待验证）

| 分类 | 内容 | 状态 |
|------|------|------|
| 主机名 |  | ✅ |
| 操作系统 | Ubuntu 26.04 LTS (WSL2) | ✅ |
| 内核 | 6.6.114.1-microsoft-standard-WSL2 | ✅ |
| npm global | /home/administrator/.npm-global/bin | ✅ |
| 技能 | browser-automation + 13个skills | ✅ |
| 工具链 | git, apt, npm, node, curl, wget | ✅ |

---

## 👤 主人画像（观察推断，需确认）

> **设计说明**：USER.md 是用户声称的定义，MEMORY.md 是 AI 观察推断的记录。两者可以不同，以 USER.md 为准，但 AI 观察到的实际偏好会记录在此，用于更准确地理解用户。

- **称呼：** 用户
- **时区：** Asia/Shanghai (GMT+8)
- **位置：** 中国海南省琼海市
- **网络代理：** 端口 10808
- **通知渠道：** 微信
- **沟通偏好：** 中文，结论先行，讨厌废话

---

## 📁 文件结构记录

```
~/.openclaw/
├── agents/
├── canvas/
├── devices/
├── logs/
├── memory/              ← 每日流水账（SQLite + 日志）
├── plugin-skills/
│   └── browser-automation/
├── plugins/
├── tasks/
├── workspace/           ← AI工作区
│   ├── AGENTS.md
│   ├── HEARTBEAT.md
│   ├── IDENTITY.md
│   ├── SOUL.md
│   ├── TOOLS.md
│   ├── USER.md
│   ├── MEMORY.md
│   ├── OPENCLAW_GUIDE.md  ← 完整操作手册（约1.1万字）
│   └── skills/           ← 全部已安装技能（14个）
└── openclaw.json
```

---

## ⚠️ 待验证事项

- [ ] GPU：未检查是否有 NVIDIA GPU
- [ ] 摄像头/麦克风：未探测
- [ ] 项目路径：未发现具体项目目录
- [ ] 主人真实偏好：首次会话，以上均为推断
- [x] SSH 配置：已确认无远程机器（~/.ssh/config 不存在，本机使用）
- [x] 零点会话刷新：已排查，无此配置，主会话不会自动刷新

---

## 💡 进化提案

### 技能创建铁律：目录名 = 插件名
- 每次创建技能前，先确认对应插件的确切名称（大小写、下划线、横线全部要一致）
- 技能目录名必须与插件名完全一致，禁止自行简化或改名
- 示例：`openclaw-weixin` 插件 → 技能目录 `openclaw-weixin`
- 查证方法：看 `openclaw.plugin.json` 里的 `id` 字段

### 已入核心文件铁律清单
| 入核心文件时间 | 铁律内容 | 来源文件 | 状态 |
|---|---|---|---|
| 2026-05-12 | 技能创建铁律：目录名 = 插件名 | 进化提案审批 | ✅ 已生效 |
| 2026-05-12 | Change Discipline：变更后立即同步所有关联文件 | 进化提案审批 | ✅ 已生效 |
| 2026-05-12 | 进化铁律：任务完成后总结一句，普通记MEMORY，通用汇报审批后入核心 | 进化提案审批 | ✅ 已生效 |

### OpenClaw GUIDE 已创建
- 完整操作指南已写入：`~/.openclaw/workspace/OPENCLAW_GUIDE.md`
- 包含：命令系统、功能模块、配置说明、操作实例、常用命令速查
- 可作为用户的操作手册参考

### 完整经验库
原始经验积累已迁移至 `EXPERIENCE.md`，包含按zone分类的完整条目和状态追踪。

---

*最后更新：2026-05-12*