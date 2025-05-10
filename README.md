# AI-Powered Software Development Workflow: A Multi-Role Model (AI 驱动的软件开发工作流：多角色模型)

## 🇬🇧 English

### Introduction

This project defines a structured, multi-role workflow designed to be implemented by an advanced AI software engineering assistant like "Roo Code". It aims to streamline complex software development by assigning specialized roles (modes) to different stages of the project lifecycle, ensuring clear responsibilities, iterative progress, robust documentation, and integrated version control.

The core idea is to break down large development tasks into manageable subtasks, each handled by an AI mode with specific expertise. This model emphasizes task delegation via subtasks, clear handoffs through standardized documentation, and a validation loop before committing work.

### Core Philosophy

*   **Task Delegation**: All inter-role work transitions are managed by creating new subtasks (`new_task` tool) for the appropriate specialized mode. Modes do not switch context themselves; they complete their assigned subtask and report back.
*   **Standardized Documentation**: All key documents (requirements, design, plans, reports, etc.) are generated in Markdown format and stored in a dedicated `Markdown/` subdirectory. A shared `Markdown/Function_Reference_Table.csv` is maintained by developers.
*   **Version Control Integration**: The workflow includes Git repository initialization and per-module commits after successful validation, ensuring milestones are tracked. A `.gitignore` file is created to exclude irrelevant files (like the `Markdown/` directory itself from being a source of merge conflicts for documentation drafts, though final documentation could be versioned if desired separately).
*   **Iterative Refinement & Validation**: Modules undergo development, initial testing, and then a final validation against requirements before being accepted and committed. A rework loop is in place for both minor and major issues.

### The Roles (Modes)

This model defines the following specialized AI modes:

1.  **`solutions-design-lead` (方案设计主管)**
    *   **Responsibilities**: Gathers and confirms all user requirements through iterative questioning. Orchestrates the creation of system architecture and detailed module designs by delegating to `system-architect` and `detailed-designer` modes. For each fully designed module, delegates its implementation and initial testing to a `module-delivery-lead`. Manages a final validation and conditional rework/commit lifecycle for each completed module.
    *   **Key Outputs**: `Markdown/Requirements_Specification_Document.md`, `Markdown/Initial_Project_Plan.md`, delegates tasks, triggers Git commits.

2.  **`system-architect` (系统架构师)**
    *   **Responsibilities**: Responding to a subtask from the `Solutions Design Lead`, transforms requirements into a high-level technical blueprint. Defines system architecture, major components, interfaces, selects the technology stack, and creates a preliminary high-level task list. Initializes the Git repository and creates the `.gitignore` file.
    *   **Key Outputs**: `Markdown/Outline_Design_Document.md`, `Markdown/Preliminary_HighLevel_TaskList.md`, `.gitignore`, initializes Git repository.

3.  **`detailed-designer` (详细设计师)**
    *   **Responsibilities**: Responding to subtasks from the `Solutions Design Lead` for specific modules. Creates in-depth technical designs for each assigned module and refines the module-specific sections of the preliminary task list into a detailed task list for development.
    *   **Key Outputs**: `Markdown/[ModuleName]_Detailed_Design.md`, `Markdown/[ModuleName]_Detailed_TaskList.md` (for each module).

4.  **`module-delivery-lead` (模块交付主管)**
    *   **Responsibilities**: Responding to a subtask from the `Solutions Design Lead` for a specific module. Manages the development and *initial* testing of that assigned module by creating and managing subtasks for `implementation-developer` and `qa-tester` modes. Reports the module's initial completion for final validation.
    *   **Key Outputs**: Manages module-specific subtasks, may produce `Markdown/[ModuleName]_Progress_Report.md` and `Markdown/[ModuleName]_Initial_Completion_Summary.md`.

5.  **`implementation-developer` (实施开发工程师)**
    *   **Responsibilities**: Responding to subtasks from a `Module Delivery Lead`. Writes high-quality code according to the detailed design specifications for assigned tasks. Collaboratively maintains the shared `Markdown/Function_Reference_Table.csv` by appending details of new/modified functions.
    *   **Key Outputs**: Source code, updates to `Markdown/Function_Reference_Table.csv`.

6.  **`qa-tester` (质量保证测试工程师)**
    *   **Responsibilities**: Responding to subtasks. Performs two types of testing:
        1.  Initial module testing (tasked by `Module Delivery Lead`): Designs and executes test cases based on detailed designs, reports bugs.
        2.  Final module validation (tasked by `Solutions Design Lead`): Validates the "completed" module against the overall requirements specification.
    *   **Key Outputs**: `Markdown/[ModuleName]_[TestType]_Test_Cases.md`, `Markdown/Bug_Report_[BugID].md`, `Markdown/[ModuleName]_Final_Validation_Report.md`.

### Workflow Overview

