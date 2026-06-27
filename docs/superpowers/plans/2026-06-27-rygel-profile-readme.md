# rygel profile README — implementation plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the default GitHub profile template in `README.md` with the curated 8-section ecosystem page designed in `docs/superpowers/specs/2026-06-27-rygel-profile-readme-design.md`.

**Architecture:** Single-file markdown replacement. No code, no build step, no tests in the traditional sense — verification is URL reachability and link integrity. Hero images, badges, and repo stats all served from external endpoints GitHub already proxies.

**Tech Stack:** GitHub Flavored Markdown, shields.io, komarev.com, raw.githubusercontent.com.

---

## File structure

| File | Action | Purpose |
|---|---|---|
| `README.md` | **Modify** (replace contents) | The profile README that GitHub renders on the profile page |

No other files touched. No assets committed to this repo — everything is external.

**Note on worktrees:** This plan runs in the current working tree, not an isolated worktree. The change is single-file content with no risk to other code, and the user has been committing directly to `main` throughout this session.

---

## Task 1: Verify external URLs

**Files:** None (verification task)

Reachability of every external URL referenced from the new `README.md`. Failures block Task 2.

- [ ] **Step 1: Verify hero image URLs return HTTP 200**

Run:

```powershell
$urls = @(
    "https://raw.githubusercontent.com/rygel/AIUsageTracker/main/docs/screenshot_dashboard_privacy.png",
    "https://raw.githubusercontent.com/rygel/needlecast/main/docs/screenshots/01-main-window.png"
)
foreach ($u in $urls) {
    $r = Invoke-WebRequest -Uri $u -Method Head -UseBasicParsing -ErrorAction Stop
    Write-Host "$($r.StatusCode) $u"
}
```

Expected: both return `200`. If any returns non-200, STOP — the URL in the README will render as a broken image.

- [ ] **Step 2: Verify shields.io endpoint pattern works**

Run:

```powershell
$r = Invoke-WebRequest -Uri "https://img.shields.io/github/followers/rygel?style=for-the-badge" -Method Head -UseBasicParsing -ErrorAction Stop
Write-Host "$($r.StatusCode) $($r.Headers['Content-Type'][0])"
```

Expected: `200 image/svg+xml`. If shields.io is down, retry once; if still failing, STOP — every badge in the README depends on this service.

- [ ] **Step 3: Verify komarev endpoint pattern works**

Run:

```powershell
$r = Invoke-WebRequest -Uri "https://komarev.com/ghpvc/?username=rygel" -Method Head -UseBasicParsing -ErrorAction Stop
Write-Host "$($r.StatusCode) $($r.Headers['Content-Type'][0])"
```

Expected: `200 image/svg+xml` (or `image/png`). If failing, the view counter badge will be broken; flag and consider switching to the fallback noted in the spec.

- [ ] **Step 4: Verify each featured-repo link resolves**

Run:

```powershell
$repos = @(
    "rygel/AIUsageTracker",
    "rygel/needlecast",
    "rygel/ponytail",
    "rygel/Project-Foundations",
    "rygel/chalie",
    "rygel/outerstellar-platform",
    "rygel/outerstellar-framework",
    "rygel/fragments4k",
    "rygel/sparkle4j",
    "rygel/update4j",
    "outerstellar-hq/maven-cleaner",
    "outerstellar-hq/skills"
)
foreach ($r in $repos) {
    $code = (Invoke-WebRequest -Uri "https://github.com/$r" -Method Head -UseBasicParsing -ErrorAction SilentlyContinue).StatusCode
    Write-Host "$code $r"
}
```

Expected: every line shows `200`. If any shows `404`, the README has a broken link — STOP and either fix the path or remove the entry.

- [ ] **Step 5: Verify upstream contribution links resolve**

Run:

```powershell
$upstreams = @(
    "DietrichGebert/ponytail",
    "erophames/Project-Foundations",
    "chalie-ai/chalie"
)
foreach ($u in $upstreams) {
    $code = (Invoke-WebRequest -Uri "https://github.com/$u" -Method Head -UseBasicParsing -ErrorAction SilentlyContinue).StatusCode
    Write-Host "$code $u"
}
```

Expected: `200` for each. STOP if any are not found — the contributions section claims contribution to a repo that does not exist.

