# 按编码标准的代码审查（详细清单）

| 字段 | 内容 |
|---|---|
| 关联编码标准 | [01_coding_standard.md](01_coding_standard.md)（版本须与本表「标准版本」一致） |
| 标准版本 | TBD（与 `01` 元表「版本」一致） |
| 变更/MR | TBD |
| 审查提交 SHA | TBD |
| 审查日期 | TBD |
| 审查人 | TBD |
| 状态 | Draft |

## 0. 使用说明

- 本文件用于**对照** [01_coding_standard.md](01_coding_standard.md) 中的 `CS-*` 规则，对**实际源代码**做逐项核对；结论、严重问题与关闭证据的**汇总表**仍写入 [07_code_review.md](07_code_review.md)。
- **结论列**统一使用：`Pass`（已核对无违规）、`Fail`（发现违规或证据不足）、`NA`（本变更不涉及该条）、`Blocked`（缺少 diff/工具报告/仓库访问，无法核对）。
- **证据列**填写可复查定位：`path/to/file.c:行号`、MR 评论链接、CI 构建号、静态分析告警 ID，或已批准的 `Deviation-*`。
- 审查人（含 AI 辅助）须**基于真实仓库内容**填写；不得在无源码或报告的情况下将 `Fail` 改为 `Pass`。

## 1. 审查输入（工具与人工共用）

| 输入项 | 位置/说明 | 已具备 |
|---|---|---|
| 待审 diff 或 MR | TBD | Yes/No |
| 完整编译日志（含告警） | TBD | Yes/No |
| 静态分析/MISRA 报告 | TBD | Yes/No |
| 单元测试/覆盖率摘要（若本次变更声称覆盖 LLR） | TBD | Yes/No |
| 格式化/编码风格检查（如 clang-format） | TBD | Yes/No |

## 2. 通用规则 `CS-GEN-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-GEN-0001 | 无未记录的编译器扩展 | 查 `__attribute__`、`#pragma`、内建 intrinsic、GNU/C11 仅扩展；对照构建脚本与 README | 静默依赖 `-gnu11` 或内建原子 | TBD | TBD |
| CS-GEN-0002 | 对外 API 错误码与恢复策略一致 | 读公开头文件与实现：返回值/错误码枚举、文档注释、调用方处理 | 返回 `void` 却可能失败；错误码未文档化 | TBD | TBD |
| CS-GEN-0003 | 无未初始化读、未检查返回值、资源泄漏 | 跟踪 `malloc/open/socket/pthread_*` 等配对；搜索 `(void)` 强转忽略返回值 | `read`/`write` 未检查；`close` 路径缺失 | TBD | TBD |
| CS-GEN-0004 | 共享可变状态有同步依据 | 搜 `static` 全局、`volatile`、双端队列；对照锁/原子 API | 多线程读写无锁；用 `volatile` 当锁 | TBD | TBD |
| CS-GEN-0005 | 偏离已挂 deviation | 对照项目 deviation 表；违规处注释含规则 ID | pragma 压告警无 deviation | TBD | TBD |

## 3. 命名与格式 `CS-FMT-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-FMT-0001 | `snake_case` 命名 | 目检新增符号；可选风格检查工具 | `camelCase` 公有 API | TBD | TBD |
| CS-FMT-0002 | 宏与枚举常量大写 | 搜 `#define`、枚举成员 | 宏小写 | TBD | TBD |
| CS-FMT-0003 | 类型名后缀一致 | 搜 `typedef struct`、公开类型 | 混用 `_Type` / `_t` | TBD | TBD |
| CS-FMT-0004 | 模块内辅助函数 `static` | 搜本文件非 static 函数是否仅本 TU 使用 | 误导出内部函数 | TBD | TBD |
| CS-FMT-0005 | 空格缩进与行宽 | `git diff` 可见 Tab；长行统计 | Tab 混入；超长行 | TBD | TBD |
| CS-FMT-0006 | 控制流必带花括号 | 搜 `if/for/while` 后单行无 `{}` | `if (x) return;` 无括号 | TBD | TBD |

## 4. C99 语言与类型 `CS-C99-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-C99-0001 | 硬件/协议宽度用 `stdint` | 搜协议结构体中的 `int`/`long` | 网络字段用 `int` | TBD | TBD |
| CS-C99-0002 | 指针契约在头文件或注释中 | 读 `.h` 中 `/** @param` 或等价；是否允许 NULL | 未说明所有权 | TBD | TBD |
| CS-C99-0003 | 无危险窄化与混号比较 | 搜隐式赋值、`size_t` 与 `int` 比较 | 有符号无符号混比无边界处理 | TBD | TBD |
| CS-C99-0004 | 无魔法数 | 搜裸字面量于协议/寄存器/超时 | `if (len > 4096)` 无命名常量 | TBD | TBD |
| CS-C99-0005 | 全局状态最小且有说明 | 搜非 const 全局；读初始化与测试复位 | 隐式跨模块顺序依赖 | TBD | TBD |
| CS-C99-0006 | `volatile` 用途正确 | 搜 `volatile`；上下文是否 MMIO/ISR 共享 | 用 volatile 代替 mutex | TBD | TBD |
| CS-C99-0007 | 安全路径动态内存 | 搜 `malloc/realloc`；是否有上界与失败分支 | 热路径无界分配 | TBD | TBD |

