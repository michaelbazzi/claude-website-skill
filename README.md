# Client Website Build Skill

A reusable [Claude Code Agent Skill](https://docs.claude.com/en/docs/claude-code/skills) for building and maintaining client websites — not a template, a *behavioral checklist* accumulated from real client builds. Every rule in here exists because something specific went wrong once and got written down so it wouldn't happen again.

## Why this exists

The gap between "an AI can write HTML/CSS" and "an AI can run a real client engagement end to end" isn't code quality — it's process discipline: asset handling, ambiguity resolution, safe revision workflows, and a long list of failure modes that only show up in production (a domain's DNS silently breaking outbound mail, a browser cache masking a real fix, a registrar quietly parking a domain because a verification email got missed). This skill is that accumulated knowledge, structured so it's reusable on the next build instead of re-learned.

## What's in this repo

- **[`SKILL.md`](SKILL.md)** — the skill itself. Covers the initial build sequence, 14 hard-won lessons (each with the specific failure mode it prevents), a live-site revision workflow (preview-file discipline so nothing half-approved ships), verification pitfalls, CSS/JS gotchas, and common post-launch automation requests.
- **[`MASTER_BUILD_PROMPT.md`](MASTER_BUILD_PROMPT.md)** — a fill-in-the-blanks kickoff prompt for a new client build, plus a structured brand-voice discovery interview (18 questions designed to extract how a founder actually talks, not how they think they should sound).
- **[`examples/ExampleClient_DesignSystem.md`](examples/ExampleClient_DesignSystem.md)** — a sanitized example of the design-system document this skill produces: exact colors, type scale, spacing tokens, and component patterns pulled from a live codebase rather than invented, for handoff to design tooling.

## A few representative lessons

Not a summary of the whole thing — just enough to show the level of specificity:

- A "paid Google Workspace account" being active doesn't mean a domain's mail DNS actually resolves — verify with a live `dig` lookup before publishing any contact email, because the failure mode is silent (mail reports "sent" while never arriving).
- `grid-template-columns: repeat(auto-fit, minmax(Xpx, 1fr))` lets a lone item in a row balloon to fill it while multi-item rows look normal — use `auto-fill` with capped track sizes instead when tile size must stay consistent.
- Every approved revision is staged in `-preview` file copies first and only promoted to live files on explicit approval — never edit live files directly, and grep the promoted file for leftover `-preview` references before considering a push done.

## Using it

Paste `SKILL.md` at the start of any Claude Code session doing client website work. Fill out and paste `MASTER_BUILD_PROMPT.md` before a new build starts. Update `SKILL.md` itself whenever a new mistake or confirmed-good pattern shows up — it's meant to keep growing, not stay static.