- [ ] **Step 6: Commit verification log (optional)**

If any checks failed, the task is blocked. If all passed, no commit needed — verification is just a precondition for Task 2.

---

## Task 2: Write README.md

**Files:**
- Modify: `README.md` (full content replacement)

- [ ] **Step 1: Read current `README.md` to confirm it's the GitHub default**

Run:

```powershell
Get-Content -LiteralPath "C:\Develop\Claude\projects\personal\rygel\README.md"
```

Expected: contains `## Hi there 👋` and the bullet list of `🔭`, `🌱`, `👯`, etc. — confirms it's the default template and we're replacing the whole file.

- [ ] **Step 2: Replace `README.md` with the new content**

Write the file in full using the content block below. The block is the literal final markdown — copy verbatim, do not paraphrase.

```markdown
<!-- rygel/rygel — profile README -->

# rygel

**Software Architect | Agentic Coding | Pragmatic Delivery**

<p align="left">
  <a href="https://github.com/rygel?tab=followers"><img src="https://img.shields.io/github/followers/rygel?style=for-the-badge&color=0f172a" alt="GitHub followers"></a>
  <a href="https://github.com/rygel"><img src="https://komarev.com/ghpvc/?username=rygel&style=for-the-badge&color=0f172a" alt="Profile views"></a>
  <a href="https://github.com/rygel?tab=repositories"><img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fapi.github.com%2Fusers%2Frygel&query=%24.public_repos&style=for-the-badge&color=0f172a&label=public%20repos" alt="Public repos"></a>
  <a href="https://github.com/rygel?tab=repositories"><img src="https://img.shields.io/badge/stack-Kotlin%20%C2%B7%20C%23%20%C2%B7%20Python-2f80ed?style=for-the-badge" alt="Main Stack"></a>
  <a href="https://github.com/rygel?tab=repositories"><img src="https://img.shields.io/badge/focus-Agentic%20Coding-0f172a?style=for-the-badge" alt="Focus"></a>
</p>

---

## `mission`

I build the tools I like to use.

---

## `flagship` · [AIUsageTracker](https://github.com/rygel/AIUsageTracker)

<p align="left">
  <a href="https://github.com/rygel/AIUsageTracker"><img src="https://raw.githubusercontent.com/rygel/AIUsageTracker/main/docs/screenshot_dashboard_privacy.png" alt="AIUsageTracker dashboard screenshot"></a>
</p>

<p align="left">
  <a href="https://github.com/rygel/AIUsageTracker/releases"><img src="https://img.shields.io/github/v/release/rygel/AIUsageTracker?style=for-the-badge&color=7c3aed" alt="Release"></a>
  <a href="https://github.com/rygel/AIUsageTracker/stargazers"><img src="https://img.shields.io/github/stars/rygel/AIUsageTracker?style=for-the-badge&color=fac315&logo=github" alt="Stars"></a>
  <a href="https://github.com/rygel/AIUsageTracker/releases"><img src="https://img.shields.io/github/downloads/rygel/AIUsageTracker/total?style=for-the-badge" alt="Downloads"></a>
  <a href="https://github.com/rygel/AIUsageTracker/blob/main/LICENSE"><img src="https://img.shields.io/github/license/rygel/AIUsageTracker?style=for-the-badge&color=22c55e" alt="License"></a>
  <a href="https://github.com/rygel/AIUsageTracker"><img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux-blue?style=for-the-badge" alt="Platform"></a>
  <a href="https://github.com/rygel/AIUsageTracker"><img src="https://img.shields.io/badge/language-C%23%20%2F%20.NET-purple?style=for-the-badge" alt="Language"></a>
</p>

Windows dashboard and tray utility for monitoring AI API usage, costs, and quotas across multiple providers — OpenAI, Anthropic, Gemini, DeepSeek, and more.

[**→ rygel/AIUsageTracker on GitHub**](https://github.com/rygel/AIUsageTracker) — primary install source (release page holds the Windows installer and portable zip).

Secondary install hint — Windows Package Manager (one-liner, copy-pastable):
`winget install AlexanderBrandt.AIConsumptionTracker`

---

## `agentic_coding_apps` · [needlecast](https://github.com/rygel/needlecast)

<p align="left">
  <a href="https://github.com/rygel/needlecast"><img src="https://raw.githubusercontent.com/rygel/needlecast/main/docs/screenshots/01-main-window.png" alt="needlecast main window"></a>
</p>

<p align="left">
  <a href="https://github.com/rygel/needlecast/releases"><img src="https://img.shields.io/github/v/release/rygel/needlecast?style=for-the-badge&color=7c3aed" alt="Release"></a>
  <a href="https://github.com/rygel/needlecast/stargazers"><img src="https://img.shields.io/github/stars/rygel/needlecast?style=for-the-badge&color=fac315&logo=github" alt="Stars"></a>
  <a href="https://github.com/rygel/needlecast/blob/main/LICENSE"><img src="https://img.shields.io/github/license/rygel/needlecast?style=for-the-badge&color=22c55e" alt="License"></a>
  <a href="https://github.com/rygel/needlecast"><img src="https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-blue?style=for-the-badge" alt="Platform"></a>
  <a href="https://github.com/rygel/needlecast"><img src="https://img.shields.io/badge/language-Kotlin-purple?style=for-the-badge" alt="Language"></a>
</p>

Desktop shell for vibe-coding CLIs. Sits between a CLI and a full IDE — project tree, terminal, editor, image viewer, log tail, renovate panel.

---

## `contributions`

Three upstream projects I contribute to:

- [**`rygel/ponytail`**](https://github.com/rygel/ponytail) — contributed to [`DietrichGebert/ponytail`](https://github.com/DietrichGebert/ponytail). "Lazy senior dev" skill for AI agents — 80–94% less code, 47–77% cheaper, 3–6× faster. Injected as a Codex / Claude / OpenCode plugin.
- [**`rygel/Project-Foundations`**](https://github.com/rygel/Project-Foundations) — contributed to [`erophames/Project-Foundations`](https://github.com/erophames/Project-Foundations). Codex/Claude plugin with language-specific project setup defaults (Python, Kotlin, C#, Rust, Go, TypeScript, etc.).
- [**`rygel/chalie`**](https://github.com/rygel/chalie) — contributed to [`chalie-ai/chalie`](https://github.com/chalie-ai/chalie). Self-hosted AI companion with persistent memory, autonomous goals, local-first data.

---

## `platforms_and_frameworks`

- [**`outerstellar-platform`**](https://github.com/rygel/outerstellar-platform) — Kotlin application platform for extension-driven web and desktop products.
- [**`outerstellar-framework`**](https://github.com/rygel/outerstellar-framework) — Companion framework for the outerstellar platform.
- [**`fragments4k`**](https://github.com/rygel/fragments4k) — Framework-agnostic Markdown content library with adapters for HTTP4k, Javalin, Spring Boot, Quarkus, Micronaut.
- [**`sparkle4j`**](https://github.com/rygel/sparkle4j) — JavaFX auto-update framework for desktop applications.
- [**`maven-cleaner`**](https://github.com/outerstellar-hq/maven-cleaner) — Tool to clean local Maven repository and Gradle caches safely — upstream-verified, local-only builds protected.
- [**`skills`**](https://github.com/outerstellar-hq/skills) — Reusable agent skills for code analysis, auditing, and quality automation.

---

## `toolchain`

<p align="left">
  <a href="https://kotlinlang.org"><img src="https://img.shields.io/badge/Kotlin-0f172a?style=for-the-badge&logo=kotlin&logoColor=white" alt="Kotlin"></a>
  <a href="https://dotnet.microsoft.com/languages/csharp"><img src="https://img.shields.io/badge/C%23-0f172a?style=for-the-badge&logo=csharp&logoColor=white" alt="C#"></a>
  <a href="https://www.java.com"><img src="https://img.shields.io/badge/Java-0f172a?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java"></a>
</p>

<p align="left">
  <a href="https://www.http4k.org"><img src="https://img.shields.io/badge/HTTP4k-0f172a?style=for-the-badge" alt="HTTP4k"></a>
  <a href="https://jte.gg"><img src="https://img.shields.io/badge/JTE-0f172a?style=for-the-badge" alt="JTE"></a>
  <a href="https://javalin.io"><img src="https://img.shields.io/badge/Javalin-0f172a?style=for-the-badge" alt="Javalin"></a>
  <a href="https://htmx.org"><img src="https://img.shields.io/badge/htmx-0f172a?style=for-the-badge" alt="htmx"></a>
  <a href="https://maven.apache.org"><img src="https://img.shields.io/badge/Maven-0f172a?style=for-the-badge&logo=apachemaven&logoColor=white" alt="Maven"></a>
  <a href="https://flywaydb.org"><img src="https://img.shields.io/badge/Flyway-0f172a?style=for-the-badge&logo=flyway&logoColor=white" alt="Flyway"></a>
  <a href="https://jdbi.org"><img src="https://img.shields.io/badge/JDBI-0f172a?style=for-the-badge" alt="JDBI"></a>
  <a href="https://docs.oracle.com/javase/tutorial/uiswing"><img src="https://img.shields.io/badge/Swing-0f172a?style=for-the-badge" alt="Swing"></a>
  <a href="https://www.docker.com"><img src="https://img.shields.io/badge/Docker-0f172a?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"></a>
</p>

<p align="left">
  <a href="https://claude.com/product/claude-code"><img src="https://img.shields.io/badge/Claude%20Code-0f172a?style=for-the-badge" alt="Claude Code"></a>
  <a href="https://github.com/openai/codex"><img src="https://img.shields.io/badge/Codex-0f172a?style=for-the-badge" alt="Codex"></a>
  <a href="https://opencode.ai"><img src="https://img.shields.io/badge/OpenCode-0f172a?style=for-the-badge" alt="OpenCode"></a>
</p>

---

## `github_analytics`

### ⭐ project star ranking — top 7 owner repos

| Rank | Project | Stars | Forks | Focus |
|---|---|:---:|:---:|---|
| 1 | [`AIUsageTracker`](https://github.com/rygel/AIUsageTracker) | ![Stars](https://img.shields.io/github/stars/rygel/AIUsageTracker?style=flat-square&label=stars&logo=github) | ![Forks](https://img.shields.io/github/forks/rygel/AIUsageTracker?style=flat-square&label=forks&logo=github) | Token usage tracker for (not only) OpenCode |
| 2 | [`needlecast`](https://github.com/rygel/needlecast) | ![Stars](https://img.shields.io/github/stars/rygel/needlecast?style=flat-square&label=stars&logo=github) | ![Forks](https://img.shields.io/github/forks/rygel/needlecast?style=flat-square&label=forks&logo=github) | Desktop shell for vibe-coding CLIs |
| 3 | [`sparkle4j`](https://github.com/rygel/sparkle4j) | ![Stars](https://img.shields.io/github/stars/rygel/sparkle4j?style=flat-square&label=stars&logo=github) | ![Forks](https://img.shields.io/github/forks/rygel/sparkle4j?style=flat-square&label=forks&logo=github) | JavaFX auto-update framework |
| 4 | [`outerstellar-platform`](https://github.com/rygel/outerstellar-platform) | ![Stars](https://img.shields.io/github/stars/rygel/outerstellar-platform?style=flat-square&label=stars&logo=github) | ![Forks](https://img.shields.io/github/forks/rygel/outerstellar-platform?style=flat-square&label=forks&logo=github) | Kotlin extension-driven application platform |
| 5 | [`outerstellar-framework`](https://github.com/rygel/outerstellar-framework) | ![Stars](https://img.shields.io/github/stars/rygel/outerstellar-framework?style=flat-square&label=stars&logo=github) | ![Forks](https://img.shields.io/github/forks/rygel/outerstellar-framework?style=flat-square&label=forks&logo=github) | Companion framework |
| 6 | [`update4j`](https://github.com/rygel/update4j) | ![Stars](https://img.shields.io/github/stars/rygel/update4j?style=flat-square&label=stars&logo=github) | ![Forks](https://img.shields.io/github/forks/rygel/update4j?style=flat-square&label=forks&logo=github) | Auto-update framework (demoted, retained for completeness) |
| 7 | [`fragments4k`](https://github.com/rygel/fragments4k) | ![Stars](https://img.shields.io/github/stars/rygel/fragments4k?style=flat-square&label=stars&logo=github) | ![Forks](https://img.shields.io/github/forks/rygel/fragments4k?style=flat-square&label=forks&logo=github) | Markdown content library for Kotlin/JVM |

[more repositories →](https://github.com/rygel?tab=repositories)
```

