# gstack Dictionary — Plain English Reference

> Written for Santiago. No assumed technical background.
> Every term you'll encounter in this repo, explained from first principles.
> Terms marked with **[YOUR WORK]** are especially relevant to Synthesis Intelligence or DolarApp.

---

## A

**Agent**
An AI that doesn't just answer questions — it takes actions: reads files, runs code, clicks through a website, creates PRs. Think of Claude Code as an agent: you give it a goal, it figures out the steps. In gstack, each "skill" gives Claude a specific agent *role* (CEO, engineer, QA lead, etc.).
→ See: `README.md`, `AGENTS.md`

**Agent SDK**
The official toolkit for building AI agents programmatically. You can wire up Claude to do specific tasks automatically using the Anthropic Agent SDK. gstack is built *on top of* this — you don't need to use the SDK directly unless you're building your own tools.
→ See: `test/helpers/session-runner.ts`

**ARIA Tree / Accessibility Tree**
Every web page has a hidden structure that screen readers use — this is the accessibility tree. It lists every button, link, form field, and heading in a structured format. gstack's browser reads this tree to understand what's on a page without having to "look" at it visually. Think of it as a map of a page that lists only the interactive parts.
→ See: `ARCHITECTURE.md`, `browse/src/snapshot.ts`

**@ref / @e1, @e2 / @c1, @c2**
When Claude takes a snapshot of a webpage, every element gets a unique label: `@e1` (button), `@e2` (link), etc. Instead of Claude trying to describe where to click ("the blue button near the top left"), it just says "click @e3." Much more reliable. `@c` refs are for elements that look clickable but aren't in the accessibility tree (custom-coded buttons).
→ See: `ARCHITECTURE.md` (The ref system section)

**Allowed-tools**
In a SKILL.md file, this is the list of things Claude is permitted to do during that skill. For example, `allowed-tools: [Bash, Read, Write, Edit, Grep]`. It's a safety guardrail — the security skill (/cso) can read files but might not be allowed to edit them.
→ See: Any `SKILL.md` file

**Anthropic**
The company that makes Claude. Anthropic also makes Claude Code (the CLI tool), the Agent SDK, and the API that gstack uses for evaluations.

**Autoplan**
A gstack skill (`/autoplan`) that chains together the CEO review + design review + engineering review automatically. Instead of you answering 30 questions across 3 skills, it answers most of them itself and only asks you the subjective "taste" decisions.
**[YOUR WORK]** Perfect starting point for Synthesis Intelligence features.
→ See: `autoplan/SKILL.md`

---

## B

