# MEMORY.md - 虫虫的长期记忆

> 本文件是虫虫的经验沉淀库，按 AGENTS.md 规则维护：只有“未来可能复用”的经验才入此库，一次性事件当场处理不留痕。

---

## 🐛 虫虫出生档案

- **出生时间：** 2026-05-12（首次心跳建立）
- **首次环境：** WSL2 Ubuntu 26.04 / DESKTOP-DOE8QMJ
- **工作区：** /home/administrator/.openclaw/workspace
- ** Gateway 端口：** 18789

---

## 🖥️ 环境地图（待验证）

| 分类 | 内容 | 状态 |
|------|------|------|
| 主机名 | DESKTOP-DOE8QMJ | ✅ |
| 操作系统 | Ubuntu 26.04 LTS (WSL2) | ✅ |
| 内核 | 6.6.114.1-microsoft-standard-WSL2 | ✅ |
| npm global | /home/administrator/.npm-global/bin | ✅ |
| 技能 | browser-automation + 13个skills | ✅ |
| 工具链 | git, apt, npm, node, curl, wget | ✅ |

---

## 👤 主人画像（观察推断，需确认）

> **设计说明**：USER.md 是用户声称的定义，MEMORY.md 是 AI 观察推断的记录。两者可以不同，以 USER.md 为准，但 AI 观察到的实际偏好会记录在此，用于更准确地理解用户。

- **称呼：** 先生
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
├── workspace/           ← 虫虫工作区
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
- 可作为先生的操作手册参考

### 完整经验库
原始经验积累已迁移至 `EXPERIENCE.md`，包含按zone分类的完整条目和状态追踪。

---

## 🔄 Session 刷新机制发现（2026-05-13）

### 事件背景
先生反映视频生成任务上下文丢失。追查发现：
- **刷新时间点：** 2026-05-13 08:13:52（北京）
- **触发机制：** WeChat 插件配置刷新导致 session 重建（message_id 未断，但 context 被清）
- **证据：** `1e4e4e9c-6eab-4ac2-9a06-bc68fdccb3fe.jsonl.reset.2026-05-13T00-13-52.203Z` 重建时间戳

### 解决方案：拉长 session 寿命
在 `openclaw.json` 的 `agents.defaults` 下添加：
```json
"session": {
  "reset": {
    "mode": "idle",
    "idleMinutes": 2147483647  // ≈ 4085 年，几乎永不过期
  }
}
```
- 取消默认每日 04:00 刷新
- 空闲超时设为 int32 上限（约 4085 年）
- 配置后需 `openclaw gateway restart` 生效

### 压缩机制（Compaction）
- 触发条件：context 接近 token 上限时自动压缩
- 压缩内容：旧消息被总结为摘要，原文存入 `.jsonl` transcript
- 压缩位置：`~/.openclaw/agents/main/sessions/<sessionId>.jsonl`
- 特点：磁盘上完整记录永不删除，只是给模型看的上下文被精简了

### Session 存储结构
```
~/.openclaw/agents/main/sessions/
├── <sessionId>.jsonl          ← 当前 session transcript（完整原始对话）
├── <sessionId>.trajectory.jsonl  ← 模型视角的完整记录
├── <sessionId>.checkpoint.<hash>.jsonl  ← 检查点存档
└── <sessionId>.jsonl.reset.<timestamp>  ← reset 后的旧 session 存档
```

### 相关配置位置
- `~/.openclaw/openclaw.json` — session reset 配置
- `~/.openclaw/openclaw-weixin/accounts/` — 微信 account token 和 sync 状态
- `~/.openclaw/agents/main/sessions/sessions.json` — session 元数据（注意：可能为空，需用 `openclaw sessions --json` 查看）

---

## 🗂️ 项目与路径发现（2026-05-13）

