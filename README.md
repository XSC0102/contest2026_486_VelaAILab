# VelaAILab

基于 openvela 的智能 Agent 助手系统

## 项目简介

VelaAILab 面向科研规划、学习计划和任务管理场景，在 openvela Goldfish Emulator 中验证 `AI Agent + Skill + Tool` 的模块化智能助手架构。用户通过 vela Console 输入自然语言请求，AI Agent 根据 Skill 描述完成能力路由，并在需要持久化或时间信息时调用工具；自然语言理解与计划生成由 MiMo V2.5 大语言模型后端提供。

当前版本包含 3 个自定义 Skill：

| Skill | 主要能力 | 典型输出或数据 |
| --- | --- | --- |
| Research Assistant | 研究计划、论文阅读、实验安排、里程碑与下一步 | `Goal`、`Paper reading`、`Experiment tasks`、`Progress checkpoint`、`Next action` |
| Study Assistant | 课程、考试、复习、日常学习和自学计划 | `Study goal`、`Today's plan`、`Review task`、`Checkpoint`、`Start now` |
| VelaStudy Task Manager | 新增、查询、完成学习或科研任务 | `/data/ai_agent/VELASTUDY_TASKS.md` |

本项目当前是以命令行交互为主的功能验证原型，已经在 Goldfish Emulator/openvela 环境中完成 Agent 调度、Skill 加载、文件工具调用和任务状态维护验证；尚未完成实体开发板、图形界面或语音入口适配。

## 系统架构

```text
用户自然语言请求
        │
        ▼
   vela Console
        │
        ▼
 openvela AI Agent ─────── MiMo V2.5 LLM Backend
        │
        ▼
    Skill Router
        │
        ├── Research Assistant
        ├── Study Assistant
        └── VelaStudy Task Manager
        │
        ▼
 read_file / write_file / get_current_time
        │
        ▼
计划结果或持久化任务数据
```

职责边界如下：

- openvela AI Agent：理解请求、维护执行流程并组织最终回复。
- Skill Router：根据 Skill 标题、描述和请求语义选择能力。
- 自定义 Skill：约束领域规则、执行步骤和输出格式。
- Tool：完成文件读取、写入和时间获取等确定性操作。
- MiMo V2.5：提供自然语言理解、规划和内容生成能力。

其中 Agent、Router 和基础 Tool 属于 `openvela.xml` 引用的上游 `packages/ai_agent` 工程；本仓库主要保存团队自定义 Skill、Repo manifest、运行证据和 AI Coding 日志。

## 仓库内容

```text
contest2026_486_VelaAILab/
├── skills/
│   ├── research-assistant.md
│   ├── study-assistant.md
│   └── velastudy-task-manager.md
├── docs/evidence/
│   ├── ai-agent-final-runtime.md
│   └── ai-coding-runtime.md
├── logs/
│   ├── XSC0102/                  # 团队真实 AI Coding 日志
│   │   ├── 2026-09-15/
│   │   ├── 2026-09-16/
│   │   └── manifest.json
│   └── your-github-login/        # 组委会示例日志，非团队证据
├── app/hello_app/                # 赛事模板 Hello App
├── quickapp/hello_quickapp/      # 赛事模板 QuickApp
├── board/contest_board/          # 赛事模板板级占位目录
├── contest2026_486_VelaAILab.xml # 团队 Repo manifest
└── openvela.xml                  # openvela 基础工程 manifest
```

### Repo manifest

`contest2026_486_VelaAILab.xml` 包含 `openvela.xml`，并将团队仓库中的三个模板目录映射到 openvela 工作区：

| 源目录 | openvela 工作区目标目录 |
| --- | --- |
| `app/hello_app` | `packages/demos/contest2026_486_hello_app` |
| `quickapp/hello_quickapp` | `packages/apps/contest2026_486_hello_quickapp` |
| `board/contest_board` | `vendor/openvela/boards/contest2026_486_board` |

`openvela.xml` 使用 `dev-ai-contest-2026` 分支，并声明 `packages/ai_agent` 等 openvela 组件。压缩包只包含 manifest，不包含 Repo 同步后的完整 `nuttx/`、`apps/`、`frameworks/`、`vendor/` 和 `packages/ai_agent/` 源码树。

### 自定义 Skill

运行时将 3 个 Skill 部署到：

```text
/data/ai_agent/skills/
```

当前包内文件与最终运行证据记录的 SHA-256 完全一致：

