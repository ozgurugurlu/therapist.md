# therapist.md

A memory-first AI Therapist Agent for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Every session builds on the last.

Inspired by [Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — adapted for therapy. Instead of retrieving context from scratch each time, Claude maintains a structured wiki of therapeutic knowledge that compounds across sessions: patterns, themes, insights, interventions, and progress.

## What It Does

You open Claude Code in this directory, type `/session`, and you're in therapy. Claude becomes a therapist who:

- **Remembers everything** — every session, every pattern, every insight, automatically indexed
- **Applies real techniques** — CBT thought records, DBT emotion regulation, ACT defusion, psychodynamic interpretation, and more
- **Tracks your patterns** — not just within a session, but across weeks and months
- **Assigns homework** — between-session exercises tied to your work
- **Evolves its understanding** — a living case formulation that deepens over time

No API keys. No databases. No external services. Just markdown files and Claude Code.

## Getting Started

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed

### Setup

```bash
git clone https://github.com/ozgurugurlu/therapist.md.git
cd therapist.md
claude
```

Then type:

```
/session
```

That's it. Since no client profile exists yet, Claude will start with **onboarding** — a warm intake conversation where you'll:

1. Introduce yourself
2. Talk about what brings you to therapy
3. Learn about different therapeutic approaches and pick one
4. Set goals together

After onboarding, your profile is saved. Every future `/session` goes straight into a full therapy session.

## Commands

| Command | What it does |
|---------|-------------|
| `/session` | Start a new therapy session (or onboarding if first time) |
| `/progress` | Review your therapeutic journey — mood trends, patterns, goal progress |
| `/homework` | Work through your active homework assignment with guidance |
| `/formulation` | See how the therapist understands you — and discuss it |

### Typical Week

```
/session              → Full therapy session (~25-35 exchanges)
                        Automatic note-taking and wiki update included

... a few days later ...

/homework             → Walk through the assigned exercise

... next week ...

/session              → New session (starts with homework review)
/progress             → Check how far you've come
```

## How It Works

### The LLM Wiki Pattern (applied to therapy)

Classic RAG: query → retrieve → respond → forget.

LLM Wiki: session → **compile knowledge** → persist → recall next time.

After every session, Claude writes structured notes:

```
Session record (raw)
  ↓
Patterns extracted     →  "Catastrophizing before presentations"
Themes identified      →  "Evaluation anxiety rooted in childhood"
Insights captured      →  "Anticipation is worse than the event itself"
Interventions logged   →  "Socratic questioning — effectiveness: 4/5"
Progress updated       →  Mood trend, goal status, timeline
```

This wiki grows over time. By session 10, Claude has a rich, cross-referenced understanding of your patterns, triggers, and growth — far deeper than any single-session chatbot.

### Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Schema    │     │    Wiki     │     │     Raw     │
│  CLAUDE.md  │     │   wiki/    │     │  sessions/  │
│             │     │             │     │             │
│ Therapeutic │     │  Compiled   │     │  Session    │
│ framework,  │     │  knowledge: │     │  transcripts│
│ techniques, │     │  patterns,  │     │  (immutable)│
│ rules       │     │  themes,    │     │             │
│             │     │  insights   │     │             │
│ (rarely     │     │ (updated    │     │ (append-    │
│  changes)   │     │  every      │     │  only)      │
│             │     │  session)   │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
```

### Under the Hood

1. **CLAUDE.md** loads automatically when Claude Code opens the directory — it contains the complete therapeutic framework
2. **Slash commands** (in `.claude/skills/`) define the entry points. Each has a shell preamble that reads the wiki state before Claude responds
3. **Post-session**, Claude automatically updates all wiki pages, creates session records, and assigns homework
4. **Everything is markdown** — human-readable, git-friendly, no lock-in

## Supported Modalities

During onboarding, you choose a therapeutic approach (or start with Eclectic and explore):

| Modality | Best for | Key techniques |
|----------|----------|----------------|
| **CBT** | Negative thought patterns, anxiety | Thought records, behavioral experiments, Socratic questioning |
| **DBT** | Intense emotions, impulsivity | TIPP, ACCEPTS, Radical Acceptance, DEAR MAN |
| **ACT** | Avoidance, feeling stuck, meaning | Defusion, values clarification, committed action |
| **Psychodynamic** | Relationship patterns, recurring themes | Free association, transference, pattern linking |
| **Eclectic** | Not sure / want flexibility | Best of all approaches, matched to what you bring |

## Directory Structure

```
therapist.md/
├── CLAUDE.md                    # Therapeutic framework (the "brain")
├── README.md                    # This file
├── .claude/skills/              # Slash command definitions
│   ├── session/SKILL.md         #   /session
│   ├── progress/SKILL.md        #   /progress
│   ├── homework/SKILL.md        #   /homework
│   └── formulation/SKILL.md     #   /formulation
│
│  ── Created per user (gitignored) ──
│
├── client/                      # Your profile, goals, case formulation
├── sessions/                    # Session records (immutable after writing)
├── wiki/                        # Compiled therapeutic knowledge
│   ├── patterns/                #   Cognitive/emotional/behavioral patterns
│   ├── themes/                  #   Overarching life themes
│   ├── insights/                #   Breakthroughs and realizations
│   ├── interventions/           #   Techniques used and their effectiveness
│   ├── relationships/           #   Key relationship dynamics
│   └── progress/                #   Mood trends, timeline, metrics
├── homework/                    # Active and completed assignments
└── log.md                       # Chronological activity log
```

Everything under "Created per user" is generated during your first `/session` (onboarding) and grows from there. It's gitignored so your personal data never gets pushed.

## Safety

- **Crisis protocol**: If you express active suicidal ideation or self-harm intent, the therapist breaks frame immediately, acknowledges your pain, and provides crisis resources (Turkish hotlines: 182, 112, ALO 113)
- **Limitations disclosed**: During onboarding, Claude explains this is not a replacement for licensed therapy — no diagnoses, no medication decisions
- **Data stays local**: All files are on your machine. Nothing is sent anywhere. Don't make the repo public with client data in it.

## Language

The therapist **automatically speaks your language**. It detects your language from your first message and uses it throughout all sessions and notes. The framework and documentation are in English. Therapeutic examples in `CLAUDE.md` are written in Turkish as tone references — Claude adapts them to your language naturally.

## Acknowledgments

- [Andrej Karpathy](https://github.com/karpathy) for the [LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — the core idea that LLMs should compile and persist knowledge rather than retrieve it fresh each time
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) for the skill/preamble system that makes this possible
- Evidence-based therapeutic frameworks: CBT, DBT, ACT, and psychodynamic therapy

## License

MIT
