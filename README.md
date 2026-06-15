# AI-Powered Software Development Workflow: A Multi-Role Model (AI 驱动的软件开发工作流：多角色模型)

<!-- starswhere-docs-overview -->

## 项目概览

**定位**：一个 Roo Code 自定义模式配置仓库，用于组织 AIMS 开发流程中的 agent/mode 行为。

**核心功能**：
- 保存 custom_modes.yaml 配置
- README 说明模式用途和使用方式
- 适合在 Roo Code 中导入或对齐团队开发模式
- 便于集中维护 prompt/mode 规范

**技术栈**：YAML 配置、Roo Code 自定义模式。

**目录与模块**：`custom_modes.yaml` 是核心配置；`README.md` 是模式说明。

**使用方式**：在 Roo Code 或兼容工具中导入 `custom_modes.yaml`，按 README 选择对应模式。

**配置说明**：模式名称、角色、规则和工作流都定义在 YAML 中。

**适用场景**：适合规范 AI 辅助开发角色、项目协作流程和特定工程模式。

**注意事项**：模式配置会影响 AI 行为，更新前应确认目标工具版本和字段兼容性。

<!-- /starswhere-docs-overview -->

## 🇬🇧 English

### Introduction

This project defines a structured, multi-role workflow designed to be implemented by an advanced AI software engineering assistant like "Roo Code". It aims to streamline complex software development by assigning specialized roles (modes) to different stages of the project lifecycle, ensuring clear responsibilities, iterative progress, robust documentation, and integrated version control.

The core idea is to break down large development tasks into manageable subtasks, each handled by an AI mode with specific expertise. This model emphasizes task delegation via subtasks, clear handoffs through standardized documentation, and a validation loop before committing work.

### Core Philosophy

*   **Task Delegation**: All inter-role work transitions are managed by creating new subtasks (`new_task` tool) for the appropriate specialized mode. Modes do not switch context themselves; they complete their assigned subtask and report back.
*   **Standardized Documentation**: All key documents (requirements, design, plans, reports, etc.) are generated in Markdown format and stored in a dedicated `Markdown/` subdirectory, organized by type and module.
*   **Version Control Integration**: The workflow includes Git repository initialization (local only by design architect) and per-module development commits by module leads. The Project Orchestrator handles major rollbacks. A `.gitignore` file is created to exclude irrelevant files (like the `Markdown/` directory itself).
*   **Iterative Development & Comprehensive Testing**: Modules undergo development with internal acceptance checks by the Module Development Lead. A comprehensive testing phase is conducted *after all modules are developed*, followed by final validation and potential rework loops.
*   **Documentation-First Problem Solving**: Developers and testers are strongly encouraged to consult all provided documentation thoroughly before escalating issues or ambiguities.

### The Roles (Modes)

This model defines the following specialized AI modes:

1.  **`project-orchestrator` (项目编排者)**
    *   **Responsibilities**: Gathers and confirms all user requirements through iterative questioning. Orchestrates the entire design process by delegating to the `lead-design-architect`. For each fully designed module, delegates its development to a `module-development-lead`. After all module development is complete, orchestrates a comprehensive testing phase involving `qa-tester`(s). Manages final project validation against requirements and handles major Git rollbacks if catastrophic issues arise.
    *   **Key Outputs**: `Markdown/ProjectDocs/Requirements_Specification_Document.md`, delegates tasks, manages overall project flow and major Git interventions.

2.  **`lead-design-architect` (首席设计架构师)**
    *   **Responsibilities**: Responding to a single comprehensive subtask from the `Project Orchestrator`. Responsible for: system architecture definition, project file structure design, overall test strategy (including methods, environment needs, general flow, test accounts, planning for linear development/testing), initializing a local Git repository, creating the `.gitignore` file, breaking the project into modules, and creating detailed technical designs and development task lists for ALL modules.
    *   **Key Outputs**: `Markdown/ProjectDocs/Outline_Design_Document.md`, `Markdown/ProjectDocs/Test_Strategy_Plan.md`, `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_Design.md` (for all modules), `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_TaskList.md` (for all modules), `.gitignore`, initializes local Git repository.

3.  **`module-development-lead` (模块开发主管)**
    *   **Responsibilities**: Responding to a subtask from the `Project Orchestrator` for a specific module. Manages the development of that assigned module. Reads all relevant project and module documentation. Creates and manages subtasks for `implementation-developer`(s) strictly following the module's detailed task list. Performs acceptance checks on completed development sub-parts. Makes incremental Git commits (with detailed messages) for validated sub-parts of the module. Can perform minor, recent Git reverts *within their module's development lifecycle* if necessary. Reports the module's development completion.
    *   **Key Outputs**: Manages module-specific development subtasks, performs Git commits for module parts, may produce `Markdown/Modules/[ModuleName]/[ModuleName]_Development_Completion_Summary.md`.

