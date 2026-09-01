---
name: equivalence-and-boundaries
description: |
  当用户要为输入/表单/API 字段设计用例，提到“等价类 / 边界值 / 次边界 / 2的幂 /
  equivalence class / boundary / sub-boundary / 空值无效数据”时调用：分类抽样并优先取边界双侧与内部边界。
  不适用于：全局何时停测（risk-based-test-selection）、黑白盒选型（four-quadrant-technique）、
  回归免疫换手法（pesticide-paradox）、或缺陷成群加码（bug-cluster-heuristic）。
tags: [equivalence-class, boundary, sub-boundary, test-design]
related_skills:
  - four-quadrant-technique
  - risk-based-test-selection
  - pesticide-paradox
  - bug-cluster-heuristic
  - prove-presence-not-absence
---

# 等价类与边界（含次边界）

## R — 核心原则 (Reading)

> 一个等价类或者等价划分是指测试相同目标或者暴露相同软件缺陷的一组测试用例。……如果软件能在其边界运行，那么在正常情况下就应该不会有什么问题。……有些边界在软件内部……称为次边界条件……2的幂和ASCII表是这方面的两个例子。

---

## I — 方法论骨架 (Interpretation)

把相似输入/输出/操作归为等价类：同类暴露同类缺陷，故每类测代表即可，用分类代替穷举。

优先从类中抽边界代表——悬崖能走稳则平地无忧。取点习惯：合法区内最后一点 + 刚越界的无效点（以及邻近有效点）。

再补三类常被漏的划分：次边界（2 的幂、ASCII/字符缝等实现习惯陷阱）、默认/空白/空值/零、垃圾/非法数据。先通过性确认基本能工作，再蓄意失效性施压。

过度划分会漏掉能暴露缺陷的用例；划分带主观性，需审查是否“足以覆盖测试对象”。

---

## A1 — 典型应用 (Past Application)

### 案例 1: 计算器与 1+max
- **问题**: 加法穷尽不可能，1+1…1+6 与 1+最大数是否同类？
- **做法**: 用等价类区分普通加法与极值/边界类，教会“选有关、弃无关”。
- **结论**: 有选择时优先边界类而非又一个普通整数。
- **结果**: 把风险抽样落到可操作技法。

### 案例 2: BASIC 数组 Dim data(10) 与画图空白宽高
- **问题**: 边界 off-by-one；清空默认后按 OK。
- **做法**: 演示边界最易错；空白/空值单独成类。
- **结论**: 边界内外点 + 空输入必须进划分；缓冲区溢出常源于边界缺陷。
- **结果**: 提供可复用的取点清单模板。

---

## A2 — 触发场景 (Future Trigger) ★

### 用户会在什么情境下需要这个 skill?

1. 表单/配置项/API 参数要写测试数据，不知取哪些值。
2. 长度、数值范围、枚举字符集已定，要边界与非法集。
3. 怀疑实现有字节/字/ASCII 缝，说明书未写内部边界。
4. 评审别人用例是否只测了“中间快乐路径”。

### 语言信号 (用户的话里出现这些就应激活)

- "等价类 / equivalence class / 等价划分"
- "边界值 / boundary / 次边界 / sub-boundary"
- "2的幂 / ASCII / 空值 / 无效数据"
- "这个字段怎么选测试数据"

### 与相邻 skill 的区分

- 与 `four-quadrant-technique` 的区别: 本 skill 默认已在动态黑盒（或带次边界思维）内设计数据；彼 skill 先选象限。
- 与 `risk-based-test-selection` 的区别: 本 skill 压缩单个输入空间；彼 skill 在项目级决定测哪些面、何时停。
- 与 `pesticide-paradox` 的区别: 换手法时可新划等价类，但免疫诊断不是本 skill 主职。

---

## E — 可执行步骤 (Execution)

当 skill 被激活后, agent 应按以下步骤执行:

1. **识别变量与规格边界**
   - 列出每个输入的合法域、非法域、默认值。
   - 完成标准: 变量表含类型、范围/集合、是否可空。

2. **划分等价类**
   - 合法/非法/空/默认；必要时按输出或操作再分。
   - 完成标准: 每类有一句话“为何同类暴露同类缺陷”。
   - 判停条件: 若变量爆炸到项目级组合，先用 `risk-based-test-selection` 砍维。

3. **选代表：边界双侧 + 次边界**
   - 对数值/长度：min-1, min, min+1, max-1, max, max+1（按域调整）；补 2 的幂邻近、ASCII 缝、空提交。
   - 完成标准: 高风险变量有边界双侧点；说明次边界来源（与程序员确认更佳）。

4. **排序执行**
   - 先通过性（简单合法），再失效性（越界/垃圾）。
   - 完成标准: 用例列表标注 pass/fail 意图，避免一上来极限掩盖基础失效。

---

## B — 边界 (Boundary) ★

### 不要在以下情况使用此 skill

- 用户问的是黑盒/白盒/静动态该选哪个。
- 用户在做发布话术或“证明没 bug”。
- 状态机/时序是主问题而数据域很简单——需状态转换测试（相关另一技法），不要硬套等价类。

### 常见失败模式

- 过度划分等价类，漏掉可能暴露缺陷的测试；新手应请有经验者审查。
- 只测边界线上的点不够，还要邻近有效与刚越界无效。
- 说明书与程序员常忽略空白/空输入。

### 适用边界与局限

- 常见示例多来自桌面 UI（画图、写字板、文件名字符）与早期本地化/ASCII；今日需映射到 Unicode、JSON schema、protobuf 枚举、云配额等，但“分类 + 边界 + 内部习惯缝”骨架仍有效。
- 次边界依赖“大体了解工作方式”；纯黑盒远程 API 时要主动问实现者或读协议。

### 容易混淆的邻近方法论

- 风险导向选测（何时停/测哪块）
- 四象限选型、探索测试、杀虫剂换手法

---

## 相关 skills

- depends-on: 常在 four-quadrant 选中动态黑盒之后
- contrasts-with: risk-based-test-selection, four-quadrant-technique
- composes-with: bug-cluster-heuristic, pesticide-paradox

---

## 审计信息

- **测试用例**: 见 test-prompts.json
- **更新时间**: 2026-07-29
