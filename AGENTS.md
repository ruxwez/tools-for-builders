# AGENTS.md

## Purpose

This document defines the exact workflow the AI agent must follow when the user provides one or multiple URLs and/or tool names, so the agent can research them, classify them correctly, and update the repository consistently.

The process is optimized for small models (for example, `gpt-5-mini`): deterministic, checklist-based, low-ambiguity, and with strict output rules.

---

## Core Rules (Mandatory)

1. **Always research before classifying.**
   - Every URL/tool must be validated with web research and content fetching to confirm its real context.
   - Never classify based only on name assumptions.

2. **Process each tool separately.**
   - If the user sends multiple tools/URLs, treat each one as an independent item first.
   - Do not assume two items belong together just because they were sent in the same message.

3. **Use existing repository structure first.**
   - Try to place each item in an existing category.
   - Only create a new category when no existing category is a good semantic fit.

4. **Never invent tags.**
   - First scan repository files to discover existing tags (e.g., `🌗 Freemium`, `🏠 Self-Hosted`, `🐧 Open Source`, `☁️ SaaS`, `🆓 Free`).
   - Reuse discovered tags consistently.
   - If a truly necessary new tag is missing, create it explicitly and document it in this file.

5. **If a new category is created, update the corresponding index/table of contents.**
   - Any structural change must be reflected in the main index/navigation.

6. **Preserve existing style and format.**
   - Keep heading style, bullet style, icon style, and line format consistent with existing topic files.

---

## Required Workflow

### Step 1) Intake & normalization

For each user-provided item:

- Normalize into:
  - `type`: `url` or `tool-name`
  - `input_value`: raw input
  - `canonical_url` (if available)
  - `status`: `pending`

If input is a tool name without URL, find official source/docs URL during research.

---

### Step 2) Repository scan (before writing)

Before classifying any new item, inspect repository documentation and topic files to extract:

- Existing categories/topics
- Existing file naming conventions
- Existing tags and their exact spelling/icon order
- Existing line entry format

Build a local “known taxonomy”:

- `known_categories`
- `known_tags`
- `entry_template`

**Hard rule:** no insertion until taxonomy is built.

---

### Step 3) Research each item independently

For each item (one by one):

1. Perform web search to identify official product page and reliable supporting sources.
2. Fetch and read the official page content (and one additional reliable source if needed).
3. Extract minimal facts:
   - What it is
   - Primary use case
   - Delivery model (`SaaS`, `Self-Hosted`, hybrid)
   - Licensing/open-source status
   - Pricing/free-tier signal (`Free`, `Freemium`, paid-only)
4. Decide best category using repository taxonomy.

If confidence is low, mark item as `needs-review` instead of forcing placement.

---

### Step 4) Category decision policy

Use this order:

1. **Exact fit in an existing category** → use it.
2. **Strong fit in an existing category** → use it.
3. **No good fit** → create a new category.

When creating a new category:

- Use concise, descriptive naming aligned with repository style.
- Place the new section in the most logical topic file (or create a new topic file if necessary).
- Update the top-level index/table of contents accordingly.

---

### Step 5) Tagging policy

For every item:

1. Apply only tags discovered from repository scan.
2. Use evidence from research for each tag.
3. Keep tag order consistent with existing entries in that file.

If a necessary tag does not exist:

- Create a new tag only if:
  - It is broadly reusable (not one-off),
  - It resolves a real classification gap.
- Add the new tag definition to **Tag Registry** section in this file.
- Use it consistently going forward.

---

### Step 6) Write/update entry

Use the repository’s established bullet format. Standard target format:

- `[Name](URL) - Short factual description. (Tag1 | Tag2 | ...)`

Constraints:

- Description must be concise and factual (no marketing fluff).
- Keep one-line style unless file convention differs.
- Prefer official URL as primary link.

---

### Step 7) Index synchronization

After any category/topic structure change:

- Update `README.md` table of contents and/or relevant topic index files.
- Ensure links are valid and ordering remains consistent with project conventions.

No structural change is complete without index update.

---

### Step 8) Final validation checklist

Before finishing, verify:

- [ ] Every input item was researched.
- [ ] Every item was handled independently.
- [ ] Classification uses existing categories when possible.
- [ ] New category created only when required.
- [ ] Tags were reused from repository (or properly introduced).
- [ ] No invented claims without evidence.
- [ ] Index/TOC updated if structure changed.
- [ ] Markdown style matches repository conventions.

---

## Optimization Rules for Small Models (`gpt-5-mini` friendly)

1. **Use deterministic decisions, not long reasoning.**
2. **Follow strict checklists.**
3. **Prefer shortest valid edit path.**
4. **Minimize rewrites; edit only affected files.**
5. **One item at a time, then merge results.**
6. **Use concise evidence extraction (what/why/category/tags).**
7. **If uncertain, flag `needs-review` instead of guessing.**

---

## Conflict Resolution

If one tool could belong to multiple categories:

1. Choose the category of its **primary use case**.
2. Use tags to represent secondary traits.
3. Avoid duplicating the same entry across multiple files unless repository rules explicitly allow duplication.

---

## Output Contract (when user sends URLs/tools)

For each run, produce:

1. **Processed items summary**
   - Item
   - Chosen category
   - Applied tags
   - Source URL(s)

2. **Changes made**
   - Files modified
   - New categories/tags created (if any)

3. **Open review points**
   - Low-confidence classifications
   - Ambiguous licensing/pricing signals

---

## Tag Registry

This section lists repository-approved tags discovered/used by the agent.  
Initial known tags (from current repository content):

- `🐧 Open Source`
- `🏠 Self-Hosted`
- `🌗 Freemium`
- `☁️ SaaS`
- `🆓 Free`

If new tags are introduced in the future, append them here with a one-line definition.

---

## Non-Negotiables

- Do not invent categories, tags, or product facts without evidence.
- Do not skip research when URL/tool context is unclear.
- Do not forget index updates when structure changes.
- Do not batch-classify multiple tools as a single combined entry unless the user explicitly requests a bundle entry.