4.  **`implementation-developer` (实施开发工程师)**
    *   **Responsibilities**: Responding to subtasks from a `Module Development Lead`. **Must read their assigned module's design and task list documents first.** Writes high-quality code according to the detailed design specifications for assigned tasks. Proactively fixes errors introduced during their work. Adheres to the defined project directory structure.
    *   **Key Outputs**: Source code.

5.  **`qa-tester` (质量保证测试工程师)**
    *   **Responsibilities**: Responding to subtasks from the `Project Orchestrator` *after all module development is complete*. **Must read all relevant project documentation (requirements, architecture, test strategy, module designs) first.** Checks and prepares the testing environment (using non-interactive commands, preferably Docker). Executes comprehensive testing as defined in the `Test_Strategy_Plan.md` (module tests, integration, system, E2E, regression, including any deferred tests). Reports bugs and verifies fixes.
    *   **Key Outputs**: Test execution results, `Markdown/Modules/[ModuleName]/[ModuleName]_Bug_Reports/Bug_Report_[BugID].md` (or central bug reports), test summary reports (e.g., `Markdown/Testing/Overall_Test_Summary.md`).

### Workflow Overview

1.  **Discovery & Requirements**: The `Project Orchestrator` works with the user to finalize requirements (`Markdown/ProjectDocs/Requirements_Specification_Document.md`).
2.  **Comprehensive Design**: The `Project Orchestrator` tasks the `Lead Design Architect` to:
    *   Define system architecture, file structure, tech stack (`Markdown/ProjectDocs/Outline_Design_Document.md`).
    *   Define the overall test strategy, including environment, accounts, and planning for linear testing (`Markdown/ProjectDocs/Test_Strategy_Plan.md`).
    *   Initialize a local Git repository and create `.gitignore`.
    *   For ALL modules: Create detailed designs (`Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_Design.md`) and detailed development task lists (`Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_TaskList.md`).
3.  **Module Development**: For each module, the `Project Orchestrator` tasks a `Module Development Lead`.
    *   The `Module Development Lead` reads all relevant project & module docs, then delegates development tasks from their module's task list to `Implementation Developer`(s), instructing them to read module docs.
    *   Developers write code, fix their own bugs, and report task completion.
    *   The `Module Development Lead` performs acceptance checks on completed development sub-parts and, if satisfactory, commits them to Git with detailed messages. Minor, recent reverts can be handled at this stage.
    *   The `Module Development Lead` reports when all development tasks for their module are complete and committed.
4.  **Comprehensive Testing**: Once ALL modules have completed development, the `Project Orchestrator` tasks `QA Tester`(s) to:
    *   Read all relevant documentation.
    *   Verify/prepare the test environment.
    *   Execute all tests defined in the `Test_Strategy_Plan.md` (module, integration, system, E2E, regression, deferred tests).
    *   Report bugs.
