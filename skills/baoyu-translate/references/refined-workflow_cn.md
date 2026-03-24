# 翻译工作流详解

本文说明各工作流步骤的细则。步骤在不同模式间共用：

- **Quick**：仅翻译（不使用本文件的步骤）
- **Normal**：步骤 1（分析）→ 翻译
- **Refined**：步骤 1（分析）→ 步骤 2（草稿）→ 步骤 3（审校）→ 步骤 4（修订）→ 步骤 5（润色）
- **Normal → Upgrade**：Normal 完成后，用户可继续步骤 3 → 步骤 4 → 步骤 5

所有中间结果均以文件形式保存在输出目录。

## 步骤 1：内容分析

翻译前深入分析源材料。将分析保存到输出目录中的 `01-analysis.md`。聚焦直接影响翻译质量的维度。

### 1.1 简要摘要

3–5 句话概括：
- 内容讲什么？
- 核心论点是什么？
- 最有价值的点是什么？

### 1.2 核心内容

- **核心论点**：一句话概括
- **关键概念**：作者使用哪些关键概念？如何定义？
- **结构**：论证如何展开？章节如何衔接？
- **论据**：用了哪些具体例子、数据或权威引用？

### 1.3 背景语境

- **作者**：是谁？背景与立场？
- **写作语境**：回应何种现象、趋势或争论？
- **目的**：作者想解决什么问题？想影响谁？
- **隐含假设**：论证背后有哪些未明说的前提？

### 1.4 术语提取

- 列出所有技术术语、专有名词、品牌名、缩写
- 与已加载术语表交叉核对
- 术语表中未收录的，查证标准译法
- 在工作用语对照表中记录定稿

### 1.5 语气与风格

- 原文偏正式还是口语？
- 是否使用幽默、隐喻或文化典故？
- 针对目标受众，译本应采用何种语域？

### 1.6 读者理解难点

标出目标读者可能卡住的地方，并按受众校准：

- **领域行话**：缺乏通行译法或直译无意义的术语
- **文化典故**：习语、历史事件、流行文化、源文化特有的社会规范
- **隐含知识**：原文作者默认具备、目标读者可能缺失的背景
- **文字游戏与隐喻**：难以跨语言传递的形象表达
- **命名概念**：有固定名称的理论、效应或现象（如 “comb-over effect”、“Dunning-Kruger effect”）
- **认知落差**：反直觉的论断或预期与现实，需为目标读者重新框定

对每个难点记录：
1. 原文用语/片段
2. 为何可能困扰目标读者
3. 可作译者注的简明通俗说明

### 1.7 形象表达与隐喻映射

找出源文中所有隐喻、明喻、习语与形象说法。对每一项记录：

1. **原文表达**：确切短语
2. **意图含义**：作者实际要传达什么（画面背后的意思）
3. **直译风险**：逐字译是否不自然、丢失内涵或让读者困惑？
4. **目标语策略**，任选其一：
   - **释义**：放弃源语画面，用自然的目标语直接表达意图
   - **替换**：换用目标语习语或画面，传达相同观点与情绪效果
   - **保留**：若画面在目标语中同样成立，可保留原意象

另需标注：
- **用词带来的情感色彩**：如 “alarming” 等带主观感受的词——记下情感效果以便保留
- **言外之意**：字面简单但含义更丰富——记下作者真实意图，便于译者传达完整用意

### 1.8 结构与创意难点

- 需为目标语自然流畅而重组的复杂句（长从句、多层修饰、分词结构等）
- 结构性难点（双关、歧义、无法直译的文字游戏）
- 需创意改编才能保留作者声口或幽默感的内容

**保存 `01-analysis.md`**，结构如下：
```
## Quick Summary
[3-5 sentences]

## Core Content
Core argument: [one sentence]
Key concepts: [list]
Structure: [outline]

## Background Context
Author: [who, background, stance]
Writing context: [what this responds to]
Purpose: [goal and target audience]
Implicit assumptions: [unstated premises]

## Terminology
[term → translation, ...]

## Tone & Style
[assessment]

## Comprehension Challenges
- [term/passage] → [why confusing] → [proposed note]
- ...

## Figurative Language & Metaphor Mapping
- [original expression] → [intended meaning] → [approach: interpret/substitute/retain] → [suggested rendering]
- ...

## Structural & Creative Challenges
[sentence restructuring needs, wordplay, creative adaptation needs]
```

## 步骤 2：组装翻译提示

主 agent 读取 `01-analysis.md`，用 [references/subagent-prompt-template.md](subagent-prompt-template.md) 组装完整翻译提示。将解析后的风格预设（来自 `--style`、EXTEND.md 的 `style` 或默认 `storytelling`）、内容背景、合并后的术语表与理解难点写入提示。保存为 `02-prompt.md`。

该提示由子 agent（分块时）或主 agent 自身（不分块时）使用。

## 步骤 3：初稿

保存到输出目录中的 `03-draft.md`。

分块内容由子 agent 产出初稿（合并各块译文）。不分块时由主 agent 直接产出。

按 `02-prompt.md` 翻译全文。除 SKILL.md 步骤 4 中的**翻译原则**外，还须遵守本步骤要求：

- 一致使用步骤 1 的术语定稿
- 贴合已识别的语气与语域
- 形象表达按步骤 1 的隐喻映射处理
- 对步骤 1 标出的理解难点补充译者注

## 步骤 4：批判性审校

主 agent 对照原文批判性审阅初稿。将审阅结论保存到 `04-critique.md`。本步骤**仅做诊断**——尚不改写。