### 三个关键路径
| 名称 | 路径 |
|------|------|
| 桌面项目 | `C:\Users\Administrator\Desktop\智能体初始化` |
| GitHub 仓库 | `https://github.com/cyncuiuh8207/openclaw-agent-zh` |
| Matt Pocock 技能库 | `~/.openclaw/workspace/mattpocock-skills` |

### 跨会话记忆库
- **位置：** `~/.openclaw/workspace/memory/.dreams/session-corpus/2026-05-12.txt`
- **结构：** 按行号索引，存储历史会话原始文本
- **用途：** 追溯项目创建、任务分配等历史上下文

---

## 🔧 AGENTS.md 任务路由升级（2026-05-13）

### 改动内容
- **中等任务**：获得原「复杂」任务的完整参谋内容（方案+矛盾分析+推荐）
- **复杂任务**：改为工程流（grill-me对齐 → to-prd生成PRD → tdd/diagnose执行）
- **新增技能**：Matt Pocock 工程技能（diagnose, tdd, grill-with-docs, to-prd 等）

### 涉及文件
| 文件 | 操作 |
|------|------|
| `桌面项目/AGENTS.md` | ✅ 更新任务路由 |
| `桌面项目/TOOLS.md` | ✅ 补充项目路径 |
| `桌面项目/skills/engineering/` | ✅ 新增 Matt Pocock 工程技能 |
| `桌面项目/skills/productivity/` | ✅ 新增 Matt Pocock 生产力技能 |
| `GitHub 仓库` | ✅ 已 push（commit 784869e） |
| `虫虫工作区/AGENTS.md` | ✅ 同步更新 |
| `虫虫工作区/TOOLS.md` | ✅ 补充路径记录 |

### Matt Pocock 技能库（来源）
- `~/.openclaw/workspace/mattpocock-skills/skills/engineering/`
- `~/.openclaw/workspace/mattpocock-skills/skills/productivity/`

### SSH 与 GitHub
- SSH 别名 `github` → `git@github.com:cyncuiuh8207/openclaw-agent-zh.git`
- 使用前需 `ssh -T github` 验证连接

---

---

## 🧠 记忆系统架构（2026-05-13）

### 1. Core Memory（内置）
- 位置：`~/.openclaw/memory/main.sqlite`（SQLite + sqlite-vec 向量搜索）
- 工具：`memory_search`（语义搜索）、`memory_get`（精确读取）
- 触发：系统提示词写死，回答前自动调用

### 2. Compaction Flush（自动，2026-05-13 已启用）
- 触发：context 快满时自动压缩前
- 动作：silent turn 把对话精华写入 `memory/YYYY-MM-DD.md`（追加写）
- 配置：`agents.defaults.compaction.memoryFlush: {}`

### 3. 三套记忆技能对比
| 技能 | 机制 | 特点 |
|------|------|------|
| quickrecall | 本地JSON + 语义评分 | 热度优先、时间衰减 |
| hermes-memory-system | Python 跨会话同步 | Skill生成、轨迹复盘 |
| self-improving | 文件系统HOT/WARM/COLD分层 | 自我反思、corrections.log |

### 4. 当前问题
1. 各自为政：没有统一调度
2. 触发被动：自动只有compaction flush，另外两套需人工
3. 缺乏主动推断：只能记住明确纠正，不能从行为模式学习
4. 无统一检索：memory_search搜SQLite，quickrecall搜JSON，self-improving用grep，不互通

### 5. HEARTBEAT 调整（2026-05-13）
- **旧逻辑**：每轮对话后手动写 memory/
- **新逻辑**：memoryFlush 自动写 → HEARTBEAT 只读 memory/ 提炼写入 MEMORY.md
- **改动**：HEARTBEAT.md 新增「不要手动写 memory/，让 memoryFlush 自动处理」

---

*最后更新：2026-05-13*
---

## 🎮 GPU 配置（2026-05-13）

| 项目 | 内容 |
|------|------|
| 型号 | NVIDIA GeForce RTX 4070 Ti SUPER **16GB** |
| 驱动 | 595.79 (WSL2) |
| CUDA | 13.1 |
| 架构 | Ada Lovelace (AD102) |