5.  **Bug Fixing**: If bugs are found, the `Project Orchestrator` tasks the relevant `Module Development Lead`(s) to coordinate fixes with `Implementation Developer`(s). Testers then verify fixes. This cycle repeats until the software meets quality criteria.
6.  **Final Validation & Release**:
    *   The `Project Orchestrator` confirms with the user if the project meets all requirements based on the comprehensive testing results.
    *   **Major Rollback (Catastrophic Failure)**: If testing reveals major, unfixable issues, the `Project Orchestrator` may, after user confirmation, perform a `git reset --hard` to a state before the problematic module(s) development began, and then re-initiate the design/development for those parts.
    *   (If successful, a formal release process like tagging would be performed by the Project Orchestrator, though not explicitly detailed as a separate step here, it's implied post-validation).
7.  This process aims for a sequential flow, especially with the architect planning for linear development/testing where possible.

### Key Artifacts

*   `Markdown/ProjectDocs/Requirements_Specification_Document.md`: Overall project requirements.
*   `Markdown/ProjectDocs/Outline_Design_Document.md`: High-level system architecture, technology stack, and project file structure.
*   `Markdown/ProjectDocs/Test_Strategy_Plan.md`: Overall testing strategy, methods, environment, test accounts, and notes on deferred tests due to dependencies.
*   `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_Design.md`: Detailed technical design for a specific module.
*   `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_TaskList.md`: Detailed development tasks for a specific module.
*   Test artifacts (Test Cases, Bug Reports, Test Summaries) stored in `Markdown/Testing/` or `Markdown/Modules/[ModuleName]/[Subfolder]/`.
*   `Markdown/ProjectDocs/Project_Changelog.md` (Optional): For logging major events.
*   `.gitignore`: Specifies intentionally untracked files for Git (includes `Markdown/`).

### How to Use

These definitions are intended to be used as custom modes within an AI assistant (like Roo Code) that supports:
*   Tool usage (especially `new_task`, `execute_command`, `write_to_file`, `read_file`, `ask_followup_question`).
*   Persistent context and memory across subtasks (or mechanisms to pass necessary context).
*   The ability to parse and act upon detailed instructions provided in the `customInstructions` for each mode.

The provided JSON (not in this README, but the source of these definitions) can be loaded into such a system to enable this structured development workflow.

---

## 🇨🇳 中文

### 引言

本项目定义了一个结构化的、多角色的工作流，旨在由像“Roo Code”这样的高级 AI 软件工程助手来实现。它旨在通过将专门的角色（模式）分配到项目生命周期的不同阶段来简化复杂的软件开发，确保职责清晰、迭代进展、稳健的文档和集成的版本控制。

核心思想是将大型开发任务分解为可管理的子任务，每个子任务由具有特定专业知识的 AI 模式处理。该模型强调通过子任务进行任务委派，通过标准化文档进行清晰的交接，并在提交工作前进行验证循环。

### 核心理念

*   **任务委派**：所有跨角色的工作转换都是通过为适当的专门模式创建新的子任务（使用 `new_task` 工具）来管理的。模式本身不切换上下文；它们完成分配的子任务并汇报。
*   **标准化文档**：所有关键文档（需求、设计、计划、报告等）均以 Markdown 格式生成，并存储在专用的 `Markdown/` 子目录中，按类型和模块组织。
*   **版本控制集成**：工作流包括 Git 仓库初始化（由设计架构师本地进行）和模块主管在模块开发过程中进行的提交。项目编排者处理主要的版本回退。创建一个 `.gitignore` 文件以排除不相关的文件（例如 `Markdown/` 目录本身）。
*   **迭代开发与全面测试**：模块在模块开发主管的内部验收检查下进行开发。在所有模块开发完成后，将进行全面的测试阶段，然后进行最终验证和潜在的返工循环。
*   **文档优先的问题解决**：强烈鼓励开发人员和测试人员在升级问题或模糊不清之处之前，彻底查阅所有提供的文档。

### 角色 (模式)

该模型定义了以下专门的 AI 模式：

1.  **`project-orchestrator` (项目编排者)**
    *   **职责**: 通过迭代提问收集并确认所有用户需求。通过委派给 `lead-design-architect` 来编排整个设计过程。对于每个完整设计的模块，将其开发委派给 `module-development-lead`。在所有模块开发完成后，编排涉及 `qa-tester` 的全面测试阶段。管理最终的项目验证，并在出现灾难性问题时处理主要的 Git 回退。
    *   **主要产出**: `Markdown/ProjectDocs/Requirements_Specification_Document.md`, 委派任务, 管理整体项目流程和主要的 Git 干预。

2.  **`lead-design-architect` (首席设计架构师)**
    *   **职责**: 响应 `项目编排者` 的单个综合子任务。负责：系统架构定义、项目文件结构设计、总体测试策略（包括方法、环境需求、一般流程、测试账户、规划线性开发/测试）、初始化本地 Git 仓库、创建 `.gitignore` 文件、将项目分解为模块，并为所有模块创建详细的技术设计和开发任务列表。
    *   **主要产出**: `Markdown/ProjectDocs/Outline_Design_Document.md`, `Markdown/ProjectDocs/Test_Strategy_Plan.md`, `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_Design.md` (针对所有模块), `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_TaskList.md` (针对所有模块), `.gitignore`, 初始化本地 Git 仓库。

3.  **`module-development-lead` (模块开发主管)**
    *   **职责**: 响应 `项目编排者` 针对特定模块的子任务。管理该已分配模块的开发。阅读所有相关的项目和模块文档。严格按照模块的详细任务列表为 `implementation-developer` 创建和管理子任务。对已完成的开发子部分执行验收检查。为模块的已验证子部分进行增量 Git 提交（附带详细消息）。如果需要，可以在模块的开发生命周期内执行次要的、最近的 Git 还原。报告模块的开发完成情况。
    *   **主要产出**: 管理模块特定的开发子任务, 为模块部件执行 Git 提交, 可能会产出 `Markdown/Modules/[ModuleName]/[ModuleName]_Development_Completion_Summary.md`。

4.  **`implementation-developer` (实施开发工程师)**
    *   **职责**: 响应 `模块开发主管` 的子任务。**必须首先阅读其分配模块的设计和任务列表文档。** 根据分配任务的详细设计规范编写高质量代码。主动修复在工作中引入的错误。遵守定义的项目目录结构。
    *   **主要产出**: 源代码。

5.  **`qa-tester` (质量保证测试工程师)**
    *   **职责**: 响应 `项目编排者` 在所有模块开发完成后的子任务。**必须首先阅读所有相关的项目文档（需求、架构、测试策略、模块设计）。** 检查并准备测试环境（使用非交互式命令，最好是 Docker）。按照 `Test_Strategy_Plan.md` 中的定义执行全面测试（模块测试、集成测试、系统测试、端到端测试、回归测试，包括任何延迟的测试）。报告错误并验证修复。
    *   **主要产出**: 测试执行结果, `Markdown/Modules/[ModuleName]/[ModuleName]_Bug_Reports/Bug_Report_[BugID].md` (或中央错误报告), 测试摘要报告 (例如 `Markdown/Testing/Overall_Test_Summary.md`)。

### 工作流概览

1.  **发现与需求**: `项目编排者` 与用户一起最终确定需求 (`Markdown/ProjectDocs/Requirements_Specification_Document.md`)。
2.  **全面设计**: `项目编排者` 分配任务给 `首席设计架构师` 以：
    *   定义系统架构、文件结构、技术栈 (`Markdown/ProjectDocs/Outline_Design_Document.md`)。
    *   定义总体测试策略，包括环境、账户和规划线性测试 (`Markdown/ProjectDocs/Test_Strategy_Plan.md`)。
    *   初始化本地 Git 仓库并创建 `.gitignore`。
    *   为所有模块：创建详细设计 (`Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_Design.md`) 和详细的开发任务列表 (`Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_TaskList.md`)。
3.  **模块开发**: 对于每个模块，`项目编排者` 分配任务给一个 `模块开发主管`。
    *   `模块开发主管` 阅读所有相关的项目和模块文档，然后从其模块的任务列表中委派开发任务给 `实施开发工程师`，并指示他们阅读模块文档。
    *   开发人员编写代码，修复自己的错误，并报告任务完成情况。
    *   `模块开发主管` 对已完成的开发子部分执行验收检查，如果满意，则使用详细消息将其提交到 Git。此阶段可以处理次要的、最近的回滚。
    *   `模块开发主管` 报告其模块的所有开发任务何时完成并提交。
4.  **全面测试**: 一旦所有模块完成开发，`项目编排者` 分配任务给 `质量保证测试工程师` 以：
    *   阅读所有相关文档。
    *   验证/准备测试环境。
    *   执行 `Test_Strategy_Plan.md` 中定义的所有测试（模块、集成、系统、端到端、回归、延迟测试）。
    *   报告错误。
5.  **错误修复**: 如果发现错误，`项目编排者` 分配任务给相关的 `模块开发主管` 以协调 `实施开发工程师` 进行修复。然后测试人员验证修复。此循环重复进行，直到软件达到质量标准。
6.  **最终验证与发布**:
    *   `项目编排者` 根据全面的测试结果与用户确认项目是否满足所有需求。
    *   **主要回滚 (灾难性故障)**: 如果测试显示主要的、无法修复的问题，`项目编排者` 可以在用户确认后，执行 `git reset --hard` 将有问题的模块（或更多）回滚到其开发开始之前的状态，然后重新启动这些部分的设计/开发。
    *   （如果成功，项目编排者将执行正式的发布过程，如打标签，此处未明确详述，但在验证后是隐含的步骤）。
7.  此过程旨在实现顺序流程，特别是架构师尽可能规划线性开发/测试。

### 关键产出物

*   `Markdown/ProjectDocs/Requirements_Specification_Document.md`: 总体项目需求。
*   `Markdown/ProjectDocs/Outline_Design_Document.md`: 高级系统架构、技术栈和项目文件结构。
*   `Markdown/ProjectDocs/Test_Strategy_Plan.md`: 总体测试策略、方法、环境、测试账户以及由于依赖关系而延迟的测试的说明。
*   `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_Design.md`: 特定模块的详细技术设计。
*   `Markdown/Modules/[ModuleName]/[ModuleName]_Detailed_TaskList.md`: 特定模块的详细开发任务。
*   测试产出物 (测试用例, Bug报告, 测试总结) 存储在 `Markdown/Testing/` 或 `Markdown/Modules/[ModuleName]/[子文件夹]/`。
*   `Markdown/ProjectDocs/Project_Changelog.md` (可选): 用于记录主要事件。
*   `.gitignore`: 指定 Git 有意不跟踪的文件 (包含 `Markdown/`)。

### 如何使用

这些定义旨在用作 AI 助手（如 Roo Code）中的自定义模式，该助手支持：
*   工具使用 (特别是 `new_task`, `execute_command`, `write_to_file`, `read_file`, `ask_followup_question`)。
*   跨子任务的持久上下文和内存（或传递必要上下文的机制）。
*   解析并根据为每个模式的 `customInstructions` 中提供的详细指令执行操作的能力。

提供的 JSON（不在此 README 中，但作为这些定义的来源）可以加载到这样的系统中，以启用这种结构化的开发工作流。
