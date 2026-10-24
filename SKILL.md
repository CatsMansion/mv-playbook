---
name: mv-playbook
slug: mv-playbook
displayName: "MV Playbook (MV 知识库入口·累积规则)"
category: video
version: 1.0.0
description: "The entry point to the accumulated MV skill family: a map of which skill governs what, plus the cross-cutting hard rules that were paid for with real mistakes (a parameter tweak invisible on 8-bit footage, a metric poisoned by grain, per-phrase subtitle reveals inviting sync-checking, an unreliable lyric timeline, a metric that answers the wrong question). Use this before starting any MV task, when unsure which skill applies, or when adding a new lesson and deciding where it belongs."
description_zh: "MV 技能家族的入口：哪条线归哪个技能管，加上跨项目通用的硬教训（8bit 素材上看不见的参数微调、被颗粒污染的指标、逐段出字等于请观众核对、不可信的歌词时间轴、问错问题的判据）。开工前先读，不确定该用哪个技能时读，或者学到新东西要决定写进哪里时读。"
agent_created: true
---

# MV Playbook · 知识库入口

## 1. 索引：哪条线归谁

| 什么时候用 | 用哪个 skill |
|---|---|
| 从零做一支 MV，要流水线 | `mv-code-driven-pipeline` |
| **整片结构怎么组织**（几族镜头 / 文字几段 / 世界连不连） | **`mv-pv-arrangement`** |
| 日文字幕断句 / 出字时刻 / 排版参数 | `mv-jp-lyrics-subtitle` |
| 审一支成片 / 反解参考片 | `mv-shot-audit` |
| 角色跨帧/跨姿势保持一致 | `consistent-character-mv-anchor` |
| 派单给美术端 / 裁决回传 | `pet-asset-pipeline` |
| 交付一个客户自己能改的工程 | `blender-editable-timeline` |
| 方法论随笔 | `mv-methodology-notes`（GitHub） |

---

## 2. ⛔ 铁律（每条都是花钱或返工换来的）

### 铁律 1：参数微调在 8bit 素材上肉眼不可见
量化步长 = 1/base（base 200/220/240 ⇒ 0.005/0.0045/0.00417）。
τ5 vs τ6 只差 **1 个量化级**；peak 1.06 只抬 1–2 级。
⇒ **任何"提升观感"的改动，先问：这一改动在成片上肉眼可见吗？** 不可见 ⇒ 不做。
观感问题改**画面层**（构图/运镜/景深/光影/调色/字幕系统/事件密度），⛔ 不是改参数。

### 铁律 2：判据只管"做没做对"，不管"好不好看"
9 条参数判据全绿 ≠ 片子好看。
⛔ 不许拿判据通过当审美达标的证据。审美只能由人目检。

### 铁律 3：指标会被污染，先问"它测的是我以为的东西吗"
- **拉普拉斯方差判糊不糊**：颗粒（σ2.8）会把方差抬高 ⇒ 有颗粒的画面**不能用它判清晰度**。
- **整体 bbox 量角色高**：漂浮的装饰件会拉大 bbox ⇒ 必须用**最大连通域**。
- **能量峰数音节**：一个长音里的颤音产生多个峰 ⇒ 16 个音节数出 33 个峰。
⇒ 换判据前先做**阳性对照**：拿一个已知异常的样本，判据必须命中。

### 铁律 4：动手前先 grep 项目里的既定规格
字号、色板、线宽、有无底板——这些往往**早就裁定过**，躺在 `02_文档/` 或 `docs/` 里。

```bash
grep -rn -E "字幕|字体|排版|几何|字高|描边|底板|色板" docs/ 02_文档/ | head -40
```
⛔ 不要"我觉得这样更好看"就自己定一套 —— 一定和已裁定规格冲突，返工成本是整片重渲。

### 铁律 5：素材默认可无限重复用
⛔ 不要给素材/效果设配额（"最多用 1 次""用过的置灰"）。
约束要落在**观感**上（crop / 镜像 / 合成模式 / 视差），不是**素材计数**上。
另外：**"某个转场用了两次" ≠ 单调**。单调的真根因是「各段变化方式像同一个模子」。

### 铁律 6：字幕不要用「逐段出字」
逐段弹出 = 请观众核对每个字的时刻。时间戳有 0.2s 误差就暴露。
⇒ 整句一次性出（断句靠版面表达），或者用「唱到时强调」代替。

### 铁律 7：歌词时间轴文件多半不可信
`lyrics.ts` / SRT / LRC 的 `start`/`end` 常常是**按行数线性切**或手工估的。
⇒ 先用**人声换气间隔**（静音段）验证每句的真实起止，再谈句内断句。
实测案例：某句标 19.50 起，真起点 **23.63**，早 4.1 秒。

### 铁律 8：agent 起的进程活不过 turn
要想让用户看到软件，只能给他一个**双击启动器**（`.cmd`，CRLF，用字节写）。
⛔ 不要用 `printf` 拼 Windows 路径（`\U`、`\b` 会被当转义吃掉）。

### 铁律 9：中文路径一律 `np.fromfile` + `cv2.imdecode`
`cv2.imread` 读中文路径会静默返回 None。

### 铁律 10：派单里的路径要逐条核验
"我以为路径对" 和 "路径真的对" 是两件事。今天就是这么发现同一个错路径出现在两份派单里的。

### 铁律 11：宁可写「有 N 项待修」，不写「基本完成」
⛔ "基本完成"是自欺。列出不合格项，逐项给实测值。

---

## 3. 累积流程（学到新东西往哪写）

每学到一条，走这三步：

1. **归位**：它属于哪个 skill？
   - 结构/编排 → `mv-pv-arrangement`
   - 字幕/文字 → `mv-jp-lyrics-subtitle`
   - 一致性/素材 → `consistent-character-mv-anchor`
   - 交付/工程 → `blender-editable-timeline`
   - 派单/裁决 → `pet-asset-pipeline`
   - **跨线通用** → 本文件 §2 铁律
2. **变成可执行**：写成「症状 → 根因 → 修法 → 实测数字」。
   ⛔ 不写「注意 XX 可能有问题」这种没用的提醒。
3. **进反面清单**：凡是我试过且失败的，写进对应 skill 的「别再试」区。
   失败的经验比成功的更值钱，因为它能省掉下一次的重复踩。

### 判断标准：一条好条目长什么样

| 差 | 好 |
|---|---|
| 「注意通道可能冲突」 | 「给 ch3 赋值时 char 被静默推到 89..177 帧，整段消失」 |
| 「字幕可能不同步」 | 「句标 19.50 起，实测 23.63，早 4.1 秒」 |
| 「颗粒会影响判据」 | 「σ2.8 颗粒把拉普拉斯方差抬高，B1 647 vs 原片 382，判据失效」 |

**没有数字的经验不算经验。**

---

## 4. 当前状态

- 2026-10-24 建立。同日新增 `mv-pv-arrangement`、`mv-jp-lyrics-subtitle`、`blender-editable-timeline`。
- 跨线铁律 11 条，全部有实测支撑。
- ⛔ 本文件是**索引 + 铁律**，不要往这里塞细节——细节进对应 skill。
