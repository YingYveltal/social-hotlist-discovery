---
name: social-hotlist-discovery
description: Use when the task is to discover hotlists / hot searches / trending boards across one or more social platforms, with aggregator-first routing and mandatory fallback to a more platform-native discovery path when evidence is weak.
---

# Social Hotlist Discovery

Use this skill when the user wants current hotlists / hot searches / trending boards from one or more social platforms.

## When to use
- Cross-platform hotlist scans
- “看看今天各平台热榜”
- “搜一下抖音/小红书/微博/B站/知乎热搜”
- Topic discovery before downstream collection or content planning

## Do not use
- The user gives one explicit platform + one explicit keyword search
- The user already gives an explicit URL / share text / local file
- The task is direct content extraction rather than trend discovery
- The task is account-wide crawling or comment-thread reconstruction

## Routing intent
- One explicit platform + one explicit keyword -> use the runtime's platform search path
- One explicit platform + hotlist / trending-board discovery -> use the runtime's trending-board path
- One explicit URL / share text / local file -> use the runtime's extraction path

## Default route
1. Start with one or more reliable aggregator hotlist sources for cross-platform speed.
2. Validate coverage, freshness, item quality, and link usability per platform.
3. If any platform is missing, stale, suspicious, structurally broken, or lacks usable downstream links, switch to a more platform-native discovery path for that platform when available.
4. Return the discovered hot items with source notes and confidence.

## Mandatory fallback rule
You must use a more platform-native discovery path for a platform if any of these is true:
1. The aggregator does not contain the requested platform.
2. The section is empty or obviously incomplete.
3. Freshness cannot be established, or the results look stale.
4. Items look polluted, mismatched, abnormal, or structurally broken.
5. Downstream-usable links are missing or unusable.
6. The user asks for higher confidence, platform-native validation, or production-grade output.

## If fallback is skipped
State explicitly:
- which aggregator source(s) were used
- whether the requested platform was covered
- what freshness/update signal was observed
- roughly how many usable items were obtained
- whether usable links were available
- why the evidence was strong enough to skip fallback this time

Never skip fallback silently.

## Downstream handoff
- Direct content links available -> hand them to whatever explicit-link extraction path exists in that runtime
- Only hot terms / search-entry links available -> use the runtime's platform search path to get candidate posts/videos, then the runtime's extraction path

## Output discipline
- Distinguish aggregator evidence from platform-native evidence.
- Do not overclaim freshness if no update signal exists.
- Do not describe generic web search results as platform hotlists.
- Prefer clear source notes over vague confidence claims.

## Reference
For the full SOP and rationale, read `references/SOP.md`.
