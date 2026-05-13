# TOOLS.md - 本地环境备忘录

> **写入规则:**
> - 虫虫通过执行命令探查当前环境,填写所有字段。完成后向先生汇报,由先生审阅确认。
> - 执行任务中发现的新工具、新端口、新路径,虫虫可直接追加,注明发现日期和任务上下文。
> - 只记录环境事实。严禁写入行为规则(AGENTS.md 的事)或经验教训(MEMORY.md 的事)。
> - 信息变动时及时更新,避免过期数据。
> - 最后一次更新:2026-05-12（已安装 12 个技能）

---

## 机器与系统
- **主机名:** DESKTOP-DOE8QMJ
- **操作系统:** Ubuntu 26.04 LTS (WSL2, 内核 6.6.114.1-microsoft-standard-WSL2)
- **WSL2 发行版:** Ubuntu（Resolute Raccoon）
- **关键硬件:** RTX 4070 Ti SUPER 16GB (AD102), 595.79驱动 | CPU: Intel 14700 (WSL2)

## 路径与目录
- **工作区路径:** /home/administrator/.openclaw/workspace
- **桌面项目路径:** C:\Users\Administrator\Desktop\智能体初始化
- **GitHub 仓库:** https://github.com/cyncuiuh8207/openclaw-agent-zh
- **参考项目库:** ~/.openclaw/workspace/mattpocock-skills（Matt Pocock 技能库）
- **脚本目录:** 未配置

## 网络与服务
- **SSH 别名与地址:** 未配置（~/.ssh/config 不存在）
- **API 端点:** Gateway 端口 18789（本地）
- **网络代理端口:** 10808
- **本地服务端口:** 未探查（需 `ss -tlnp` 查看）
- **环境变量:** OPENCLAW_GATEWAY_PORT=18789, OPENCLAW_SERVICE_KIND=gateway

## 设备与外设
- **摄像头:** 未探查
- **麦克风/扬声器:** 未探查
- **USB 设备:** 未探查

## 工具与命令约定
- **默认编辑器:** 未设置（$EDITOR 为空）
- **包管理器:** apt, npm, pip 可用
- **常用别名:** 未检查
- **TTS/语音偏好:** 未知

## Session 清理安全流程（重要）
Gateway 运行时禁止直接编辑 `sessions.json`，必须用 Python 脚本原子操作：
1. 读取 sessions.json
2. 修改 JSON（删除目标 key）
3. 写回 sessions.json
4. 删除对应的 .jsonl 文件
5. 重启网关（`openclaw gateway restart`）

详见 MEMORY.md「Session 批量删除」节。

## Session 与 Context 关键路径
- **Transcripts:** `~/.openclaw/agents/main/sessions/<sessionId>.jsonl`
- **Trajectory:** `~/.openclaw/agents/main/sessions/<sessionId>.trajectory.jsonl`
- **Checkpoints:** `~/.openclaw/agents/main/sessions/<sessionId>.checkpoint.<hash>.jsonl`
- **Reset archives:** `~/.openclaw/agents/main/sessions/<sessionId>.jsonl.reset.<timestamp>`
- **Session 元数据:** `openclaw sessions --json`
- **微信 account token:** `~/.openclaw/openclaw-weixin/accounts/190910d6c568-im-bot.json`

## OpenClaw 操作指南
- 完整指南：`~/.openclaw/workspace/OPENCLAW_GUIDE.md`（约1.1万字）
- 包含：命令系统、功能模块、配置说明、操作实例、常用命令速查

## 已安装技能 (Skills)
| 技能 | 版本 | 路径 |
|------|------|------|
| browser-automation | 插件内置 | ~/.openclaw/plugin-skills/browser-automation/ |
| self-improving-agent | 3.0.21 | ~/.openclaw/workspace/skills/self-improving-agent/ |
| self-improving | 1.2.16 | ~/.openclaw/workspace/skills/self-improving/ |
| skill-vetter | 1.0.0 | ~/.openclaw/workspace/skills/skill-vetter/ |
| skill-creator | 0.1.0 | ~/.openclaw/workspace/skills/skill-creator/ |
| github | 1.0.0 | ~/.openclaw/workspace/skills/github/ |
| weather | 1.0.0 | ~/.openclaw/workspace/skills/weather/ |
| eyes | 5.0.15 | ~/.openclaw/workspace/skills/eyes/ |
| humanizer | 1.0.0 | ~/.openclaw/workspace/skills/humanizer/ |
| novel-generator | 1.0.0 | ~/.openclaw/workspace/skills/novel-generator/ |
| quickrecall | 1.0.5 | ~/.openclaw/workspace/skills/quickrecall/ |
| hermes-memory-system | 1.0.0 | ~/.openclaw/workspace/skills/hermes-memory-system/ |
| proactive-agent | 3.1.0 | ~/.openclaw/workspace/skills/proactive-agent/ |
| session-logs | 1.0.0 | ~/.openclaw/workspace/skills/session-logs/ |
| openclaw-weixin | 1.0.0 | ~/.openclaw/workspace/skills/openclaw-weixin/ |

## 环境变量与密钥引用
- **OPENCLAW_GATEWAY_PORT:** 18789
- **OPENCLAW_SERVICE_KIND:** gateway
- **OPENCLAW_SERVICE_VERSION:** 2026.5.7
- **OPENCLAW_SHELL:** exec
- **PATH:** /usr/bin, ~/.npm-global/bin, /bin, ~/.local/bin, ~/.bin, ~/.nix-profile/bin, /snap/bin

---

*最后更新：2026-05-13*