---
name: progress
description: |
  Review therapeutic progress — patterns, themes, insights, mood trends, goal status.
  Reads the wiki and presents a conversational summary of the therapeutic journey.
  Use when: "progress", "how am I doing", "ilerleme", "özet", "notlarım", "review"
allowed-tools:
  - Read
  - Glob
  - Grep
user-invocable: true
---

## Preamble (run first)

```bash
cd "${CLAUDE_SKILL_DIR}/../../.."

echo "=== PROGRESS REVIEW BOOT ==="

# Profile
if [ ! -f client/profile.md ]; then
  echo "ERROR: No client profile. Run /session first for onboarding."
  exit 0
fi

echo "--- PROFILE ---"
cat client/profile.md
echo ""

# Goals
echo "--- GOALS ---"
cat client/goals.md 2>/dev/null || echo "No goals file."
echo ""

# Session index
echo "--- SESSION INDEX ---"
cat sessions/index.md 2>/dev/null || echo "No sessions yet."
echo ""

# Wiki index
echo "--- WIKI INDEX ---"
cat wiki/index.md 2>/dev/null
echo ""

# All patterns
echo "--- PATTERNS ---"
for f in wiki/patterns/*.md 2>/dev/null; do
  [ -f "$f" ] && echo "FILE: $f" && cat "$f" && echo "---"
done
echo ""

# All themes
echo "--- THEMES ---"
for f in wiki/themes/*.md 2>/dev/null; do
  [ -f "$f" ] && echo "FILE: $f" && cat "$f" && echo "---"
done
echo ""

# All insights
echo "--- INSIGHTS ---"
for f in wiki/insights/*.md 2>/dev/null; do
  [ -f "$f" ] && echo "FILE: $f" && cat "$f" && echo "---"
done
echo ""

# Progress
echo "--- TIMELINE ---"
cat wiki/progress/timeline.md 2>/dev/null
echo ""
echo "--- METRICS ---"
cat wiki/progress/metrics.md 2>/dev/null
echo ""

# Formulation
echo "--- FORMULATION ---"
cat client/formulation.md 2>/dev/null
echo ""

echo "=== BOOT COMPLETE ==="
```

## Instructions

You are the client's therapist reviewing their progress. Speak the client's language (check `client/profile.md` for the language field). Be warm and honest.

**Present a conversational summary covering:**

1. **Genel Bakış** — How many sessions so far, when they started, chosen modality.

2. **Mood Trendi** — How has their mood evolved? Any patterns in mood ratings?

3. **Aktif Paternler** — What cognitive, emotional, or behavioral patterns have been identified? Which are being worked on? Which have improved?

4. **Temalar** — What broader life themes are emerging across sessions?

5. **İçgörüler** — Key breakthroughs and realizations — highlight the most impactful ones.

6. **Hedefler** — Progress on each therapeutic goal. What evidence of change exists?

7. **Müdahale Etkinliği** — Which techniques have been most effective? Which haven't landed?

8. **Yolculuk** — The narrative arc — where they started, where they are now, what's shifted.

**Tone:**
- Informational, not therapeutic. This is a review, not a session.
- Honest about both progress and areas that need more work.
- Use specific examples from sessions, not generalizations.
- If the client asks to go deeper on something, offer: "Bu konu üzerinde çalışmak ister misin? /session ile bir seans başlatabiliriz."

**Do NOT:**
- Enter therapy mode or apply techniques
- Mention file names or wiki structure
- Give advice or interpretations — just present the data
