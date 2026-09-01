# Software Testing Skills

一组面向软件测试实践的 Agent Skills，帮助 Agent 从需求生成结构化测试用例，并使用风险分析、等价类、边界值、探索测试和缺陷报告等方法完善测试设计。

适用于 **Cursor、Claude Code 以及其他兼容 Agent Skills 格式的工具**。

> **核心用法：** 提供一份需求、用户故事、PRD 或接口说明，生成可落盘的 Markdown 测试用例文档。

[English](#english)

## 特性

- 从需求、PRD、用户故事和接口说明生成测试用例。
- 使用等价类、边界值、状态转换和风险优先级减少遗漏。
- 在需求不完整时生成探索性测试清单。
- 根据历史缺陷和测试结果补充回归测试范围。
- 生成可复用的 Markdown 测试设计文档。
- 提供缺陷报告、覆盖缺口和剩余风险的表达框架。
- 不依赖运行时服务、数据库或第三方 API；Skill 本身是 Markdown 知识文件。

## Skill 清单

本项目共包含 **11 个 Skill**：1 个编排入口和 10 个测试方法 Skill。

### 编排入口

| Skill | 用途 |
|---|---|
| [`requirement-to-testcases`](./skills/requirement-to-testcases/SKILL.md) | 从需求生成结构化测试用例，并写入 Markdown 文件。 |

### 测试方法与辅助能力

| Skill | 适用场景 |
|---|---|
| [`spec-static-blackbox-review`](./skills/spec-static-blackbox-review/SKILL.md) | 审查需求、PRD 或接口说明中的歧义、矛盾和缺口。 |
| [`equivalence-and-boundaries`](./skills/equivalence-and-boundaries/SKILL.md) | 设计等价类、边界值、次边界、空值和非法输入。 |
| [`four-quadrant-technique`](./skills/four-quadrant-technique/SKILL.md) | 选择静态/动态、黑盒/白盒等测试方式。 |
| [`risk-based-test-selection`](./skills/risk-based-test-selection/SKILL.md) | 用例过多时按风险选择范围并判断何时停止。 |
| [`exploratory-testing`](./skills/exploratory-testing/SKILL.md) | 需求或文档不足时组织探索性测试。 |
| [`bug-cluster-heuristic`](./skills/bug-cluster-heuristic/SKILL.md) | 根据缺陷聚集区域决定重点测试模块。 |
| [`pesticide-paradox`](./skills/pesticide-paradox/SKILL.md) | 为长期运行的回归测试增加新的数据、路径和测试方法。 |
| [`early-find-cost-curve`](./skills/early-find-cost-curve/SKILL.md) | 组织早期测试和修复的优先顺序，分析测试成本。 |
| [`prove-presence-not-absence`](./skills/prove-presence-not-absence/SKILL.md) | 约束测试结论，识别覆盖不足，避免把通过误写成“没有缺陷”。 |
| [`professional-bug-reporting`](./skills/professional-bug-reporting/SKILL.md) | 将测试发现整理为可复现、可定位、可跟踪的缺陷报告。 |

## 推荐使用方式

### 只生成测试用例

直接调用主入口：

```text
@requirement-to-testcases

根据下面的需求生成完整测试用例，并写入 Markdown 文件：

<粘贴需求、用户故事、PRD 或接口说明>
```

默认输出到：

```text
docs/testcases/TC-<module>-YYYYMMDD.md
```

### 生成全面的测试设计

`requirement-to-testcases` 会根据需要组合其他 Skill。不是每次都需要加载全部方法：

```text
需求 / PRD / 接口说明
        ↓
说明书审查
        ↓
测试类型选择
        ↓
等价类与边界设计
        ↓
状态、异常和权限场景
        ↓
风险取舍
        ↓
历史缺陷与回归盲区补充
        ↓
覆盖缺口和剩余风险检查
        ↓
测试用例 Markdown
```

建议组合关系：

- **主体设计**：`spec-static-blackbox-review`、`four-quadrant-technique`、`equivalence-and-boundaries`、`risk-based-test-selection`。
- **需求不足时**：增加 `exploratory-testing`。
- **已有缺陷或回归历史时**：增加 `bug-cluster-heuristic` 和 `pesticide-paradox`。
- **需要安排测试顺序时**：增加 `early-find-cost-curve`。
- **需要检查结论和覆盖缺口时**：增加 `prove-presence-not-absence`。
- **执行中发现缺陷时**：增加 `professional-bug-reporting`。

这些 Skill 是可组合的方法库，不是必须依次调用的固定脚本。Agent 应根据需求复杂度和测试目标选择必要能力。

## 输出内容

生成的测试设计通常包含：

- 需求理解和一句话测试契约。
- 测试角色、前置条件、输入、规则和副作用。
- 有效类、无效类、空值和默认值。
- 数值、长度、时间、枚举和字符边界。
- 正常流程、异常流程、状态转换和重复操作。
- 权限、兼容性、持久化和恢复场景。
- P0/P1/P2 风险优先级。
- 每条用例的步骤、数据、预期和清理动作。
- 已覆盖、未覆盖、待确认问题和剩余风险。

每条用例应尽量只验证一个主要行为，避免把多个独立问题混在同一条用例中。

## 安装

### Cursor

将 `skills` 目录下的 Skill 子目录复制到项目级或用户级 Skill 目录。

项目级安装，适合团队共享：

```bash
cp -r skills/* /path/to/your-project/.cursor/skills/
```

用户级安装，适用于本机所有项目：

```bash
cp -r skills/* ~/.cursor/skills/
```

Windows PowerShell：

```powershell
Copy-Item -Recurse .\skills\* C:\path\to\your-project\.cursor\skills\
```

### Claude Code

复制到项目的 `.claude/skills/` 目录：

```bash
cp -r skills/* /path/to/your-project/.claude/skills/
```

安装后重新打开 Agent 会话，使工具重新加载 Skill 列表。

## 文件结构

```text
auto_test_cases/
├── README.md
├── LICENSE
└── skills/
    ├── requirement-to-testcases/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── spec-static-blackbox-review/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── equivalence-and-boundaries/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── four-quadrant-technique/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── risk-based-test-selection/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── exploratory-testing/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── bug-cluster-heuristic/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── pesticide-paradox/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── early-find-cost-curve/
    │   ├── SKILL.md
    │   └── test-prompts.json
    ├── prove-presence-not-absence/
    │   ├── SKILL.md
    │   └── test-prompts.json
    └── professional-bug-reporting/
        ├── SKILL.md
        └── test-prompts.json
```

`test-prompts.json` 是对应 Skill 的测试提示词和验证数据，不是运行时依赖。具体使用方式以宿主 Agent 工具的 Skill 加载规则为准。

## 使用边界

这组 Skill 负责测试设计和测试沟通，不会自动连接被测系统，也不会替代实际测试执行：

- 生成测试用例不等于执行测试用例。
- 测试通过不等于证明软件没有缺陷。
- 用例覆盖需求和已识别风险，不代表穷举所有输入、时序、并发和部署环境。
- 需求存在歧义时，应保留“假设”和“待确认问题”，不能将推测写成确定契约。
- 发现缺陷后，应保留最小复现步骤、输入、实际结果、预期结果和环境信息。

## 贡献指南

欢迎提交 Issue 和 Pull Request。适合贡献的内容包括：

- 新的测试设计方法或领域适配。
- 更清晰的测试用例模板和示例。
- Web、API、移动端、游戏、数据处理等场景的测试提示词。
- 中英文内容改进和术语修正。
- 对 `test-prompts.json` 的验证场景补充。

提交前请确认：

- 每个 Skill 目录都包含有效的 `SKILL.md`。
- `SKILL.md` 的 frontmatter 使用小写字母、数字和连字符组成的 `name`。
- 描述同时说明能力和触发场景。
- 文档中的链接和示例路径有效。
- 不在 Skill 中写入 token、密码、生产数据或其他敏感信息。
- 新增方法不会与现有 Skill 的职责重复；如有重叠，请在 README 中说明组合关系。

## 许可证

本项目使用 [MIT License](./LICENSE)。

## English

# Software Testing Skills

A collection of Agent Skills for practical software testing. The skills help agents turn requirements into structured test cases and improve test design with risk analysis, equivalence classes, boundary values, exploratory testing, regression heuristics, and bug reporting.

The collection works with **Cursor, Claude Code, and other tools that support the Agent Skills format**.

### What it includes

- Requirement-to-test-case generation with Markdown output.
- Specification review for ambiguity and missing rules.
- Equivalence-class, boundary, and sub-boundary design.
- Static/dynamic and black-box/white-box test selection.
- Risk-based test selection and stop-testing decisions.
- Exploratory testing when documentation is incomplete.
- Bug-cluster and regression-suite analysis.
- Coverage-gap and test-conclusion checks.
- Reproducible professional bug-report structure.

### Skill collection

This repository contains **11 skills**: one orchestration entry point and ten testing-method skills.

The primary entry point is [`requirement-to-testcases`](./skills/requirement-to-testcases/SKILL.md):

```text
Requirement / PRD / API specification
        ↓
Test design analysis
        ↓
Equivalence classes and boundaries
        ↓
State, exception, and permission scenarios
        ↓
Risk prioritization
        ↓
Markdown test-case document
```

The other skills are composable methods. They are loaded when useful; they are not required to run in a fixed order for every request.

### Quick start

```text
@requirement-to-testcases

Generate comprehensive test cases from the following requirement
and write them to a Markdown file:

<paste your requirement here>
```

The default output path is:

```text
docs/testcases/TC-<module>-YYYYMMDD.md
```

### Installation

Copy the skill directories under `skills/` into your project-level or user-level skills directory:

```bash
# Project-level
cp -r skills/* /path/to/your-project/.cursor/skills/

# User-level
cp -r skills/* ~/.cursor/skills/
```

For Claude Code, use `.claude/skills/` instead of `.cursor/skills/`.

Restart the Agent session after installation so the host can reload the skills.

### Scope and limitations

These files provide test-design knowledge and workflows. They do not connect to a system under test or replace test execution. Generated test cases cover the supplied requirements and identified risks; they do not prove the absence of defects or exhaust every possible input, timing, concurrency, and deployment combination.

### License

MIT. See [`LICENSE`](./LICENSE).