### 4.1 准确与完整
- 逐段、逐句对照原文
- 核对事实、数字、日期与专有名词
- 标出误增、误删或误改
- 检查术语是否与术语表全文一致
- 确认无漏段、漏节

### 4.2 翻译腔诊断（针对 CJK 目标语）
- **多余连接词**：上下文已能体现关系时仍滥用 因此/然而/此外/另外
- **滥用被动**：本可用主动却大量 被/由/受到
- **名词堆砌**：本应拆成短句的长定语链
- **分裂句**：机械套用英文 “It is...that” 的不自然 “是...的” 结构
- **过度名词化**：本可用动词、形容词处堆抽象名词（如 “进行了讨论” → “讨论了”）
- **代词别扭**：本可省略却滥用 他/她/它/我们/你

### 4.3 形象表达与情感忠实度
- 对照 `01-analysis.md` 中的隐喻映射：已标注的隐喻/习语是否按建议策略（释义/替换/保留）处理？
- 标出直译后不自然或丢失本意的隐喻与形象说法
- 检查情感色彩：源文带主观色彩的词（如 “alarming”、“haunting”、“striking”）在译文中是否仍引发相近反应，还是被压平为中性客观描述？
- 标出丢失的言外之意：译者过于贴字面而未传达作者深层意图的句子

### 4.4 策略执行
- `02-prompt.md` 中的翻译策略是否落实？
- 是否采用分析中识别的语气与语域？
- `01-analysis.md` 中的理解难点是否以合适注释处理？
- 术语表是否一致使用？

### 4.5 表达与逻辑
- 标出「翻译腔」句子——语序别扭、硬套、僵硬
- 检查句间、段间逻辑衔接
- 指出可通过重组提升可读性的位置
- 指出未用地道目标语习语之处

### 4.6 译者注质量
- 注释是否准确、简明、真正有用？
- 哪些理解难点漏注？
- 对目标受众显而易见的词是否过度注释？
- 文化典故是否在需要处得到解释？

### 4.7 文化适配
- 隐喻与习语在目标语中是否成立？
- 是否有在目标文化中易误解或冒犯的说法？
- 是否因文化语境差异可能被误读的段落？

**保存 `04-critique.md`**，结构如下：
```
## Accuracy & Completeness
- [issue]: [location] — [description]
- ...

## Europeanized Language Issues
- [issue type]: [example from draft] → [suggested fix]
- ...

## Figurative Language & Emotional Fidelity
- [literal metaphor]: [original] → [draft rendering] → [suggested interpretation]
- [flattened emotion]: [original word/phrase] → [draft rendering] → [how to restore emotional effect]
- ...

## Strategy Execution
- [strategy]: [followed/missed] — [details]
- ...

## Expression & Logic
- [location]: [problem] → [suggestion]
- ...

## Translator's Notes
- [add/remove/revise]: [term] — [reason]
- ...

## Cultural Adaptation
- [issue]: [description] — [suggestion]
- ...

## Summary
[Overall assessment: X critical issues, Y improvements, Z minor suggestions]
```

## 步骤 5：修订

根据 `04-critique.md` 的全部结论产出修订稿。保存为 `05-revision.md`。

修订时阅读 `03-draft.md`（初稿）与 `04-critique.md`（审校结论），并可回溯原文与 `01-analysis.md`：

- 修正审校标出的所有准确性问题
- 将翻译腔改写为自然的目标语表达
- 按隐喻映射重新处理直译的隐喻与形象说法；换用自然译文以传达原意与情感效果
- 恢复被压平的情感色彩：带主观感受的词应引发与原文相近的反应
- 补做遗漏的翻译策略
- 重组僵硬、别扭的句子以提升流畅度
- 按审校建议增删改译者注
- 改善段间过渡
- 按建议处理文化典故

## 步骤 6：润色

终稿保存为 `translation.md`。

对 `05-revision.md` 做发布前最后一遍：

- 通读译文作为独立文本——是否像母语写作般连贯？
- 磨平段间仍显粗糙的过渡
- 确保叙事声音全文一致
- 一致贯彻所选翻译风格：storytelling 应如叙事般流动，formal 保持中性专业，humorous 让笑点自然落在目标语中，等等
- 最后扫一遍残留直译隐喻或情感扁平：仍显「译出来」而非「写出来」的形象表达，应改写成自然目标语
- 全文术语最后再核对一遍
- 确认格式保留正确（标题、加粗、链接、代码块）
- 清除残余翻译腔

## 子 agent 职责

每个子 agent（每块一个）**仅**负责其对应块的初稿（步骤 3）。主 agent 组装共用提示（步骤 2）、并行拉起所有子 agent，随后接手批判性审校（步骤 4）、修订（步骤 5）与润色（步骤 6）。主 agent 可自行决定是否将修订或润色再委派给子 agent。

## 分块 Refined 翻译

当内容超过分块阈值（见 SKILL.md 中的 Defaults）且使用 refined 模式时：

1. 主 agent 先对**整篇**文档执行分析（步骤 1）→ `01-analysis.md`
2. 主 agent 组装翻译提示 → `02-prompt.md`
3. 拆分为块 → `chunks/`
4. 每块并行拉起一个子 agent（各块均读 `02-prompt.md` 获取共用上下文）→ 合并为 `03-draft.md`
5. 主 agent 批判性审校合并稿 → `04-critique.md`
6. 主 agent 根据审校修订 → `05-revision.md`
7. 主 agent 润色 → `translation.md`
8. 跨块一致性终检：
   - 检查块边界的术语一致
   - 核对块间叙事衔接
   - 修复块边界过渡问题
