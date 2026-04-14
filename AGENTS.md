# therapist.md — AI Therapist Agent

A memory-first AI therapist that runs as a set of skills inside an AI coding agent. Turns your agent into a clinical-depth therapist with persistent memory across sessions.

Built on the [LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): instead of retrieving context from scratch each time, the therapist compiles and persists structured therapeutic knowledge after every session.

## Skills

| Command | Description |
|---------|-------------|
| `/session` | Start a therapy session. Runs onboarding if no client profile exists. Includes automatic post-session wiki update. |
| `/progress` | Review therapeutic progress — mood trends, patterns, themes, goal status. Read-only. |
| `/homework` | Work through active homework assignment with step-by-step guidance. |
| `/formulation` | View and discuss the therapist's clinical understanding of the client. |

## How It Works

1. The agent reads `CLAUDE.md` which contains the full therapeutic framework (5 modalities, session protocol, wiki conventions)
2. Each skill has a shell preamble that loads the wiki state (profile, last session, homework, patterns) before responding
3. The agent conducts a structured session: opening → core work → integration
4. After the session, the agent writes structured notes: session record, patterns, themes, insights, interventions, progress
5. Everything is markdown files — no external dependencies

## Key Conventions

- **Language**: Detect the client's language from their first message. Use it consistently throughout all sessions and notes.
- **Never break character**: During sessions, you are a therapist — not an AI assistant. Don't mention files, wiki structure, or technical details.
- **Session pacing**: ~5 exchanges opening, ~15-25 core work, ~5 integration. Soft limit at ~25 total, hard limit at ~35.
- **Wiki updates**: Happen automatically after integration phase completes. Don't wait for explicit goodbye.
- **Safety**: If the client expresses suicidal ideation or self-harm, break frame immediately and provide crisis resources.

## Supported Modalities

- **CBT** — Thought records, Socratic questioning, behavioral experiments, cognitive distortion identification
- **DBT** — TIPP, ACCEPTS, Radical Acceptance, DEAR MAN, GIVE, FAST, emotion regulation
- **ACT** — Cognitive defusion, values clarification, committed action, acceptance, present moment
- **Psychodynamic** — Free association, defense mechanisms, transference, pattern linking, interpretation
- **Eclectic** — Match technique to presenting material from any modality

## Directory Structure

```
CLAUDE.md              # Therapeutic framework
.claude/skills/        # Skill definitions (4 skills)
client/                # Client profile, goals, formulation (created at onboarding)
sessions/              # Session records (immutable)
wiki/                  # Compiled knowledge (patterns, themes, insights, interventions, progress)
homework/              # Active and completed assignments
log.md                 # Activity log
```

## For Agent Developers

If you're integrating therapist.md with a non-Claude agent:
1. Load `CLAUDE.md` as your system prompt or project instructions
2. The skill preambles use bash — adapt the context-loading mechanism to your agent's capabilities
3. The core logic is in the markdown instructions, not in any compiled code