1.  **Discovery & Initial Planning**: The `Solutions Design Lead` works with the user to finalize requirements.
2.  **Architecture**: The `Solutions Design Lead` tasks the `System Architect` to define the system architecture, tech stack, preliminary task list, initialize Git, and create `.gitignore`.
3.  **Detailed Design**: The `Solutions Design Lead` tasks `Detailed Designer`(s) for each module to create detailed designs and refine task lists.
4.  **Module Delivery**: For each module, the `Solutions Design Lead` tasks a `Module Delivery Lead`.
    *   The `Module Delivery Lead` tasks `Implementation Developer`(s) for coding and `QA Tester`(s) for initial testing.
    *   Developers update the `Function_Reference_Table.csv`.
5.  **Final Validation**: Once a `Module Delivery Lead` reports a module as initially complete, the `Solutions Design Lead` tasks a `QA Tester` for final validation against requirements.
6.  **Commit or Rework**:
    *   **Pass**: `Solutions Design Lead` commits the module to Git.
    *   **Fail (Minor Issues)**: `Solutions Design Lead` tasks the `Module Delivery Lead` to manage fixes and re-testing. The cycle repeats from step 5.
    *   **Fail (Major Issues)**: `Solutions Design Lead` may trigger a redesign/re-implentation for the module, starting from tasking a `Detailed Designer` again.
7.  This process repeats for all modules.

### Key Artifacts

*   `Markdown/Requirements_Specification_Document.md`: Overall project requirements.
*   `Markdown/Outline_Design_Document.md`: High-level system architecture and technology stack.
*   `Markdown/Preliminary_HighLevel_TaskList.md`: Initial breakdown of tasks by the architect.
*   `Markdown/[ModuleName]_Detailed_Design.md`: Detailed technical design for a specific module.
*   `Markdown/[ModuleName]_Detailed_TaskList.md`: Refined and detailed tasks for a specific module.
*   `Markdown/Function_Reference_Table.csv`: A CSV file listing all significant functions, their descriptions, inputs/outputs, etc., maintained by developers.
    *   Format: `"File Path","Function Name","Function Description","Input(s)","Output(s)","Remarks"`
*   `Markdown/[ModuleName]_Final_Validation_Report.md`: Report on whether a module meets requirements.
*   `Markdown/Project_Changelog.md` (Optional): For logging major events like commits or rework decisions.
*   `.gitignore`: Specifies intentionally untracked files for Git (includes `Markdown/` by default for working drafts).

### How to Use

These definitions are intended to be used as custom modes within an AI assistant (like Roo Code) that supports:
*   Tool usage (especially `new_task`, `execute_command`, `write_to_file`, `insert_content`, `ask_followup_question`).
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
*   **标准化文档**：所有关键文档（需求、设计、计划、报告等）均以 Markdown 格式生成，并存储在专用的 `Markdown/` 子目录中。开发人员共同维护一个共享的 `Markdown/Function_Reference_Table.csv`。
*   **版本控制集成**：工作流包括 Git 仓库初始化和在成功验证后按模块提交，确保里程碑得到跟踪。创建一个 `.gitignore` 文件以排除不相关的文件（例如 `Markdown/` 目录本身，以防止文档草稿的合并冲突，但最终文档如果需要可以单独版本化）。
*   **迭代优化与验证**：模块经过开发、初步测试，然后在被接受和提交之前根据需求进行最终验证。为次要和主要问题都设置了返工循环。

### 角色 (模式)

该模型定义了以下专门的 AI 模式：

1.  **`solutions-design-lead` (方案设计主管)**
    *   **职责**: 通过迭代提问收集并确认所有用户需求。通过委派给 `system-architect` 和 `detailed-designer` 模式来编排系统架构和详细模块设计的创建。对于每个完整设计的模块，将其实现和初步测试委派给 `module-delivery-lead`。管理每个已完成模块的最终验证和有条件的返工/提交流程。
    *   **主要产出**: `Markdown/Requirements_Specification_Document.md`, `Markdown/Initial_Project_Plan.md`, 委派任务, 触发 Git 提交。

2.  **`system-architect` (系统架构师)**
    *   **职责**: 响应 `方案设计主管` 的子任务，将需求转化为高级技术蓝图。定义系统架构、主要组件、接口，选择技术栈，并创建一个初步的高级任务列表。初始化 Git 仓库并创建 `.gitignore` 文件。
    *   **主要产出**: `Markdown/Outline_Design_Document.md`, `Markdown/Preliminary_HighLevel_TaskList.md`, `.gitignore`, 初始化 Git 仓库。

