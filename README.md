# social-hotlist-discovery

A reusable OpenClaw skill for discovering social hotlists / hot searches / trending boards across one or more platforms.

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
- `examples/` — example prompts and expected routing patterns

## Suggested placement

Place this directory under your OpenClaw skills path, for example:

```bash
~/.openclaw/skills/social-hotlist-discovery
```

## Example requests this skill should catch

- “搜搜各个平台的热榜/热搜”
- “看看今天都在聊什么”
- “抓一下小红书/抖音/微博/B站/知乎热榜”

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