**Base branch**
The main branch of your code repository — usually called `main` or `master`. When you make changes, you branch off from here, work, then merge back. Many gstack skills need to detect the base branch to know what changed (only review what's new, not the whole codebase).
→ See: `CLAUDE.md` (BASE_BRANCH_DETECT placeholder)

**Bearer Token**
A secret password that proves you're allowed to talk to the browse server. It's randomly generated each session. You don't need to manage it — gstack handles it automatically.
→ See: `ARCHITECTURE.md` (Security model section)

**Binary / Compiled Binary**
The browse tool in gstack is compiled into a single executable file (~58MB). Think of it like an app you install — you double-click it and it runs, without needing anything else installed. This makes it work reliably on any machine.
→ See: `ARCHITECTURE.md` (Why Bun section), `browse/dist/browse`

**Browse / $B**
gstack's headless browser — a real Chrome browser that runs without a visible window. Claude controls it with commands like `$B goto https://example.com` or `$B screenshot`. It's fast (~100ms per command) because it stays running in the background.
**[YOUR WORK]** Useful for testing the Synthesis Intelligence UI. Also useful for capturing screenshots of the DolarApp KYC flow.
→ See: `BROWSER.md`, `browse/SKILL.md`

**Bun**
A fast JavaScript runtime (like Node.js, but faster). gstack uses it to run the browse server and to compile the browse binary. You just need it installed — you won't interact with it directly.
→ See: `ARCHITECTURE.md` (Why Bun section)

---

## C

**Canary**
In software, a "canary" is a monitoring check right after you deploy something — like a canary in a coal mine. If something's wrong, the canary catches it early. The `/canary` skill watches your live app for errors after you ship.
→ See: `canary/SKILL.md`

**CDP (Chrome DevTools Protocol)**
The technical language that Playwright uses to talk to Chromium. You never interact with this directly — it's the "plumbing" between gstack's server and the browser.
→ See: `ARCHITECTURE.md` diagram

**Chain**
A browse command that lets you run multiple browser actions in a single call. Instead of 10 separate tool calls to click through a form, you send one "chain" command with all the steps. Faster and uses fewer AI tokens.
→ See: `BROWSER.md`

**CHANGELOG.md**
A running log of every version of gstack and what changed. Written for users, not engineers — plain English about what you can now do that you couldn't before.
→ See: `CHANGELOG.md` in repo root

**claude -p**
Running Claude in "print mode" — Claude processes a prompt once and exits, instead of staying in interactive conversation mode. gstack's test infrastructure uses this to run skills automatically during testing.
→ See: `ARCHITECTURE.md` (Session runner section)

**CLAUDE.md**
The config file that tells Claude about your specific project — what test commands to use, what deploy commands to run, what frameworks you're using. Every gstack skill reads this file to adapt to your project instead of guessing.
**[YOUR WORK]** You'll want to create a good CLAUDE.md for Synthesis Intelligence before running skills.
→ See: `CLAUDE.md` in repo root

**Codex**
OpenAI's coding AI (not to be confused with Claude). The `/codex` skill uses Codex as a "second opinion" — an independent reviewer who doesn't know what Claude thought. Good for catching blind spots.
→ See: `codex/SKILL.md`

**Confidence Threshold**
In the `/cso` security skill, findings are only reported if the AI is confident enough (8 out of 10 confidence or higher). This prevents false alarms.
→ See: `cso/SKILL.md`

**CSO / /cso**
Chief Security Officer skill. Runs an automated security audit of your codebase — checks for common vulnerabilities (OWASP Top 10), threat modeling (STRIDE), secrets accidentally committed to code, supply chain risks.
**[YOUR WORK]** Critical before Synthesis Intelligence handles any real user data or financial information.
→ See: `cso/SKILL.md`

**Cursor-interactive (@c refs)**
Some clickable elements on web pages aren't properly labeled for accessibility — they're `<div>` tags styled to look like buttons. The `-C` snapshot flag finds these hidden clickables and assigns them `@c1`, `@c2` refs.
→ See: `ARCHITECTURE.md` (Cursor-interactive refs section)

---

## D

**Daemon**
A program that runs quietly in the background, waiting to be called. gstack's browse server is a daemon — it starts when you first use it, stays running between commands (so it's fast), and auto-shuts-down after 30 minutes of inactivity.
→ See: `ARCHITECTURE.md` (The daemon model section)

**Design Doc**
A document created by `/office-hours` that captures the product vision, implementation approaches, and decisions made during brainstorming. All downstream skills (CEO review, eng review) automatically read this file.
**[YOUR WORK]** Run `/office-hours` on Synthesis Intelligence to generate this.
→ See: `office-hours/SKILL.md`

**Diff / git diff**
Shows the differences between two versions of a file — what was added (green), what was removed (red). Many gstack skills analyze the diff to understand what changed and only review the new code.
→ See: `ARCHITECTURE.md`, many SKILL.md files

**Diff-based test selection**
Instead of running all tests every time, gstack looks at what files changed and only runs tests related to those files. Saves time and money. You can override with `EVALS_ALL=1` to run everything.
→ See: `test/helpers/touchfiles.ts`, `CLAUDE.md`

---

## E

**E2E (End-to-End) Test**
Tests that simulate a real user using the whole system — not just one function in isolation. gstack's E2E tests actually spin up Claude as a subprocess and have it run skills, then check if the output was correct. Expensive (~$3.85/run) but the most realistic test possible.
→ See: `test/skill-e2e-*.test.ts`

**Eval / Evaluation**
A test that uses an LLM (like Claude) to judge whether something is good quality. Different from regular tests that check exact values — evals check things like "is this explanation clear?" or "did this security audit catch the planted bug?"
→ See: `test/skill-llm-eval.test.ts`, `test/helpers/llm-judge.ts`

**Eval Baseline**
A saved record of "this is how well the system performed last time." Future runs compare against this baseline to detect regressions (things getting worse) or improvements.
→ See: `test/fixtures/eval-baselines.json`

---

## F

**Freeze / /freeze**
A safety skill that locks Claude to only edit files in one specific directory. Useful when you want to fix a bug in one module without accidentally touching other code.
→ See: `freeze/SKILL.md`

**Funnel**
**[YOUR WORK]** A sequence of steps users must complete. In the DolarApp KYC context, the funnel is: start KYC → upload ID → take selfie → pass all checks → complete. The "funnel pass rate" is the percentage of users who complete all steps. It's dropping — your job is to find out why.

---

## G

**git / GitHub**
Version control tools. Git tracks changes to your code over time. GitHub hosts your code online and provides features like pull requests. gstack integrates deeply with both.

