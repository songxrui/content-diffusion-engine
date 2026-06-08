---
name: content-diffusion-engine
description: "跨平台内容扩散引擎。从dbs-content-system产出的内容单元自动适配为多平台版本(公众号/小红书/推特/抖音/飞书)，保持核心观点一致但形式适配。触发词：content-diffusion、跨平台、多平台发布、一鱼多吃、内容扩散、平台适配。不适用：单平台优化→对应平台skill; 内容创作→khazix-writer/dbs-content。正例：'帮我把这篇文章适配到小红书和推特'→触发; '/content-diffusion 最近3篇推文扩散到公众号'→触发。反例：'帮我写一篇推文'→不触发→khazix-writer; '优化这个标题'→不触发→dbs-hook。"
version: "1.0.0 | R1: 2026-06-08 | incubated from: dbs-content-system + crosspost + dbs-xhs-title + khazix-writer + viral-writer patterns | model: DeepSeek v4 Pro"
---

# content-diffusion-engine R1 — 跨平台内容扩散引擎

> 孵化来源: 深度结合 dbs-content-system 的 5 类内容单元 + crosspost 的多平台模式 + 各平台 skill (xhs/wechat/x) 的格式约束

## 一句话定义

从内容资产库的 5 类单元（QST/CON/OPI/CAS/SOL）自动生成适配不同平台的版本——同一核心观点，不同表达形式。

## 平台适配矩阵

| 平台 | 核心约束 | 形式 | 字数 | 节奏 | 对应 skill |
|------|---------|------|------|------|-----------|
| 公众号 | 深度+结构 | 长文 | 1500-3000 | 慢/深 | khazix-writer |
| 小红书 | 视觉+情绪 | 图文 | 300-800 | 快/轻 | dbs-xhs-title |
| 推特/X | 锐利+一句 | 单推/线程 | 280/条 | 极快 | viral-writer |
| 抖音/短视频 | 前3秒+口语 | 口播脚本 | 200-400 | 极快 | dbs-hook |
| 飞书 | 专业+结构 | 文档 | 不限 | 中 | documents |
| 微博 | 话题+情绪 | 短博文 | 140-2000 | 快 | baoyu-post-to-weibo |

## 5 阶段管线

### Phase 1: 提取核心单元
从 dbs-content-system 产出的内容单元库提取：
- QST（核心问题）→ 各平台 hook
- OPI（核心观点）→ 各平台论点
- CAS（核心案例）→ 各平台故事
- SOL（方案）→ 各平台行动号召

### Phase 2: 平台拆解
对每个目标平台，按约束拆解：
```
公众号版: [QST引入] → [CON解释] → [OPI展开+CAS佐证] → [SOL行动]
小红书版: [图片标题] → [QST痛点] → [OPI一句话] → [CAS小故事] → [CTA]
推特版: [OPI锐利版] → [CAS精简版] → [SOL号召]
抖音版: [前3秒hook] → [QST] → [OPI反转] → [CTA]
```

### Phase 3: 自动化生成
调用对应平台 skill 生成各版本：
- 公众号 → khazix-writer (长文模式)
- 小红书 → dbs-xhs-title + baoyu-xhs-images
- 推特/X → viral-writer (线程模式)
- 飞书 → documents (结构化文档)

### Phase 4: 一致性校验
- 核心观点是否跨平台一致？
- 案例是否跨平台一致？
- CTA 是否跨平台协调？
- 发布时间是否协调（不同平台节奏不同）？

### Phase 5: 发布计划
```
# 内容扩散发布计划
## 核心内容单元: {标题}
## 发布日程
| 日期 | 平台 | 版本 | 状态 |
|------|------|------|------|
| D+0 | 公众号 | 长文版 | 待发布 |
| D+1 | 小红书 | 图文版 | 待发布 |
| D+1 | 推特/X | 线程版 | 待发布 |
| D+2 | 抖音 | 口播版 | 待发布 |
| D+3 | 飞书 | 归档版 | 待发布 |
```

## 与其他 skill 联动

```
dbs-content-system ──→ content-diffusion-engine ──→ 各平台:
  (5类单元提取)           (跨平台适配)          ├── khazix-writer (公众号)
                                                ├── dbs-xhs-title (小红书)
                                                ├── viral-writer (推特)
                                                ├── dbs-hook (抖音)
                                                └── documents (飞书)
```

## 验证清单

- [ ] 5 类核心单元已提取
- [ ] 每个目标平台有适配版本
- [ ] 核心观点跨平台一致
- [ ] 发布计划时间协调
- [ ] 各版本已通过 dbs-ai-check 去 AI 味

## G1-G6

| 门禁 | 状态 |
|------|------|
| G1 ≤10KB | ✅ |
| G2 触发层(6词+正反例) | ✅ |
| G3 可执行(5Phase) | ✅ |
| G4 验证(5项) | ✅ |
| G5 失败兜底 | ✅ |
| G6 安全 | ✅ |
