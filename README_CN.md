# social-hotlist-discovery

一个可复用的 OpenClaw Skill，用来处理**跨平台热榜 / 热搜 / Trending Board 发现**。

[English README](./README.md)

## 这个 Skill 解决什么问题

它主要固化一条决策规则：

- **默认先走聚合源（aggregator-first）**，提高跨平台扫描效率
- **当聚合源证据不足时，必须切到更平台原生的发现路径**，而不是草率给结论

适合这类请求：
- 看看今天各个平台热榜
- 抓一下抖音 / 小红书 / 微博 / B站 / 知乎热搜
- 做选题前先扫一下今天大家都在聊什么

## 为什么要单独做成 Skill

因为 agent 很容易犯两个错：

1. 直接拿通用网页搜索结果冒充“热榜”
2. 聚合源质量很差时，也不切换到更靠谱的平台原生路径

这个 Skill 的核心原则就是：

> 默认 aggregator-first；只要聚合证据缺失、过旧、可疑、污染或不可靠，就必须做 platform-native fallback。

## 仓库内容

- `SKILL.md`：给 agent 读的技能定义
- `references/SOP.md`：完整 SOP 与判断规则
- `examples/requests.md`：示例请求
- `README.md`：英文说明

## 安装方式

### 方式 A：直接 clone 到 skills 目录

```bash
cd ~/.openclaw/skills
git clone https://github.com/YingYveltal/social-hotlist-discovery.git
```

### 方式 B：手动复制目录

把仓库目录放到：

```bash
~/.openclaw/skills/social-hotlist-discovery
```

## 建议如何接入你自己的 OpenClaw

你可以在自己的 `AGENTS.md`、能力集合配置、或工作区约束里写清楚：

- 跨平台热榜 / 热搜发现 -> 用这个 skill
- 单平台 + 明确关键词搜索 -> 走你自己的 platform search 路径
- 显式链接提取 -> 走你自己的 extraction 路径

## 推荐路由模型

你不一定要有和我完全同名的工具，只要能映射到这些角色就行：

- **Aggregator path**：跨平台聚合热榜来源
- **Platform-native fallback**：平台原生热榜发现能力
- **Platform search path**：站内关键词搜索能力
- **Extraction path**：显式链接提取能力

重点不是工具名，重点是决策规则：

> 当 coverage、freshness、structure、confidence 任一项站不住脚时，不要停在 aggregator 结果上。

## 适用请求示例

- 搜搜各个平台的热榜/热搜
- 看看今天都在聊什么
- 抓一下小红书/抖音/微博/B站热榜
- 给我一个跨平台热点快照

## 两种工作模式

### 轻量扫描
1. 先看聚合源
2. 检查平台覆盖、更新时间、结构质量
3. 输出快照
4. 只对缺失 / 可疑平台走平台原生 fallback

### 高置信 / 生产级扫描
1. 先看聚合源
2. 严格检查 freshness 和 structure
3. 对缺失 / 过旧 / 可疑平台立即走平台原生 fallback
4. 带来源说明返回结果

## 下游交接

- 如果已经拿到显式链接 -> 交给你运行时里的 extraction path
- 如果只有热词 / 搜索入口 -> 先走 platform search path，再交给 extraction path

## 说明

这个 Skill 本身**不负责正文提取、评论提取、媒体分析**。
它只负责：

- 热榜发现
- 路由决策
- fallback 纪律
- 证据说明标准
