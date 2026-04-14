# Contributing to therapist.md

Thank you for your interest in contributing. This project aims to make high-quality AI-assisted therapy accessible to everyone.

## Ways to Contribute

### Adding Therapeutic Techniques
The technique libraries in `CLAUDE.md` (Section 5) can always be expanded. If you have clinical knowledge of a technique that's missing:
1. Add it under the appropriate modality section
2. Include: technique name, when to use it, step-by-step guide, and what to save in the wiki
3. Keep instructions in English — the therapist adapts to the client's language at runtime

### Adding New Modalities
Currently supported: CBT, DBT, ACT, Psychodynamic, Eclectic. To add a new modality (e.g., EMDR, Schema Therapy, Narrative Therapy):
1. Add a new subsection under Section 5 in `CLAUDE.md`
2. Update the onboarding modality descriptions (Section 2, Step 3)
3. Update the Eclectic matching table (Section 5.5)
4. Update `README.md` modalities table

### Improving Session Quality
If you use therapist.md and notice the therapist could be better at something (asking details, pacing, transitions), open an issue describing:
- What happened
- What you expected
- What would be better

### New Skills
Skills live in `.claude/skills/<name>/SKILL.md`. If you have an idea for a new therapeutic skill (e.g., `/journal`, `/crisis-plan`, `/review-goals`):
1. Create the skill directory and SKILL.md with frontmatter
2. Include a preamble that reads relevant context
3. Write clear instructions
4. Update README.md commands table

## Development Setup

```bash
# Fork and clone
git clone https://github.com/YOUR_USERNAME/therapist.md.git
cd therapist.md

# Test locally
claude
/session   # Verify onboarding works
```

No build step. No dependencies. Edit markdown, test in Claude Code.

## Pull Request Guidelines

- **One concern per PR.** Don't mix a new technique with a session flow fix.
- **Test your changes.** Run at least one `/session` to verify nothing breaks.
- **Keep the therapeutic framework coherent.** Changes to one modality shouldn't contradict another.
- **English for instructions, examples are language-agnostic.** The therapist adapts at runtime.
- **Update CHANGELOG.md** with a brief description of your change under an `[Unreleased]` section.
- **Bump VERSION** for releases (maintainers will handle this).

## What Needs Explicit Approval

- Changes to the safety/crisis protocol
- Removal or weakening of the limitations disclosure
- Changes that would make the therapist give direct advice (against therapeutic principles)
- Changes to the core session structure (opening → core → integration → wiki update)

## Code of Conduct

This project deals with mental health. Be respectful in issues and PRs. Remember that real people use this for real therapeutic work.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