**Guard / /guard**
The combination of `/careful` (warns before destructive actions) + `/freeze` (locks edit scope) in one command. Maximum safety mode for working in production.
→ See: `guard/SKILL.md`

**gstack**
Garry Tan's open-source AI engineering workflow system. A set of "skills" (specialized roles) for Claude Code that turn a single developer into a full engineering team. Think: CEO, engineer, designer, QA lead, security officer — all automated.
→ See: `README.md`

---

## H

**Handoff**
A browse command that transfers control of the headless browser to a visible Chrome window — so a human can take over and do something manually (like solve a CAPTCHA), then hand it back to Claude.
→ See: `BROWSER.md`

**Hook**
An automated action that runs when a specific event happens. For example, the `/careful` skill uses a hook that fires *before* any destructive command runs, to warn you.
→ See: `careful/SKILL.md`, `CLAUDE.md` settings

---

## I

**Investigate / /investigate**
The root-cause debugging skill. Follows 4 phases: investigate (gather evidence), analyze (find the cause), hypothesize (test theories), implement (fix it). The Iron Law: never fix before understanding why it broke.
**[YOUR WORK]** Use this methodology for the DolarApp case — investigate each KYC check failure before proposing solutions.
→ See: `investigate/SKILL.md`

---

## K

**KYC (Know Your Customer)**
**[YOUR WORK]** A legal requirement for financial platforms. Before allowing someone to use a financial service, you must verify their identity. ARQ's KYC process involves: (1) ID verification — checking the ID document is real, (2) Liveness check — confirming the selfie is a live person, not a photo of a photo, (3) Identity verification — matching the selfie to the ID, (4) Watchlist screening — checking the person isn't on sanctions or crime lists.

**Keychain (macOS)**
The macOS password manager. gstack uses it to securely access cookies from your real browsers (Chrome, Arc, etc.) when you run the cookie import command. It prompts you to click "Allow" — gstack never accesses it silently.
→ See: `ARCHITECTURE.md` (Cookie security section)

---

## L

**Land-and-deploy / /land-and-deploy**
The skill that runs *after* `/ship`. Takes the PR that `/ship` created, merges it, waits for CI to pass, deploys to production, and runs canary checks to verify the live site is healthy.
→ See: `land-and-deploy/SKILL.md`

**Layer 1 / Layer 2 / Layer 3 (Three Layers of Knowledge)**
Garry Tan's framework for approaching any technical decision:
- Layer 1: Established, battle-tested approaches everyone uses
- Layer 2: Current trends, popular blog posts (scrutinize these)
- Layer 3: First-principles reasoning specific to your problem (most valuable)
**[YOUR WORK]** When building Synthesis Intelligence's data pipeline, search what exists (Layer 1+2) before building custom solutions. Then apply your unique insight about Mexican private markets (Layer 3).
→ See: `ETHOS.md`

**Liveness Verification**
**[YOUR WORK]** In KYC, this check ensures the selfie is a live person taking a photo in real time — not someone holding up a photo of someone else's face. Often done via "blink now" or "turn your head" prompts.

**LLM (Large Language Model)**
The underlying AI technology behind Claude, GPT, Gemini, etc. A model trained on massive amounts of text that can understand and generate language. In gstack, LLMs are used both as the agents doing the work AND as judges evaluating the quality of that work.

**LLM Judge**
Using an AI to evaluate the quality of another AI's output. gstack uses Claude Sonnet to score its own documentation on clarity, completeness, and actionability (1-5 scale). Catches quality regressions automatically.
→ See: `test/helpers/llm-judge.ts`

**Localhost**
The address `127.0.0.1` — your own computer talking to itself. gstack's browse server runs on localhost, meaning it's only accessible from your machine, not the internet.
→ See: `ARCHITECTURE.md` (Security model)

---

## M