| 文件 | 大小 | SHA-256 |
| --- | ---: | --- |
| `research-assistant.md` | 1977 bytes | `1342fabb332b8a2ca11397c30746a5abfa4d5dc20d7d7b31f95d024e7b11874d` |
| `study-assistant.md` | 1356 bytes | `7ed8b7ade34465eddf0e989a4c85a15c6df676a9357262817371f2b1563f7758` |
| `velastudy-task-manager.md` | 1473 bytes | `74140ae40bb6a0f32e00912331392a994cc2321a8aa310e263a811a3d1cb6ca8` |

Research Assistant 与 Study Assistant 已加入明确的路由边界：研究项目、论文阅读和实验规划优先进入 Research Assistant，即使请求中出现 `study`、`learn` 或 `learning`；课程、考试、复习和日常学习计划进入 Study Assistant。

### 运行证据与日志

- `docs/evidence/ai-coding-runtime.md`：记录早期 Skill 部署、Study Assistant 验证、任务新增/查询/完成和 ADB 持久化验证；其中 Research Assistant 的早期测试因 60 秒 watchdog 超时。
- `docs/evidence/ai-agent-final-runtime.md`：记录后续 Research Assistant 最终测试。MiMo 响应用时约 91.675 秒，超过旧阈值但最终以 `END status=ok` 完成，并记录最终 Skill 哈希。
- `logs/XSC0102/`：包含 2026 年 9 月 15 日和 16 日的真实 OpenCode 会话，主要记录 Skill 行为、路由规则及描述解析问题的迭代过程。
- `logs/your-github-login/`：组委会模板示例，不计入本项目的 AI Coding 证据。

两份 evidence 反映先后两轮测试，不应把早期 timeout 与最终成功结果视为冲突。模型响应时间受网络和云端服务状态影响，当前材料不提供稳定的固定时延指标。

## 运行环境

技术报告记录的验证环境如下：

| 项目 | 配置 |
| --- | --- |
| 主机操作系统 | Ubuntu 22.04.5 LTS |
| 处理器 | Intel Core i5-14600K |
| 内存 | 10 GiB |
| 运行平台 | openvela Goldfish Emulator |
| 目标架构 | ARM64 |
| AI Agent | openvela AI Agent |
| 大语言模型 | MiMo V2.5 |
| 交互入口 | vela Console |
| 测试日期 | 2026 年 9 月 19 日 |

## 运行前提

本仓库不是完整 openvela 源码快照。运行前需准备：

1. 使用本仓库 manifest 同步 `dev-ai-contest-2026` 对应的完整 openvela 工作区。
2. 按 openvela AI Agent 的要求完成 MiMo V2.5 模型服务配置；不要把 API Key 或 Token 写入仓库。
3. 构建 ARM64 Goldfish Emulator 镜像和 `packages/ai_agent`。
4. 将 `skills/` 下的 3 个文件原样部署到 `/data/ai_agent/skills/`。
5. 确认运行环境允许 Agent 使用 `read_file`、`write_file` 和 `get_current_time`。

包内 evidence 还列出了运行时修改过的 `src/core/agent_loop.c`、`src/core/context_builder.c` 和 `src/tools/skill_loader.c`。这些文件属于上游 AI Agent 工程，未包含在本团队仓库压缩包中；仅凭本压缩包不能复原这些核心源码修改，复现时应使用实际验证过的 `packages/ai_agent` 工作区版本。

## 启动与基本检查

以下命令适用于已经完成同步、配置和构建的验证环境。构建输出目录应以本机实际配置为准。

```bash
cd ~/openvela-workspace
./emulator.sh cmake_out/vela_goldfish-arm64-v8a-ap
```

进入 `goldfish-armv8a-ap>` 后启动 AI Agent：

```text
ai_agent
```

出现 `vela>` 提示符后，可先检查 Skill 是否加载：

```text
ask 技能列表
```

预期能够识别：

```text
Research Assistant
Study Assistant
VelaStudy Task Manager
```

## 已成功测试的示例任务

以下命令采用此前实际成功测试并保留下来的任务，不是根据功能说明临时编写的示例。

### 1. 生成科研计划

```text
ask I am researching temporal knowledge graph reasoning. Give me a concise research plan in Chinese with paper reading, experiment tasks, progress checkpoint, and next action. Do not use the web.
```