- [ ] **Step 3: Confirm the file was written**

Run:

```powershell
(Get-Content -LiteralPath "C:\Develop\Claude\projects\personal\rygel\README.md").Count
```

Expected: at least 100 lines (the new content is ~135 lines). If the count is ~16, the GitHub default is still there — re-check the write.

---

## Task 3: Self-verify the rendered output

**Files:** None (verification task)

- [ ] **Step 1: Re-verify hero images load (regression check)**

Run:

```powershell
$urls = @(
    "https://raw.githubusercontent.com/rygel/AIUsageTracker/main/docs/screenshot_dashboard_privacy.png",
    "https://raw.githubusercontent.com/rygel/needlecast/main/docs/screenshots/01-main-window.png"
)
foreach ($u in $urls) {
    $r = Invoke-WebRequest -Uri $u -Method Head -UseBasicParsing -ErrorAction Stop
    Write-Host "$($r.StatusCode) $u"
}
```

Expected: both `200`.

- [ ] **Step 2: Re-verify every shields.io badge URL pattern returns SVG**

Extract every `img.shields.io` URL from `README.md` and HEAD-check each. One block, all URLs in one pass:

```powershell
$content = Get-Content -LiteralPath "C:\Develop\Claude\projects\personal\rygel\README.md" -Raw
$pattern = 'https://img\.shields\.io/[^")]+'
$urls = [regex]::Matches($content, $pattern) | ForEach-Object { $_.Value } | Sort-Object -Unique
foreach ($u in $urls) {
    $r = Invoke-WebRequest -Uri $u -Method Head -UseBasicParsing -ErrorAction SilentlyContinue
    Write-Host "$($r.StatusCode) $u"
}
```