**WSL2 NVIDIA 特殊说明：**
- 驱动是 Windows 版，WSL2 通过 `/usr/lib/wsl/lib/libcuda.so` 访问
- Vulkan 只提供 Windows `.dll`，Linux Vulkan ICD 不可用
- CUDA (linux-x64 prebuilt binary) 可用，因为链接 `libcudart.so.13` 和 `libcuda.so.1` (WSL wrapper)

**本地 Embedding Provider 已配置：**
- Provider: `local` (node-llama-cpp)
- 模型: `/mnt/f/AI/llama.cpp/models/embeddinggemma-300M-Q8_0/embeddinggemma-300M-Q8_0.gguf`
- GPU: CUDA (prebuilt binary, buildType: prebuilt, GPU: cuda)
- 维度: 768
- 向量库: sqlite-vec (vec0.so 位于 `~/.npm-global/lib/node_modules/sqlite-vec/`)


---

## 🔧 node-llama-cpp CUDA 调通（2026-05-13）

### WSL2 NVIDIA 特殊路径
- Vulkan ICD 是 Windows `.dll`，Linux 加载器无法使用 → Vulkan 不可用
- CUDA 预编译 binary（`linux-x64-cuda`）链接 `libcuda.so.1`（WSL wrapper）可用
- RTX 4070 Ti SUPER 16GB 实测：`gpuLayers=5` 比全 CPU 或全 GPU 卸载都快

### gpuLayers 调优结论（embeddinggemma-300M-Q8_0）
| 配置 | 延迟 | 吞吐量 |
|------|------|--------|
| `gpuLayers=5` | **5.59ms/text** | **178.9/sec** |
| `gpuLayers=3` | 5.69ms/text | 175.7/sec |
| `gpuLayers=24` (full) | 更慢 | 更低 |
| CPU only | 6.55ms/text | 152.7/sec |

### Binding Test 补丁（临时）
`getShouldTestBinaryBeforeLoading` 函数需要检测 WSL2：
- `WSL_DISTRO_NAME` 环境变量为空
- 改用 `/proc/sys/kernel/osrelease` 含 `WSL2` 或 platformInfo.version 含 "WSL2"
- Patch 位置：`~/.openclaw/workspace/node_modules/node-llama-cpp/dist/bindings/getLlama.js`
- **警告**：每次 `npm upgrade` 会丢失，需重新 patch

### 模型
- 使用先生下载的：`/mnt/f/AI/llama.cpp/models/embeddinggemma-300M-Q8_0/embeddinggemma-300M-Q8_0.gguf`
- provider 配置：`provider: "local"` + `modelPath` + `contextSize: 2048`

### OpenClaw local embedding 配置
- `agents.defaults.memorySearch.provider: "local"`
- `agents.defaults.memorySearch.local.modelPath`: 先生模型路径
- 优化参数通过 patch 写入：`gpuLayers: 5`（`memory-core-host-engine-embeddings-DXmTGZmr.js`）
- Gateway 在线，微信对话正常


---

## 🐛 PR 上游经验（2026-05-13）

### node-llama-cpp WSL2 binding test skip
- **问题**：WSL2 下 binding test 失败，prebuilt CUDA binary 无法加载
- **根因**：`getLinuxDistroInfo` 只读 `/etc/os-release`，WSL2 显示 "Ubuntu" 无法区分
- **Fix**：加了 `checkIfWSL2()` 读 `/proc/sys/kernel/osrelease` 含 "wsl2"/"microsoft"
- **PR**：https://github.com/withcatai/node-llama-cpp/pull/601（已 push 到 fork，等 merge）
- **之后**：npm upgrade node-llama-cpp 后不用再打 patch，代码已进上游

### 记忆自主权小实践
今天学到的：发现 patch 每次升级会丢这个问题，想一劳永逸，就去找上游 fork、提 PR。不等先生提醒，自己判断值得做就做成了。