该任务加载 `/data/ai_agent/skills/research-assistant.md`，生成中文科研计划，最终运行证据记录为：

```text
END status=ok
```

### 2. 将任务从 Pending 更新为 Completed

测试前任务文件中已存在“复习C语言指针”。成功使用的验证命令为：

```text
ask Use write_file now. Move 复习C语言指针 from Pending to Completed. Do not read files first.
```

运行时实际调用 `write_file`，并返回：

```text
OK: wrote 83 bytes to /data/ai_agent/VELASTUDY_TASKS.md
END status=ok
```

这条命令用于复现当时的定向工具调用测试。日常使用时，应遵循当前 `velastudy-task-manager.md` 的完整流程：先读取整个任务文件，再保存更新后的完整内容。

### 3. 查询任务状态

```text
ask Read /data/ai_agent/skills/velastudy-task-manager.md. Show tasks.
```

成功结果显示“复习C语言指针”位于 `Completed`，并以以下状态结束：

```text
END status=ok
```

技术报告记录 Study Assistant 测试通过，`ai-coding-runtime.md` 也记录其加载成功且最终状态为 `ok`；但现有材料没有保存可逐字核对的完整成功输入，因此这里不补写未经证据确认的 Study Assistant 命令。

## 功能测试结果

技术报告记录的核心功能测试如下：

| 编号 | 测试内容 | 实际结果 | 结论 |
| --- | --- | --- | --- |
| F01 | 查询可用 Skill | 可识别并列出 3 个 Skill | 通过 |
| F02 | 生成科研计划 | 完成 Research Assistant 计划生成 | 通过 |
| F03 | 生成学习计划 | 完成 Study Assistant 学习计划生成 | 通过 |
| F04 | 创建学习任务 | 完成任务创建及写入 | 通过 |
| F05 | 查询任务 | 完成任务读取与展示 | 通过 |
| F06 | 更新完成状态 | 完成 Pending 到 Completed 状态迁移 | 通过 |
| F07 | 获取当前时间 | 完成时间工具调用 | 通过 |

这些结果来自技术报告和 `docs/evidence/`。`logs/XSC0102/` 是 AI Coding 开发日志，不是完整的 vela Console 运行日志。

## 已知限制与仓库边界

- `app/hello_app/`、`quickapp/hello_quickapp/` 和 `board/contest_board/` 仍是赛事模板，占位代码中保留 `team 000`、`contest2026_000` 等标识；它们不是 VelaAILab Agent 的核心实现。
- `board_boot.c` 是空初始化占位实现，`defconfig` 也明确标注不用于启动真实硬件，不能据此声称已完成实体开发板适配。
- QuickApp 清单引用 `/common/logo.png`，但当前包中没有对应资源；当前验证入口是 vela Console，不是 QuickApp 图形界面。
- Agent 核心源码及 evidence 中提到的三处源码修改未包含在团队仓库压缩包内。
- 当前版本依赖网络和 MiMo V2.5 云端服务；网络或模型接口不可用时，复杂规划无法正常生成。
- Task Manager Skill 还描述了可选的 `edit_file` 和 `cron_add` 路径，但当前提交材料只明确验证了主要文件读写、时间获取和任务状态维护流程，不把定时提醒列为已完成测试项。
- 报告中的 `1,522,634 Tokens` 和“AI Coding 代码占比约 30%”属于团队统计口径；现有 JSONL 日志未包含可独立汇总该总量的 token 字段。

## AI-Native 开发说明

项目使用 ChatGPT、Codex 和 MiMo V2.5 辅助需求拆解、架构讨论、Skill 设计、路由调试、测试整理和技术文档编写。真实开发日志显示，团队围绕 Research Assistant 的默认约束处理、Research/Study 路由冲突以及 Skill 描述加载规则进行了多轮迭代，并通过运行证据验证最终 Skill 文件。

AI 工具用于生成候选方案和加速重复工作，环境部署、功能取舍、文件落地、运行验证和最终审核由团队完成。

## 当前状态

当前压缩包已包含 VelaAILab 的 3 个自定义 Skill、最终哈希、两份运行证据和团队真实 AI Coding 日志，能够说明项目的 Skill 设计与验证过程。它不是完整 openvela 工作区，也未包含运行时修改后的 AI Agent 核心源码，因此完整复现仍依赖对应的 `dev-ai-contest-2026` openvela 环境和实际验证过的 `packages/ai_agent` 版本。
