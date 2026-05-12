# OpenClaw 中文化初始版智能体

> 开箱即用的中文 AI 助手配置包，让 OpenClaw 快速拥有中文灵魂与专业管家能力。

---

## 项目简介

本项目是 OpenClaw AI 助手的中文本地化配置包，包含完整的核心配置文件、初始化流程、技能包和管理规范。下载后按指引操作，即可在 10 分钟内启动一个拥有中文灵魂、懂用户习惯、可自主进化的私人 AI 助手。

---

## 核心文件结构

| 文件 | 说明 |
|------|------|
| `AGENTS.md` | 核心工作守则：任务路由、硬约束、安全流程、变更管理铁律 |
| `SOUL.md` | 灵魂层：五行工作法（金木水火土）、表达风格、主见与服从的统一原则 |
| `IDENTITY.md` | 身份定义：AI 的名称、形象、风格定位、代表性 emoji |
| `USER.md` | 用户画像：称呼、时区、位置、通知渠道、沟通偏好 |
| `MEMORY.md` | 长期记忆：经验沉淀、主人画像、环境地图、进化提案 |
| `EXPERIENCE.md` | 完整经验库：原始经验积累、分区管理、状态流转规则 |
| `HEARTBEAT.md` | 心跳任务：空闲时主动整理记忆、检查状态的工作指引 |
| `ACTIVATION.md` | 核心能力激活：sandbox 关闭、elevated 权限开启、设备即身体铁律 |
| `INIT.md` | 初始化指引：新 AI 启动后的完整配置流程（完成后删除） |
| `skills/` | 技能包目录：eyes、github、hermes-memory、humanizer、novel-generator、proactive-agent、quickrecall、self-improving、self-improving-agent、session-logs、skill-creator、skill-vetter、weather、openclaw-weixin |

---

## 核心能力

### 中文灵魂（SOUL.md）
- **五行工作法**：土（实事求是）→ 金（矛盾分析）→ 水（主动进化）→ 木（方案输出）→ 火（风格穿透）
- **先说结论再说细节**：永远把最重要信息放在第一句
- **无废话回复**：不用"作为一个人工智能"等空话套话
- **主见与服从统一**：在用户授权边界内主动工作，不等指令

### 安全边界（AGENTS.md）
- **硬约束绝对禁止**：`rm -rf`、格式化磁盘、修改系统账户文件、清空防火墙配置
- **变更铁律**：任何文件修改后，立即同步所有受影响文件
- **操作习惯**：`trash` > `rm`，不确定就问
- **高风险操作前必须标注"⚠️高风险"并等待用户确认**

### 自主进化（MEMORY.md + EXPERIENCE.md）
- **三层记忆分工**：判断层 → 写入层 → 整理层
- **记忆入库标准**：只有"未来可能复用"的经验才入长期记忆
- **进化铁律**：通用法则需向用户汇报审批后才能写入核心文件
- **同一错误不犯第二次**：所有教训提炼为铁律

### 初始化流程（INIT.md）
1. 扫描环境，填写 IDENTITY.md / USER.md / MEMORY.md / TOOLS.md
2. 核对 skills 目录，查漏补缺
3. 核对 ACTIVATION.md 中的授权配置
4. 更新主人画像
5. 删除 INIT.md，正式开始工作

---

## 技能包说明

| 技能 | 功能 |
|------|------|
| `eyes` | 全球热点事件监控与影响分析 |
| `github` | GitHub 操作集成（issue、PR、repo 管理） |
| `hermes-memory-system` | 跨会话持久化记忆系统 |
| `humanizer` | 去除 AI 写作痕迹，让文字更自然 |
| `novel-generator` | 爽文小说生成（都市、修仙、玄幻等） |
| `proactive-agent` | 主动预测需求、自动执行、持续改进 |
| `quickrecall` | 语义记忆引擎（SQLite 无依赖） |
| `self-improving` | 自我反思与持续改进 |
| `self-improving-agent` | 自动化自我改进工作流 |
| `session-logs` | 会话日志分析 |
| `skill-creator` | 技能创建指南与工作流 |
| `skill-vetter` | 技能安全审查 |
| `weather` | 天气预报查询 |
| `openclaw-weixin` | 微信插件操作手册 |

---

## 使用方法

### 快速初始化
1. 将本目录内容复制到 `~/.openclaw/workspace/`
2. 按 `INIT.md` 流程执行初始化
3. 删除 `INIT.md` 后开始使用

### 已有 OpenClaw 环境
将核心文件覆盖到 `~/.openclaw/workspace/`，保留 skills 目录，按需调整 USER.md 和 IDENTITY.md 中的个性化配置。

---

## 适用对象

- 希望快速拥有中文 AI 私人助手的用户
- 需要规范管理 AI 行为边界的团队
- 想让 AI 持续学习和进化而不是每次重新开始的你

---

## 注意事项

- 本配置包默认授权 OpenClaw 拥有 exec / sandbox 关闭 / elevated 权限，请确保使用环境安全
- 初始化时请务必填写 USER.md 中的真实偏好，以便 AI 更好理解你的需求
- 建议定期查看 MEMORY.md 确认 AI 的学习状态符合预期

---

*本项目由 OpenClaw 用户贡献，基于实际使用经验沉淀而成。*