**MCP (Model Context Protocol)**
A protocol for connecting AI models to external tools. gstack explicitly avoids MCP for the browse tool because plain HTTP is simpler and uses fewer tokens.
→ See: `ARCHITECTURE.md` (What's intentionally not here)

**Main branch**
The primary branch of a git repository, usually named `main` or `master`. What's on main is what's in production.

---

## N

**NDJSON (Newline-Delimited JSON)**
A data format where each line is a separate JSON object. gstack uses this for streaming output from `claude -p` — each line represents one event (tool call, message, etc.) as it happens in real time.
→ See: `test/helpers/session-runner.ts`

---

## O

**Office Hours / /office-hours**
The first skill to run on any new idea. Modeled after YC's "office hours" sessions where founders pressure-test their ideas. Asks 6 forcing questions that expose whether your idea is solving a real problem, for real people, in the right way.
**[YOUR WORK]** Run this on Synthesis Intelligence to sharpen the product thesis before building.
→ See: `office-hours/SKILL.md`

**OWASP Top 10**
The ten most common security vulnerabilities in software (SQL injection, XSS, authentication failures, etc.). The `/cso` skill checks your code against all ten.
**[YOUR WORK]** Financial platforms are high-value targets — run `/cso` before Synthesis Intelligence handles real money or user data.
→ See: `cso/SKILL.md`

---

## P

**Pass Rate**
**[YOUR WORK]** The percentage of users who start a process and complete it. For DolarApp KYC: how many users who begin KYC actually finish all steps and get approved. A declining pass rate means more users are dropping out — your case study is about finding exactly where and why.

**Playwright**
An open-source library that controls real web browsers programmatically. gstack's browse tool is built on Playwright. It lets Claude click, fill forms, take screenshots, and navigate websites as if it were a human user.
→ See: `ARCHITECTURE.md`, `browse/src/`

**Planted Bug**
A deliberately introduced bug in a test fixture, used to check if the QA skill can detect it. If `/qa` fails to find a planted bug, something is wrong with the skill.
→ See: `test/fixtures/`

**PR (Pull Request)**
A request to merge code from your branch into the main branch. GitHub shows the diff, runs CI checks, and allows team review before merging. The `/ship` skill creates PRs automatically.

**Preamble**
A block of context that gets injected at the start of every gstack skill. Contains: update check, session tracking (how many Claude windows are open), the builder ethos, and formatting instructions. You never edit this — it's auto-generated.
→ See: `ARCHITECTURE.md` (The preamble section)

**Private market / Mexican private market**
**[YOUR WORK]** Financial assets (companies, debt, real estate) that are not traded on public exchanges. In Mexico, 70-80% of transactions happen privately — no Bloomberg terminal, no centralized data source. Bankers spend huge portions of their week manually researching these deals. Synthesis Intelligence aims to aggregate and structure this data.

---

## Q

**QA (Quality Assurance)**
Testing software to find bugs before users do. The `/qa` skill opens a real browser, clicks through your app, finds problems, fixes them in the code, and verifies the fix worked.
**[YOUR WORK]** Use this to test the Synthesis Intelligence web interface before demos.
→ See: `qa/SKILL.md`, `qa-only/SKILL.md`

---

## R

**Ref (Reference)**
See **@ref** above.

**Regression**
When a bug that was previously fixed comes back. Regression tests verify that fixed bugs stay fixed.

**Retro / /retro**
Retrospective — a skill that looks back at your recent commits, counts what was shipped, who shipped it, test coverage trends, and generates a structured weekly report.
→ See: `retro/SKILL.md`

**Review / /review**
A code review skill that reads your git diff and finds bugs, security holes, and completeness gaps before you ship. Auto-fixes safe issues. Flags risky ones for your approval.
→ See: `review/SKILL.md`

---

## S

**Session**
One Claude Code conversation/window. Each session has its own context. gstack tracks how many sessions are open — if you have 3+ open, it activates "ELI16 mode" (explain everything like I'm 16) because you're juggling contexts.

**Ship / /ship**
The release skill. Syncs main branch, runs tests, bumps version number, updates CHANGELOG, commits, pushes, opens a PR.
→ See: `ship/SKILL.md`

**Skill**
In gstack, a "skill" is a Markdown file (SKILL.md) that tells Claude how to behave in a specific role. When you type `/office-hours`, Claude reads the office-hours skill and becomes a YC partner asking hard questions. Skills are the core innovation of gstack.
→ See: `README.md`, any `{skill-name}/SKILL.md`

**SKILL.md**
The actual skill file Claude reads. Auto-generated from SKILL.md.tmpl — never edit SKILL.md directly, always edit the .tmpl file and regenerate.
→ See: `CLAUDE.md` (SKILL.md workflow section)

**SKILL.md.tmpl**
The template source file for a skill. Human-written with placeholders like `{{COMMAND_REFERENCE}}` that get filled in automatically at build time from the actual source code. Edit this to change skill behavior.
→ See: Any `{skill-name}/SKILL.md.tmpl`

**Snapshot**
The browse tool's way of "reading" a webpage — it generates a structured list of all interactive elements (buttons, links, forms) with @ref labels. Think of it as Claude taking a structured photo of the page.
→ See: `BROWSER.md`, `browse/src/snapshot.ts`

**Sprint**
In software, a sprint is a time-boxed work cycle. gstack's "sprint" is a process: Think → Plan → Build → Review → Test → Ship → Reflect. Each phase has dedicated skills.
→ See: `README.md` (The sprint section)

**STRIDE**
A threat modeling framework: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege. The `/cso` skill runs STRIDE analysis to find security design flaws before attackers do.
**[YOUR WORK]** Synthesis Intelligence handles financial data — STRIDE analysis is essential.
→ See: `cso/SKILL.md`

**Supply Chain Risk**
When a dependency (a library your code uses) is compromised by an attacker. The `/cso` skill checks all your dependencies for known vulnerabilities.

**Symlink**
A file that points to another file or folder, like a shortcut. During development, gstack can create a symlink from `.claude/skills/gstack` back to the source code — so changes to the source immediately affect the installed skill.
→ See: `CLAUDE.md` (Vendored symlink awareness section)

---

## T

**Template / .tmpl**
See **SKILL.md.tmpl** above.

**Tier (Test Tier)**
gstack's tests are split into 3 tiers by cost:
- Tier 1: Free, runs in <2 seconds (static checks)
- Tier 2: ~$3.85/run, takes ~20 minutes (real Claude sessions)
- Tier 3: ~$0.15/run, takes ~30 seconds (LLM judge quality checks)
→ See: `ARCHITECTURE.md` (Test tiers section)

**Touchfile**
A file that, if changed, triggers a specific test to run. Each test declares which files it "touches" (depends on). This is how gstack knows to run the browse tests when browse source code changes, but not when security skill templates change.
→ See: `test/helpers/touchfiles.ts`

**Token**
The unit of text that AI models process. A token is roughly ¾ of a word. Models have limits on how many tokens they can process at once (context window). Costs are billed per token. gstack's browse tool returns plain text instead of JSON to reduce token usage.

---

## V

**VERSION**
A single file in the repo root containing the current version number (e.g., `0.11.9.0`). `/ship` bumps this automatically.
→ See: `VERSION` in repo root

---

## W

**Watchlist Screening**
**[YOUR WORK]** A KYC check that compares a user's identity against global sanctions lists, PEP (Politically Exposed Person) lists, and criminal watchlists. If a match is found, the platform must report it or refuse service.

**Worktree**
A git feature that lets you check out multiple branches of a repo at the same time, each in its own folder. The gstack repo you're reading right now (`wonderful-chatterjee`) is running in a worktree — a separate copy of the repo for this conversation.
→ See: current directory path

---

## X

**XSS (Cross-Site Scripting)**
A security vulnerability where an attacker injects malicious JavaScript into your web pages. It's one of the OWASP Top 10. The `/cso` skill specifically checks for XSS and other injection vulnerabilities.

---

## Abbreviation Quick Reference

| Abbrev | Full Name | Plain English |
|--------|-----------|---------------|
| AI | Artificial Intelligence | Machine that learns patterns from data |
| API | Application Programming Interface | How two programs talk to each other |
| ARIA | Accessible Rich Internet Applications | Accessibility labels on web elements |
| CI/CD | Continuous Integration/Deployment | Automated testing + deployment pipeline |
| CLI | Command-Line Interface | Text-based tool you run in a terminal |
| CSO | Chief Security Officer | Security role (also the gstack skill name) |
| DOM | Document Object Model | The internal structure of a web page |
| E2E | End-to-End | Tests that simulate a full user journey |
| HTTP | HyperText Transfer Protocol | How web browsers talk to servers |
| KYC | Know Your Customer | Identity verification process for financial platforms |
| LLM | Large Language Model | AI like Claude, GPT, Gemini |
| LOC | Lines of Code | How developers measure code volume |
| MCP | Model Context Protocol | Standard for connecting AI to tools |
| MIT | Massachusetts Institute of Technology | Refers to the MIT open-source license (free to use) |
| NDJSON | Newline-Delimited JSON | Data format, one record per line |
| OWASP | Open Web Application Security Project | Security standards organization |
| PEP | Politically Exposed Person | Watchlist category for KYC |
| PII | Personally Identifiable Information | Data that identifies a person (name, ID, etc.) |
| PR | Pull Request | Request to merge code changes |
| QA | Quality Assurance | Testing for bugs |
| SDK | Software Development Kit | Tools for building on a platform |
| SPA | Single-Page Application | Web app built in React/Vue/etc. |
| SQL | Structured Query Language | Language for querying databases |
| STRIDE | (see above) | Security threat modeling framework |
| UI | User Interface | The visual part of an app |
| UUID | Universally Unique Identifier | A random ID (used for security tokens) |
| YC | Y Combinator | Top startup accelerator, Garry Tan's company |

---

*Dictionary maintained in: `/DICTIONARY.md`*
*Last updated: 2026-03-27*
