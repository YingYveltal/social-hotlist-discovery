# social-hotlist-discovery

A reusable OpenClaw skill for discovering social hotlists / hot searches / trending boards across one or more platforms.

[中文说明](./README_CN.md)

## What this skill does

This skill standardizes one routing rule:

- **Aggregator-first** for fast cross-platform discovery
- **Mandatory fallback to a more platform-native discovery path** when aggregator evidence is missing, stale, weak, polluted, or unusable

It is designed for asks like:
- 看看各个平台今天的热榜
- 搜一下抖音 / 小红书 / 微博 / B站 热搜
- 做选题前先扫一下今天大家在聊什么

## Why this exists

Without a dedicated skill, agents tend to do one of two bad things:

1. jump straight to generic web search and call it “热榜”
2. forget to switch to a more platform-native discovery path when aggregator quality is weak

This skill fixes that by encoding a simple policy:

> Aggregator-first by default; platform-native fallback is mandatory when aggregator evidence is missing, weak, stale, or unreliable.

## Files

- `SKILL.md` — the agent-facing skill contract
- `references/SOP.md` — the full SOP / reasoning / fallback rules
- `examples/requests.md` — example prompts and expected routing patterns
- `README_CN.md` — Chinese introduction for sharing

## Installation

### Option A: clone into your OpenClaw skills directory

```bash
cd ~/.openclaw/skills
git clone https://github.com/YingYveltal/social-hotlist-discovery.git
```

### Option B: copy the folder manually

Copy this repository to:

```bash
~/.openclaw/skills/social-hotlist-discovery
```

## Suggested registration

Make the skill discoverable from your workspace guidance, for example in `AGENTS.md` or your capability collection notes:

- use this skill for cross-platform hotlist / hot-search / trending-board discovery
- keep it separate from keyword search skills
- keep it separate from explicit-link extraction skills

## Example requests this skill should catch

- “搜搜各个平台的热榜/热搜”
- “看看今天都在聊什么”
- “抓一下小红书/抖音/微博/B站/知乎热榜”
- “给我一个跨平台热点快照”

## Expected routing behavior

### Lightweight scan
1. check aggregator source(s)
2. validate platform coverage + freshness
3. summarize hot items
4. switch to a platform-native discovery path only for missing / suspicious platforms

### High-confidence / production scan
1. check aggregator source(s)
2. validate freshness + structure
3. switch to a platform-native discovery path for any missing / stale / suspicious platform
4. return findings with source notes

## Example adaptation

If your runtime already has named tools, map them like this:

- aggregator page fetcher -> aggregator path
- per-platform trending tool / browser workflow -> platform-native fallback
- per-platform keyword search tool -> platform search path
- explicit-link extractor -> extraction path

If your runtime does **not** have these tools yet, this skill still helps by enforcing the decision rule and evidence standard.

## Downstream handoff

- explicit links -> use the runtime's explicit-link extraction path
- only hot terms -> use the runtime's platform search path, then its extraction path

## Adapting this skill to your own tool stack

This skill is intentionally **tool-agnostic**.

You can map its routing rules onto whatever your runtime already has:

- If you have a platform-native trending tool, use it as the fallback path.
- If you do not, use browser automation, site-specific scraping, MCP tools, or manual platform pages.
- If you have an extractor, hand explicit links to it.
- If you do not, keep this skill as a discovery-only policy and let downstream collection be implemented separately.

A simple mapping model:

- **Aggregator path** -> your preferred cross-platform hotlist source(s)
- **Platform-native fallback** -> your strongest per-platform trending discovery path
- **Platform search path** -> your per-platform keyword/candidate search path
- **Extraction path** -> your explicit-link content extraction path

The important part is not the exact tool name.
The important part is the decision rule:

> Do not stop at aggregator results when coverage, freshness, structure, or confidence is weak.

## Notes

This skill does **not** extract正文, comments, or media by itself.
It only governs **discovery and routing**.

## License / sharing

If you reuse or adapt it, feel free to keep the core routing principle and rewrite the examples to match your own tool stack.
