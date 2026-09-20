# VelaAILab

基于 openvela 的科研、学习与任务管理智能 Agent 助手系统

## 一、作品简介

VelaAILab 是基于 openvela 官方 AI Agent 与 Skill 机制构建的 AI Native 智能辅助原型，面向科研学习、知识整理与任务管理场景，探索大语言模型能力与智能终端系统的结合方式。

项目采用 **AI Agent + Skill + Tool** 的模块化架构。openvela AI Agent 作为统一交互入口，理解用户的自然语言请求，并根据 Skill 描述选择相应能力；需要确定性操作时，再调用文件读写、时间获取等内置工具。目前项目新增并集成了以下三个自定义 Skill：

- **Research Assistant（研究助手）**：辅助制定研究计划、拆解实验任务、安排阶段目标和整理研究过程。
- **Study Assistant（学习助手）**：辅助制定学习计划、规划复习任务和整理技术学习内容。
- **VelaStudy Task Manager（任务管理器）**：支持任务创建、查询、完成状态更新与持久化保存。

项目在 openvela Goldfish Emulator 环境中完成运行验证，并保留了项目开发过程中的 AI Coding 日志与运行证据。

## 二、参赛方向

**AI 硬件产品创新**

本项目围绕智能终端中的自然语言交互与任务执行需求，基于 openvela 现有 AI Agent 扩展场景 Skill，使终端具备任务理解、能力选择、工具调用和结果反馈能力。

## 三、系统架构

系统总体流程如下：

```text
用户请求
   ↓
vela Console
   ↓
AI Agent
   ↓
Skill 匹配与加载
   ↓
目标 Skill
   ↓
Tools / MiMo V2.5 LLM Backend
   ↓
任务执行结果
```

各模块职责如下：

- **AI Agent**：理解用户请求并组织任务处理流程。
- **Skill 匹配与加载**：由 openvela AI Agent 根据请求内容和 Skill 描述选择目标 Skill。
- **Research Assistant**：处理研究规划与实验任务相关请求。
- **Study Assistant**：处理学习计划与复习规划相关请求。
- **VelaStudy Task Manager**：处理任务记录、查询和状态管理。
- **Tools**：提供文件读写、时间获取等基础能力。
- **MiMo V2.5 LLM Backend**：提供自然语言理解与内容生成能力。

## 四、项目目录

```text
contest2026_486_VelaAILab/
├── app/
│   └── hello_app/                # manifest 映射的应用目录
├── board/
│   └── contest_board/            # manifest 映射的板级目录
├── quickapp/
│   └── hello_quickapp/           # manifest 映射的快应用目录
├── skills/                       # 自定义 Skill
│   ├── research-assistant.md
│   ├── study-assistant.md
│   └── velastudy-task-manager.md
├── logs/                         # AI Coding 日志
│   ├── README.md
│   └── XSC0102/
│       ├── manifest.json
│       ├── 2026-09-15/
│       └── 2026-09-16/
├── docs/
│   └── evidence/                 # 项目运行与验证材料
└── README.md                     # 项目说明
```

说明：以上目录展示项目的主要组成，具体文件以提交压缩包中的实际内容为准。

## 五、运行环境

项目已验证的主要环境如下：

- Ubuntu 22.04.5 LTS
- openvela `dev-ai-contest-2026` 开发环境
- Goldfish Emulator
- MiMo V2.5 LLM Backend

## 六、运行方式

### 6.1 进入 openvela 工作区

```bash
cd ~/openvela-workspace
```

### 6.2 启动 Goldfish Emulator

```bash
./emulator.sh cmake_out/vela_goldfish-arm64-v8a-ap
```

### 6.3 查看 Skill

模拟器进入 `goldfish-armv8a-ap>` 提示符后，先启动 AI Agent：

```text
ai_agent
```

出现 `vela>` 提示符后，可执行已经成功验证的 Skill 列表命令：

```text
ask 技能列表
```

### 6.4 执行 AI Agent 任务

在 vela Console 中输入：

```text
ask <任务内容>
```

系统将结合请求内容和 Skill 描述选择相应能力。运行时自定义 Skill 位于：

```text
/data/ai_agent/skills/
```

## 七、核心功能

### 7.1 Research Assistant

Research Assistant 面向科研活动，主要支持：