Expected: every line shows `200`. If any returns non-200, that badge URL is malformed — fix in `README.md` and re-run.

- [ ] **Step 3: Re-verify komarev view counter**

Run:

```powershell
$r = Invoke-WebRequest -Uri "https://komarev.com/ghpvc/?username=rygel" -Method Head -UseBasicParsing -ErrorAction Stop
Write-Host "$($r.StatusCode)"
```

Expected: `200`.

- [ ] **Step 4: Sanity-check the file structure**

Run:

```powershell
Get-Content -LiteralPath "C:\Develop\Claude\projects\personal\rygel\README.md" |
  Select-String -Pattern '^## ' |
  ForEach-Object { $_.Line }
```

Expected output (8 section headers, in order):

```
## `mission`
## `flagship` · [AIUsageTracker](https://github.com/rygel/AIUsageTracker)
## `agentic_coding_apps` · [needlecast](https://github.com/rygel/needlecast)
## `contributions`
## `platforms_and_frameworks`
## `toolchain`
## `github_analytics`
```

If sections are missing or out of order, STOP and correct the file.

- [ ] **Step 5: Count `---` separators (visual dividers)**

Run:

```powershell
(Get-Content -LiteralPath "C:\Develop\Claude\projects\personal\rygel\README.md" | Select-String -Pattern '^---$' | Measure-Object).Count
```

Expected: `7` — one between each pair of adjacent sections plus after the header.

---

## Task 4: Commit

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Stage and commit**

Run:

```powershell
git -C "C:\Develop\Claude\projects\personal\rygel" add README.md
git -C "C:\Develop\Claude\projects\personal\rygel" -c user.email="agent@local" -c user.name="agent" commit -m "Replace profile README with curated 8-section ecosystem page"
```

Expected: commit succeeds. If git complains about identity, the user should run `git config user.email` and `user.name` first; the `-c` flags here provide a fallback for the local repo only.

- [ ] **Step 2: Confirm commit landed**

Run:

```powershell
git -C "C:\Develop\Claude\projects\personal\rygel" log --oneline -3
git -C "C:\Develop\Claude\projects\personal\rygel" status
```

Expected: latest commit is the README replacement. `git status` shows working tree clean.

- [ ] **Step 3: STOP — do not push**

Per project conventions (server access is uploads-only, no pushes without explicit instruction), this plan stops at the local commit. Push is a separate decision the user must make explicitly.
