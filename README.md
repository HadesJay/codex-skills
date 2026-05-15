# codex-skills

本仓库包含一套面向航空嵌入式软件开发的 Markdown 文档模板和对应的 Codex Skill。

## 内容

- `templates/aviation-embedded/`：可直接复制到项目中的 Markdown 模板集合。
- `aviation-embedded-docs/`：Codex Skill 源目录，用于生成、补全、审查 DO-178C 风格文档。

## 默认定位

- 文档语言：中文为主，保留 DO-178C、DAL、HLR、LLR、MC/DC 等英文术语。
- 严谨度：DAL B/C 风格。
- 当前平台：Linux 嵌入式。
- 扩展方向：RTOS 或裸机平台迁移。
- 用途：工程开发、内部审查、需求追踪、测试证据准备。

## 模板集合

| 文件 | 用途 |
|---|---|
| `00_project_plan.md` | 项目计划、生命周期、角色、基线和审查策略 |
| `01_coding_standard.md` | C/C++/Linux 嵌入式代码标准和 RTOS 迁移规则 |
| `02_system_requirements.md` | 系统需求 |
| `03_high_level_requirements.md` | 高层软件需求 HLR |
| `04_low_level_requirements.md` | 低层软件需求 LLR |
| `05_architecture_design.md` | 软件架构设计 |
| `06_detailed_design.md` | 模块详细设计 |
| `07_code_review.md` | 代码审查记录 |
| `08_unit_test.md` | 单元测试计划、用例和结果 |
| `09_integration_test.md` | 集成测试计划、环境和结果 |
| `10_verification_summary.md` | 验证总结和覆盖率 |
| `11_traceability_matrix.md` | 端到端追踪矩阵 |
| `12_change_impact_analysis.md` | 变更影响分析 |

## ID 规则

| 对象 | ID 格式 |
|---|---|
| 系统需求 | `SYS-REQ-0001` |
| 高层需求 | `HLR-0001` |
| 低层需求 | `LLR-0001` |
| 设计元素 | `DES-0001` |
| 代码单元 | `CODE-<module>-0001` |
| 单元测试 | `UT-0001` |
| 集成测试 | `IT-0001` |
| 问题/偏差 | `ISSUE-0001` |
| 变更 | `CHG-0001` |

## 使用方式

将 `templates/aviation-embedded/` 复制到具体项目的文档目录，按项目模块逐步填写。需求、设计、代码和测试发生变化时，同步更新 `11_traceability_matrix.md` 和 `12_change_impact_analysis.md`。

如果要让 Codex 自动发现该 Skill，可将 `aviation-embedded-docs/` 复制到 `C:\Users\xxx\.codex\skills\`。