- 生成研究计划；
- 拆解实验任务；
- 安排阶段目标；
- 整理论文阅读计划；
- 输出结构化研究建议。

已成功测试的任务：

```text
ask I am researching temporal knowledge graph reasoning. Give me a concise research plan in Chinese with paper reading, experiment tasks, progress checkpoint, and next action. Do not use the web.
```

该任务成功生成中文研究计划，运行记录以 `END status=ok` 结束。

### 7.2 Study Assistant

Study Assistant 面向课程学习与考试准备，主要支持：

- 制定学习计划；
- 规划复习进度；
- 拆解技术主题；
- 安排每日学习任务；
- 输出结构化学习建议。

### 7.3 VelaStudy Task Manager

VelaStudy Task Manager 面向长期任务管理，主要支持：

- 创建任务；
- 查看任务列表；
- 查询任务状态；
- 更新任务完成状态；
- 持久化保存任务记录。

任务数据保存在：

```text
/data/ai_agent/VELASTUDY_TASKS.md
```

此前测试已创建“复习C语言指针”任务。以下为已成功执行的状态更新任务：

```text
ask Use write_file now. Move 复习C语言指针 from Pending to Completed. Do not read files first.
```

该任务实际调用 `write_file`，返回 `OK: wrote 83 bytes to /data/ai_agent/VELASTUDY_TASKS.md`，并以 `END status=ok` 结束。

已成功执行的查询任务：

```text
ask Read /data/ai_agent/skills/velastudy-task-manager.md. Show tasks.
```

查询结果显示“复习C语言指针”位于 `Completed`，并以 `END status=ok` 结束。

## 八、项目特点与创新点

### 8.1 Agent + Skill 模块化架构

项目将不同场景能力封装为相互独立的 Skill，并由 AI Agent 统一理解请求和调度。新增能力时可以继续扩展 Skill，降低功能耦合度。

### 8.2 面向研究与学习场景的能力组合

项目没有将大语言模型仅作为问答工具，而是围绕研究规划、学习辅助和任务管理建立连续的任务处理流程。

### 8.3 任务持久化管理

VelaStudy Task Manager 支持保存任务状态，使 AI Agent 不仅能够生成内容，还能够参与任务记录与后续管理。

### 8.4 面向 openvela 的 AI Native 应用探索

项目结合 openvela 运行环境、Skill 机制和大语言模型后端，探索 AI Agent 在智能终端中的交互与执行方式。

### 8.5 AI 辅助开发闭环

项目开发形成以下闭环：

```text
需求分析 → AI 辅助设计 → Skill 实现 → 运行调试 → 功能验证 → 文档与日志沉淀
```

## 九、AI Coding 使用说明

项目开发过程中使用 AI 工具辅助完成以下工作：

- 需求拆解与系统方案设计；
- Agent 与 Skill 架构规划；
- Skill 内容编写与优化；
- 运行错误分析与调试；
- 测试步骤与验证材料整理；
- 技术报告和项目文档完善。

使用的 AI 工具包括：

- ChatGPT
- Codex
- OpenCode
- MiMo V2.5

完整 AI Coding 日志保存在：

```text
logs/XSC0102/
```

其中 `manifest.json` 用于记录日志信息，按日期划分的目录中保存对应的开发会话日志。

## 十、验证情况

项目已完成以下验证：

- openvela 开发环境运行验证；
- Goldfish Emulator 启动验证；
- AI Agent 基础交互验证；
- Skill 列表识别验证；
- Research Assistant 调用验证；
- Study Assistant 调用验证；
- VelaStudy Task Manager 调用验证；
- 任务持久化相关功能验证；
- AI Coding 日志整理与目录检查。

项目运行截图及其他验证材料保存在：

```text
docs/evidence/
```

## 十一、提交说明

- 项目源码、Skill、AI Coding 日志和验证材料均包含在提交目录中。
- `logs/XSC0102/` 为参赛者账号对应的真实 AI Coding 日志目录。
- 最终提交时不包含官方 README 备份文件、Git 历史目录、临时文件、缓存文件及敏感配置信息。

## 十二、作品信息

- 作品名称：VelaAILab
- 参赛方向：AI 硬件产品创新
- 参赛账号：XSC0102
- 运行平台：openvela
