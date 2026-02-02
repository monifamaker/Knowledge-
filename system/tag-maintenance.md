# Tag Maintenance Guide

Tags evolve over time. New concepts emerge, old tags become redundant, and clusters shift as your thinking develops. This guide provides Cowork prompts for maintaining a healthy, connected knowledge graph.

---

## Maintenance Schedule

| Frequency | Task | Time |
|-----------|------|------|
| **Weekly** | Quick tag health check | 10 min |
| **Weekly** | Synthesis note | 15 min |
| **Monthly** | Tag consolidation analysis | 30 min |
| **Monthly** | Connection discovery | 30 min |
| **After consolidation** | Batch re-tag all notes | 15 min |

---

## 1. Weekly Tag Health Check

Run this weekly to catch sprawl early.

### Cowork Prompt
```
Analyze the current state of my knowledge graph:

1. Read tag-library.md and count total tags
2. Scan all notes in /notes/ and count:
   - How many times each tag is used
   - Which tags are used only once
   - Which tags are used most frequently
3. Identify any tags in notes that aren't in tag-library.md

Create a brief health report with:
- Total tags in library
- Total unique tags in use
- Top 10 most-used tags
- Tags used only once (candidates for removal/merge)
- Orphan tags (in notes but not in library)
```

### What to Look For
- **Single-use tags**: Probably too specific, merge into broader category
- **Orphan tags**: Add to library or consolidate
- **Tag explosion**: If you added 10+ new tags this week, review for overlap

---

## 2. Monthly Tag Consolidation Analysis

Run monthly to identify merge opportunities and clean up redundancy.

### Cowork Prompt
```
Perform a deep analysis of tag-library.md and all notes in /notes/:

1. Identify tags that mean the same thing or overlap significantly
   Examples: #BusinessModel vs #BusinessStrategy, #Growth vs #GrowthMarketing

2. Identify tags that could be merged into a parent concept
   Example: #Meditation, #Mindfulness, #Presence → all under #PersonalGrowth?

3. Identify low-frequency tags (used 1-2 times) that should be:
   - Merged into broader tags
   - Kept because they're genuinely unique
   - Deprecated entirely

4. Identify potential NEW clusters emerging from recent notes
   that aren't yet reflected in the tag relationship section

Create a consolidation report with:
- Recommended merges (with reasoning)
- Tags to deprecate (with reasoning)
- New clusters to add to tag-library.md
- DO NOT apply changes yet - just report recommendations
```

### Review Process
1. Review Cowork's recommendations
2. Decide which consolidations to accept
3. Update tag-library.md manually with your decisions
4. Run batch re-tagging (next section)

---

## 3. Batch Re-Tagging After Library Updates

Run this AFTER you've updated tag-library.md with consolidations.

### Cowork Prompt (Preview Mode)
```
The tag library has been updated with consolidations.
Before applying changes, show me what would change:

1. Read the current tag-library.md (especially the Deprecated Tags section)
2. Scan all notes in /notes/
3. For each note, identify:
   - Deprecated tags that need replacement
   - Missing tags that should be added based on content
   - Tags that no longer fit the note's content

Create a preview report showing:
- Which notes would be modified
- What tags would be removed
- What tags would be added
- DO NOT make changes yet
```

### Cowork Prompt (Apply Mode)
```
Apply the tag updates to all notes in /notes/:

1. Read tag-library.md for the current canonical tag list
2. Read the Deprecated Tags section for replacements
3. For each note in /notes/:
   - Replace any deprecated tags with their replacements
   - Remove tags that no longer exist in the library
   - Ensure the Tags section uses only tags from tag-library.md
4. Save each modified note

Report:
- How many notes were modified
- Summary of changes made
- Any notes that couldn't be updated (and why)
```

### Tag Replacement Mapping

When you consolidate tags, add them to this section of tag-library.md:

```markdown
## Deprecated Tags

| Old Tag | Replaced By | Date |
|---------|-------------|------|
| #BusinessModel | #BusinessStrategy | 2026-01 |
| #GrowthMarketing | #Growth | 2026-01 |
| #PersonalChange | #PersonalGrowth | 2025-09 |
```

Cowork will use this mapping during re-tagging.

---

## 4. Monthly Connection Discovery

Run monthly alongside tag consolidation to find hidden patterns and cross-domain insights.