## 5. 函数与控制流 `CS-CTL-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-CTL-0001 | 单一职责与复杂度可控 | 读函数体行数与分支；工具圈复杂度可选 | 巨型函数 | TBD | TBD |
| CS-CTL-0002 | 公开 API 校验参数 | 每个 `extern` 函数入口 | 未判 NULL/范围 | TBD | TBD |
| CS-CTL-0003 | 返回值被处理或显式忽略 | 搜调用点；项目约定宏如 `(void)` 须有注释 | 静默吞错 | TBD | TBD |
| CS-CTL-0004 | `switch` 覆盖枚举 | 对照枚举定义与 `case`；`default` 注释 | 新增枚举未加 case | TBD | TBD |
| CS-CTL-0005 | `goto` 仅用于清理 | 读所有 `goto` 目标与跨域 | 跨多层逻辑跳转 | TBD | TBD |
| CS-CTL-0006 | 无未批准递归 | 搜直接/间接递归 | 深度未知递归 | TBD | TBD |
| CS-CTL-0007 | ISR/回调无阻塞与重入问题 | 读 signal/IRQ 注册函数体 | `malloc`/日志/锁 在 ISR | TBD | TBD |

## 6. 协议与内存布局 `CS-MEM-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-MEM-0001 | 字节序显式 | 搜序列化函数、`htons`/`be32` 等或项目封装 | 裸 `memcpy` 结构体跨端 | TBD | TBD |
| CS-MEM-0002 | 对齐与 packed 有依据 | 读 `struct` 与 `#pragma pack`/`__packed` | 隐式依赖编译器布局 | TBD | TBD |
| CS-MEM-0003 | 缓冲区带长度 | 搜 `strcpy/sprintf/gets`；长度是否来自调用方 | 无界拷贝 | TBD | TBD |
| CS-MEM-0004 | 位域不用于可移植协议/寄存器映射 | 搜 `:` 位域于硬件/网络结构 | 协议解析用位域 | TBD | TBD |

## 7. Linux 嵌入式 `CS-LNX-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-LNX-0001 | 系统调用等返回值检查 | 搜 `read/write/ioctl/pthread_*` 调用点 | 忽略 `EINTR`/`errno` | TBD | TBD |
| CS-LNX-0002 | 设备/sysfs ABI 有注释 | 读 `open` 路径与内核版本假设 | 硬编码节点无说明 | TBD | TBD |
| CS-LNX-0003 | 日志无敏感且非实时热路径 | 搜 `printf`/日志宏于 tight loop | 密钥打印 | TBD | TBD |
| CS-LNX-0004 | 线程属性可追溯 | 搜 `pthread_attr_*`、`sched_set*` | 优先级魔法数无注释 | TBD | TBD |
| CS-LNX-0005 | 资源有成对释放 | 搜 `mmap/munmap`、`open/close`、锁配对 | 错误分支泄漏 fd | TBD | TBD |

## 8. RTOS/裸机迁移 `CS-RTOS-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-RTOS-0001 | 动态内存有界或可替换 | 同 CS-C99-0007；RTOS 路径单独标 | ISR 中分配 | TBD | TBD |
| CS-RTOS-0002 | 最坏路径有记录 | 读设计注释或迁移小节 | 无 WCET/队列深度依据 | TBD | TBD |
| CS-RTOS-0003 | 寄存器访问封装 | 搜裸指针写 `0x` 地址 | 散落魔术寄存器写 | TBD | TBD |
| CS-RTOS-0004 | 栈/队列/定时器有上限 | 读常量配置与注释 | 无水位监控 | TBD | TBD |
| CS-RTOS-0005 | ISR 与任务同步正确 | 读临界区、关中断、RTOS API | 双端无保护访问 | TBD | TBD |

## 9. 工具链 `CS-TOOL-*`

| ID | 审查要点 | 在源码中如何核对 | 典型违规 | 结论 | 证据 |
|---|---|---|---|---|---|
| CS-TOOL-0001 | 告警清零策略 | 读 CI 日志与 `CMakeLists`/Makefile 告警选项 | 未开 `-Wall` 或大量未解释豁免 | TBD | TBD |
| CS-TOOL-0002 | MISRA/静态分析在 CI | 对报告编号与代码行 | 未跑或仅本地口头通过 | TBD | TBD |
| CS-TOOL-0003 | 工具链版本冻结 | 对照 [00_project_plan.md](00_project_plan.md) 基线 | 审查用编译器与 CI 不一致 | TBD | TBD |
| CS-TOOL-0004 | 第三方代码可追踪 | 搜 `vendor/`、`third_party` 头；许可证文件 | 拷贝代码无版本与许可 | TBD | TBD |

## 10. 违规汇总（可复制到 [07_code_review.md](07_code_review.md) 第 3 节）

| 临时 ID | CS 规则 ID | 严重度 | 问题摘要 | 代码定位 | 建议修复 | 偏差 ID |
|---|---|---|---|---|---|---|
| CR-001 | TBD | Major/Minor/TBD | TBD | `file.c:line` | TBD | N/A 或 Deviation-* |

## 11. 与轻量审查表的关系

- 本节结论汇总后，将**审查结论、Open 问题数量、批准状态**写入 [07_code_review.md](07_code_review.md) 第 4 节；将第 10 节行项目同步为 [07_code_review.md](07_code_review.md) 第 3 节 `ISSUE-*`（或建立 `ISSUE-*` ↔ `CR-*` 映射表）。
- 若本次变更仅触及脚本/文档，可在第 2–9 节对应行填 `NA`，并在 [07_code_review.md](07_code_review.md) 说明范围。

## 12. 审查签出

- 所有 `Fail` 已转为 `Pass`（已修复）或已关联有效 `Deviation-*`，或已拆分为后续变更跟踪。
- 工具报告与本次 SHA 已归档路径：TBD
- 审查人签字/日期：TBD