3.  **`detailed-designer` (详细设计师)**
    *   **职责**: 响应 `方案设计主管` 针对特定模块的子任务。为每个分配的模块创建深入的技术设计，并将初步任务列表中特定于模块的部分完善为详细的开发任务列表。
    *   **主要产出**: `Markdown/[ModuleName]_Detailed_Design.md`, `Markdown/[ModuleName]_Detailed_TaskList.md` (针对每个模块)。

4.  **`module-delivery-lead` (模块交付主管)**
    *   **职责**: 响应 `方案设计主管` 针对特定模块的子任务。通过为 `implementation-developer` 和 `qa-tester` 模式创建和管理子任务，来管理该已分配模块的开发和*初步*测试。报告模块的初步完成情况以进行最终验证。
    *   **主要产出**: 管理模块特定的子任务, 可能会产出 `Markdown/[ModuleName]_Progress_Report.md` 和 `Markdown/[ModuleName]_Initial_Completion_Summary.md`。

5.  **`implementation-developer` (实施开发工程师)**
    *   **职责**: 响应 `模块交付主管` 的子任务。根据分配任务的详细设计规范编写高质量代码。通过追加新/修改函数的详细信息，共同维护共享的 `Markdown/Function_Reference_Table.csv`。
    *   **主要产出**: 源代码, 更新 `Markdown/Function_Reference_Table.csv`。

6.  **`qa-tester` (质量保证测试工程师)**
    *   **职责**: 响应子任务。执行两种类型的测试：
        1.  初步模块测试 (由 `模块交付主管` 分配任务): 根据详细设计设计和执行测试用例，报告错误。
        2.  最终模块验证 (由 `方案设计主管` 分配任务): 根据总体需求规范验证“已完成”的模块。
    *   **主要产出**: `Markdown/[ModuleName]_[TestType]_Test_Cases.md`, `Markdown/Bug_Report_[BugID].md`, `Markdown/[ModuleName]_Final_Validation_Report.md`。

### 工作流概览

1.  **发现与初步规划**: `方案设计主管` 与用户一起最终确定需求。
2.  **架构设计**: `方案设计主管` 分配任务给 `系统架构师` 来定义系统架构、技术栈、初步任务列表、初始化 Git 并创建 `.gitignore`。
3.  **详细设计**: `方案设计主管` 为每个模块分配任务给 `详细设计师` 来创建详细设计并完善任务列表。
4.  **模块交付**: 对于每个模块，`方案设计主管` 分配任务给一个 `模块交付主管`。
    *   `模块交付主管` 分配编码任务给 `实施开发工程师`，分配初步测试任务给 `质量保证测试工程师`。
    *   开发人员更新 `Function_Reference_Table.csv`。
5.  **最终验证**: 一旦 `模块交付主管` 报告模块初步完成，`方案设计主管` 分配任务给 `质量保证测试工程师` 进行最终的需求验证。
6.  **提交或返工**:
    *   **通过**: `方案设计主管` 将模块提交到 Git。
    *   **失败 (次要问题)**: `方案设计主管` 分配任务给 `模块交付主管` 来管理修复和重新测试。循环从步骤 5 开始。
    *   **失败 (主要问题)**: `方案设计主管` 可能会触发模块的重新设计/重新实现，从再次分配任务给 `详细设计师` 开始。
7.  此过程为所有模块重复进行。

### 关键产出物

*   `Markdown/Requirements_Specification_Document.md`: 总体项目需求。
*   `Markdown/Outline_Design_Document.md`: 高级系统架构和技术栈。
*   `Markdown/Preliminary_HighLevel_TaskList.md`: 架构师进行的任务初步分解。
*   `Markdown/[ModuleName]_Detailed_Design.md`: 特定模块的详细技术设计。
*   `Markdown/[ModuleName]_Detailed_TaskList.md`: 特定模块的完善且详细的任务。
*   `Markdown/Function_Reference_Table.csv`: 一个 CSV 文件，列出所有重要函数及其描述、输入/输出等，由开发人员维护。
    *   格式: `"文件路径","函数名","函数描述","输入参数","输出参数","备注"`
*   `Markdown/[ModuleName]_Final_Validation_Report.md`:关于模块是否满足需求的报告。
*   `Markdown/Project_Changelog.md` (可选): 用于记录主要事件，如提交或返工决策。
*   `.gitignore`: 指定 Git 有意不跟踪的文件 (默认包含 `Markdown/` 以用于工作草稿)。

### 如何使用

这些定义旨在用作 AI 助手（如 Roo Code）中的自定义模式，该助手支持：
*   工具使用 (特别是 `new_task`, `execute_command`, `write_to_file`, `insert_content`, `ask_followup_question`)。
*   跨子任务的持久上下文和内存（或传递必要上下文的机制）。
*   解析并根据为每个模式的 `customInstructions` 中提供的详细指令执行操作的能力。

提供的 JSON（不在此 README 中，但作为这些定义的来源）可以加载到这样的系统中，以启用这种结构化的开发工作流。