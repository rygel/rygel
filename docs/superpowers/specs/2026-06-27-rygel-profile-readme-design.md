# rygel profile README — design spec

**Date:** 2026-06-27
**Status:** Draft, awaiting user review
**Owner:** rygel

## Context

Replace the default GitHub profile README in `rygel/rygel` with a curated profile page modeled on [wesleysimplicio/wesleysimplicio](https://github.com/wesleysimplicio/wesleysimplicio), adapted for an ecosystem rather than a single-hero page.

The page is the visitor's first impression when they land on the GitHub profile. It should:

- Make the tagline and mission immediately legible
- Anchor on the one project with real social proof (AIUsageTracker, 35 stars)
- Surface the active agentic-coding layer and supporting platforms
- Credit upstream contributions where the user has contributed but does not own
- Stay truthful — no inflated numbers, no AI-fabricated screenshots, no projects the user does not actively use

## Decisions locked during brainstorming

| # | Decision | Value |
|---|---|---|
| 1 | Tagline | `Software Architect \| Agentic Coding \| Pragmatic Delivery` |
| 2 | Mission statement | `I build the tools I like to use.` |
| 3 | Hero images | Only real product screenshots that already exist in the source repos. No AI-generated visuals. |
| 4 | Forks | All forks ignored as the user's own work. `ponytail`, `Project-Foundations`, `chalie` mentioned in a separate `contributions` block as upstream contributions. |
| 5 | Old Java era (2014–2018) | Omitted entirely. No footnote, no link. |
| 6 | GitHub stats cards (`github-readme-stats`) | Skipped. Public Vercel instance is best-effort and unreliable; cards add noise without signal. |
| 7 | Multi-language READMEs | English only. No translation copies. |
| 8 | `signal` values list | Skipped. |
| 9 | Mentor acknowledgment | Skipped. |
| 10 | `update4j` | Demoted from featured sections. Stays in star ranking. |
| 11 | Section names | `mission`, `flagship`, `agentic_coding_apps`, `contributions`, `platforms_and_frameworks`, `toolchain`, `github_analytics` |

## Final structure

Eight sections, in this order:

```
1. Header
2. mission
3. flagship
4. agentic_coding_apps
5. contributions
6. platforms_and_frameworks
7. toolchain
8. github_analytics
```

Aesthetic matches the reference: shields.io badges, monospace backtick section headers (`## `section``), centered `---` separators between sections.

---

## Section 1 — Header

```
# rygel
**Software Architect | Agentic Coding | Pragmatic Delivery**
```

Badge row (shields.io, all dynamic):

| Badge | Style |
|---|---|
| GitHub followers | `for-the-badge`, color `0f172a` |
| Profile views (komarev) | `for-the-badge`, color `0f172a` |
| Public repos | `for-the-badge`, color `0f172a` |
| Total stars | `for-the-badge`, color `0f172a` |
| Main stack | `Kotlin · C# · Python`, `for-the-badge`, color `2f80ed` |
| Focus | `Agentic Coding`, `for-the-badge`, color `0f172a` |

All badges link to `https://github.com/rygel?tab=...` (followers / repositories).

## Section 2 — `mission`

Single sentence, no embellishment:

> I build the tools I like to use.

This is the user's verbatim mission. No additional context paragraph needed — the next sections show what they built.

## Section 3 — `flagship`

**`AIUsageTracker`** — `https://github.com/rygel/AIUsageTracker`

Hero image (verified 200, 95 KB):
`https://raw.githubusercontent.com/rygel/AIUsageTracker/main/docs/screenshot_dashboard_privacy.png`

Badge row:
- `release` (shields.io, dynamic from GitHub releases)
- `stars` (for-the-badge, color `fac315`)
- `downloads` (total, dynamic)
- `license:MIT`
- `platform:Windows | Linux`
- `language:C#/.NET`

Pitch paragraph (~25 words):
> Windows dashboard and tray utility for monitoring AI API usage, costs, and quotas across multiple providers — OpenAI, Anthropic, Gemini, DeepSeek, and more.

Primary install source — link to the GitHub repo (releases page holds the Windows installer and portable zip):
[`→ rygel/AIUsageTracker on GitHub`](https://github.com/rygel/AIUsageTracker)

Secondary install hint — Windows Package Manager (one-liner, copy-pastable):
`winget install AlexanderBrandt.AIConsumptionTracker`

## Section 4 — `agentic_coding_apps`

**`needlecast`** — `https://github.com/rygel/needlecast`

Hero image (verified 200, 86 KB):
`https://raw.githubusercontent.com/rygel/needlecast/main/docs/screenshots/01-main-window.png`

Badge row:
- `release` (dynamic)
- `stars` (for-the-badge, color `fac315`)
- `license:MIT`
- `platform:Windows | macOS | Linux`
- `language:Kotlin`

Pitch (~25 words):
> Desktop shell for vibe-coding CLIs. Sits between a CLI and a full IDE — project tree, terminal, editor, image viewer, log tail, renovate panel.

## Section 5 — `contributions`

Badge-only cards, no hero images. Three projects the user contributed to upstream but does not own:

| Project | Upstream | Pitch |
|---|---|---|
| `ponytail` | `DietrichGebert/ponytail` | "Lazy senior dev" skill for AI agents — 80–94% less code, 47–77% cheaper, 3–6× faster. Injected as a Codex / Claude / OpenCode plugin. |
| `Project-Foundations` | `erophames/Project-Foundations` | Codex/Claude plugin with language-specific project setup defaults (Python, Kotlin, C#, Rust, Go, TypeScript, etc.). |
| `chalie` | `chalie-ai/chalie` | Self-hosted AI companion with persistent memory, autonomous goals, local-first data. |

Each entry: bold link to the user's fork on `rygel/` + 1-sentence pitch + a small annotation reading `contributed to <upstream>`. The fork link makes the source concrete; the annotation frames it as contribution rather than ownership.

## Section 6 — `platforms_and_frameworks`

Six projects, badge-only grid (Wesley's pattern for secondary sections).

| Project | Org | Language | One-line pitch |
|---|---|---|---|
| `outerstellar-platform` | `rygel/` | Kotlin | Kotlin application platform for extension-driven web and desktop products. |
| `outerstellar-framework` | `rygel/` | Kotlin | Companion framework for the outerstellar platform. |
| `fragments4k` | `rygel/` | Kotlin | Framework-agnostic Markdown content library with adapters for HTTP4k, Javalin, Spring Boot, Quarkus, Micronaut. |
| `sparkle4j` | `rygel/` | Java | JavaFX auto-update framework for desktop applications. |
| `maven-cleaner` | `outerstellar-hq/` | Kotlin | Tool to clean local Maven repository and Gradle caches safely — upstream-verified, local-only builds protected. |
| `skills` | `outerstellar-hq/` | Python | Reusable agent skills for code analysis, auditing, and quality automation. |

## Section 7 — `toolchain`

Shields.io badges in `for-the-badge` style, dark grey (`0f172a`), no logos (cleaner).

**Languages:**
- Kotlin · C# · Java

**Stack:**
- HTTP4k · JTE · Javalin · htmx · Maven · Flyway · JDBI · Swing · Docker

**Agents:**
- Claude Code · Codex · OpenCode

Each badge links to a relevant docs page or the upstream project.

## Section 8 — `github_analytics`

Star ranking table — owner repos only (not forks, not org repos). Sorted by stars desc, ties broken by commit count.

Columns: Rank | Project | Stars | Forks | Focus

| Rank | Project | Stars | Forks | Focus |
|---|---|---|---|---|
| 1 | `AIUsageTracker` | 35 | 2 | Token usage tracker for (not only) OpenCode |
| 2 | `needlecast` | 5 | 1 | Desktop shell for vibe-coding CLIs |
| 3 | `sparkle4j` | 0 | 0 | JavaFX auto-update framework |
| 4 | `outerstellar-platform` | 0 | 0 | Kotlin extension-driven application platform |
| 5 | `outerstellar-framework` | 0 | 0 | Companion framework |
| 6 | `update4j` | 0 | 0 | Auto-update framework (demoted, retained for completeness) |
| 7 | `fragments4k` | 0 | 0 | Markdown content library for Kotlin/JVM |

Stars and forks rendered as dynamic shields.io badges (`style=flat-square&label=stars&logo=github`). Rank numbers use emoji prefix (🚀 for rank 1, then plain digits 2–7) — matches Wesley's pattern.

A short footer line links to the full repos tab: `[more repositories](https://github.com/rygel?tab=repositories)`.

---

## Open items to confirm during implementation

These are specifics I can finalize once the structure is approved; flag any objections now:

1. **Header view-count badge:** resolved — `komarev.com`.
2. **AIUsageTracker install hint:** resolved — primary link is the GitHub repo; winget install is secondary.
3. **`toolchain` row count:** resolved — see Section 7. Final list is 15 badges across 3 rows (languages, stack, agents).
4. **Star-ranking table footer:** resolved — `[more repositories →](https://github.com/rygel?tab=repositories)`.

## Implementation notes

- Single file: `README.md` in repo root, replaces the GitHub-default template.
- Hero images and badges all served from existing URLs — no assets committed to the profile repo.
- Shields.io URLs follow the standard pattern: `https://img.shields.io/github/...&style=for-the-badge`.
- Section headers use Wesley's aesthetic: `## \`section_name\`` (backticks render in monospace, anchors via `#section_name`).
- Document width: GitHub renders centered at ~1012 px. Hero images will scale to fit.
- No JavaScript. No external trackers. No third-party CDN beyond shields.io and the existing GitHub raw image hosts.
