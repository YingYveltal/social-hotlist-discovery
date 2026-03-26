# Example requests

## Should trigger this skill

- 搜搜各个平台的热榜/热搜
- 看看今天都在聊什么
- 抓一下小红书/抖音/微博/B站热榜
- 给我一个跨平台热点快照

## Should NOT trigger this skill

- 在小红书搜一下 AI 绘画
  - route: `social-search`
- 帮我提取这个微博链接
  - route: `universal-extractor`
- 把这几个视频都总结一下
  - route: extraction / analysis, not hotlist discovery

## Example routing notes

### Request
看看今天抖音、小红书、微博都在聊什么

### Expected behavior
1. Use aggregator source(s) first for a quick scan.
2. Validate section freshness and structure for each requested platform.
3. If any platform is missing / stale / suspicious / link-poor, switch to a more platform-native discovery path for that platform.
4. Return hot items with source notes.