### Cowork Prompt (Theme Analysis)
```
Analyze all notes in /notes/ for hidden connections and patterns:

1. Read every note and extract:
   - Main topics and themes
   - People mentioned
   - Projects referenced
   - Emotional/reflective content vs strategic/business content

2. Identify connections I might have missed:
   - Notes that discuss similar concepts but use different tags
   - Themes that span multiple tag clusters
   - People who appear across different contexts
   - Ideas that evolved over time (trace the thread)

3. Look for cross-domain insights:
   - Where does somatic/embodiment work connect to business strategy?
   - Where does identity work show up in leadership content?
   - What patterns emerge across client engagements?

Create a Connection Discovery Report with:
- Top 5 hidden connections with specific note references
- Emerging themes not yet captured in tag clusters
- Suggested new tag relationships to add to tag-library.md
- Notes that should link to each other (suggest wikilinks)
```

### Cowork Prompt (Knowledge Graph Visualization)
```
Create a text-based map of my knowledge graph:

1. Read all notes in /notes/
2. Group notes by their primary tag clusters
3. Identify bridge notes (notes that connect multiple clusters)
4. Map the relationships between clusters

Output format:
- List each major cluster with its notes
- Show which clusters connect to which (and through what notes)
- Highlight the most connected notes (knowledge hubs)
- Identify isolated notes that might need better tagging
```

### Cowork Prompt (Temporal Analysis)
```
Analyze how my thinking has evolved over time:

1. Read all notes in /notes/ sorted by date
2. Track which tags appear in which time periods
3. Identify:
   - Topics that have grown in importance
   - Topics that have faded
   - New themes that emerged recently
   - Recurring cycles (topics that come back)

Create a Temporal Analysis showing:
- Timeline of major theme shifts
- Tags that peaked and declined
- Your current focus areas (last 30 days)
- Predictions for emerging themes based on recent trajectory
```

---

## 5. Tag Evolution Principles

### When to Add a New Tag
✅ Content covers genuinely new territory not captured by existing tags
✅ The concept appears in 3+ notes (or likely will)
✅ The tag creates meaningful connections in the knowledge graph

### When to Merge Tags
✅ Two tags consistently appear together
✅ One tag is a subset of another
✅ Tags represent the same concept with different words

### When to Deprecate Tags
✅ Tag used only once and unlikely to recur
✅ Tag is too specific (could be a note title instead)
✅ Tag has been superseded by a better tag

### Tag Hygiene Rules
- **Aim for 75-100 total tags** — enough for nuance, not so many you lose coherence
- **Review new tags weekly** — don't let them accumulate unchecked
- **Document consolidations** — keep the Deprecated Tags section updated
- **Trust the clusters** — let related notes find each other through shared tags

---

## 6. Emergency Tag Cleanup

If your tag library has gotten out of control (150+ tags, lots of duplicates):

### Cowork Prompt (Nuclear Option)
```
My tag library needs a major cleanup. Help me restructure it:

1. Read all notes in /notes/ and tag-library.md
2. Identify the TRUE core themes across all my notes (aim for 10-15 major themes)
3. For each theme, identify which existing tags belong to it
4. Propose a simplified tag structure:
   - 10-15 primary category tags
   - 3-5 tags under each category
   - Total target: 50-75 tags

Create:
- New proposed tag-library.md structure
- Mapping from ALL current tags to new tags
- List of tags to completely remove (with reasoning)

DO NOT apply changes. Present for my review first.
```

After reviewing and approving:
```
Apply the new tag structure to all notes in /notes/:
- Use the mapping we agreed on
- Replace all old tags with new equivalents
- Save the new tag-library.md
- Report on completion
```

---

## Quick Reference: All Maintenance Prompts

| Task | Prompt Starter |
|------|----------------|
| Health check | "Analyze the current state of my knowledge graph..." |
| Consolidation | "Perform a deep analysis of tag-library.md..." |
| Re-tag preview | "The tag library has been updated. Show me what would change..." |
| Re-tag apply | "Apply the tag updates to all notes..." |
| Connections | "Analyze all notes for hidden connections..." |
| Visualization | "Create a text-based map of my knowledge graph..." |
| Temporal | "Analyze how my thinking has evolved over time..." |
| Nuclear cleanup | "My tag library needs a major cleanup..." |
