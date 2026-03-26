---
id: social-hotlist-discovery
title: "Social Hotlist Discovery SOP"
tags: [social, trending, hotlist, hotsearch, aggregator, fallback, social-trending, discovery]
owner: main
version: "2026-03-27"
last_updated: "2026-03-27"
---

# Social Hotlist Discovery SOP

## Trigger

Use when the task is to discover current hotlists / hot searches / trending boards from one or more social platforms.

Typical asks:
- “搜搜各个平台的热榜/热搜”
- “看看今天都在聊什么”
- “抓一下小红书/抖音/微博/B站/知乎热榜”

## Goal

Prefer a fast aggregator-first discovery path for cross-platform scans, while treating a more platform-native discovery path as a **mandatory fallback** when aggregator coverage is missing, stale, broken, or insufficient for reliable downstream work.

## Default Route

### Step 1: Aggregator-first discovery

Check one or more aggregator hotlist sources first when they are reachable and appear current.

Examples:
- `ablesci / tophot`
- `hotday`
- `momoyu`
- other verifiable aggregator pages with clear platform sections and update times

Desired outputs from this step:
- platform name
- hotlist items / hot search terms
- update time / freshness signal
- jump links when available

### Step 2: Validate aggregator quality

Before trusting aggregator output, verify at least these:
- the target platform section exists
- update time is present or freshness is otherwise evident
- item count is non-trivial for that platform
- titles look platform-relevant rather than polluted/noisy
- links are usable for downstream navigation or collection

## Mandatory Fallback Rule

If **any** of the following is true, you **must use a more platform-native discovery path** for that platform unless no such path is available in the current runtime:

1. The aggregator does not contain the requested platform.
2. The platform section is empty or obviously incomplete.
3. Freshness cannot be established, or the data looks stale.
4. Returned items are abnormal, polluted, mismatched, or structurally broken.
5. Links are missing or unusable for downstream collection.
6. The user asks for higher confidence, platform-native validation, or production-grade output.

## If Platform-Native Fallback Is Not Used

You must provide a concrete reason, not a vague excuse.

State clearly:
- which aggregator source(s) were used
- whether the requested platform was covered
- what freshness/update signal was observed
- approximately how many usable items were obtained
- whether usable links were obtained
- why the evidence is strong enough to skip fallback this time

In short:
- either use a platform-native fallback path, or
- give an evidence-backed reason for not using it

Do **not** silently skip fallback out of convenience.

## Reliability Modes

### Lightweight scan

Use when the user mainly wants a quick cross-platform snapshot.

Route:
1. aggregator-first
2. validate freshness + coverage
3. summarize results
4. use a platform-native discovery path only for missing/suspicious platforms

### High-confidence / production scan

Use when the output will drive topic selection, operations, reporting, or downstream collection.

Route:
1. aggregator-first scan
2. immediately switch to a platform-native discovery path for any missing / stale / suspicious platform
3. optionally cross-check with other runtime-specific platform or extraction flows
4. return findings with clearer source notes

## Downstream Collection Route

After hotlist discovery:

### Case A: direct content links are available
- pass the explicit links to whatever extraction path exists in that runtime

### Case B: only hot terms / search-entry links are available
- use the runtime's platform search path to find concrete candidate posts/videos
- then pass the chosen explicit links to the runtime's extraction path

## Notes

- Aggregators are the default fast path, not the final source of truth.
- Platform-native fallback is real fallback, not decorative.
- For cross-platform scans, aggregator-first usually improves speed and breadth.
- For weak aggregator results, fallback should happen early rather than after a bad summary is already delivered.

## One-line Principle

**Aggregator-first by default; platform-native fallback is mandatory when aggregator evidence is missing, weak, stale, or unreliable.**
