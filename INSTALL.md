# Agent Starter Kit: install requirements and setup journey

**Status:** draft v3, 2026-10-12. v1 was derived from the actual code and a reference two-Mac setup (read-only inspection). v2 applied the fixes from a synthetic-persona QA run. v3 restructures the journey around the "unfolding" arc (SKILL.md): privacy gate → source discovery → 30-day seed → pause → multi-day grind → build-out proposed from evidence. ⚠️ marks claims not verified on a machine.

## The short version

You install your agent (Claude Code or Codex) if you don't have it, then paste **one prompt**. Your agent does the rest, with you in the room for the parts only you can do. Those parts are batched: **one sitting of Mac approvals per install session** (~10 minutes) and a handful of sign-in clicks.

| When | What you get | Your attention | Wall-clock |
|---|---|---|---|
| **Day 1, hour one** | The two-minute privacy gate; a silent inventory of your sources ("yes to these?"); a **seed index of your last 30 days** and an **orientation brief** of your world in your browser. Google Takeout requested for your full history (no Google Cloud console) | **~30 min**, mostly reading the brief | under an hour |
| **Day 1, hour two** | The pause (you correct what it inferred, pick what it should watch); your instruction file, safe permissions, the `kit` CLI, placed into Claude Code / Codex; the **daily situational-awareness brief** scheduled for tomorrow | **~20 min** + one approvals sitting (~5 min) | an hour |
| **Days 1–7, the grind** | Your **full history** imported, (optionally) scored, embedded for meaning-based search, summarized by month and year, and distilled into a **wiki** (people / organizations / projects / timeline). Runs on your subscription, newest-first, resumable | a glance at each morning's brief; keep the laptop awake and plugged in | **3–7 days** of background work (§2b) |
| **Days 2–14, build-out** | Modules proposed one at a time from what it found: comms triage (skills pack), chat sources (Slack / Beeper), the phone Console over Tailscale, more monitors | **5–20 min** per accepted module | as you say yes |
| *Later, optional* | An employee agent on a spare Mac | a weekend | plus a spare Mac |

**Cost, one figure everywhere:** the **$200/month subscription** you probably already have (Claude Max, or ChatGPT Pro; the $100 Max tier works for lighter use, but scheduled jobs share its limits), plus **$0–20/month of optional keys**. The core runs entirely on the subscription through `claude -p` / `codex exec`. Optional keys are only for extras you choose (§1, "Keys at a glance").

---

## Layout: repos, paths, and the one prompt

| Thing | Where | Notes |
|---|---|---|
| **`agent-starter-kit`** (private repo during the pilot) | `~/agent-starter-kit` | The onboard skill, foundation templates, skills pack, kit edition of the Agent Console, and the `kit` CLI. Ships `.claude/skills/onboard` and `.agents/skills/onboard` (relative symlinks to `skills/onboard`), so `/onboard` is discoverable in Claude Code **and** Codex. Read-only to you: `kit update` fast-forwards it. |
| **`deep-context-toolkit`** (public, `github.com/labenz/deep-context-toolkit`) | `~/deep-context-toolkit` | A separate clone, **pinned** to the tag in `~/agent-starter-kit/deps.json`. Code only. Cloned on day 1 for the seed; filled by the grind. Must include `tools/import-takeout.ts` (the Takeout importer, new in October 2026). |
| **Your state** | `~/.agent-kit/` | Everything personal: `profile.yaml`, the generated `AGENTS.md` + `AGENTS.local.md` (your own additions), `kit.config.json`, `generated/`, `seed/`, `grind/` (state + plan), `data/deep-context/` (via `DEEP_CONTEXT_DB` / `DEEP_CONTEXT_VECTOR_DB`), `data/takeout/` (the zip), `wiki/`, `briefs/`, `voice/`, custom `skills/`, `.env` (mode 600), `_trash/`, `logs/`. A local git repo, so every change is reversible. Because none of this lives in either repo, **`kit update` is always a fast-forward pull.** |
| Runtime config | `~/.claude/CLAUDE.md`, `~/.claude/settings.json`, `~/.codex/AGENTS.md`, `~/.codex/config.toml`, `~/.codex/rules/agent-starter-kit.rules` | The kit **never overwrites** these. It backs them up and merges a clearly delimited kit-managed block (§7). |

**Before the one prompt** (skip what you already have):
- Claude Code: open Terminal, paste `curl -fsSL https://claude.ai/install.sh | bash`, type `claude`, and click **Authorize** in the browser. No admin password needed.
- Codex: install the Codex app (`codex app` opens the installer ⚠️ path for first-time users unverified), or `brew install --cask codex` if you have Homebrew. Then sign in with ChatGPT.
- A **GitHub account** that has accepted the kit repo invite. This is only needed while the repo is private; the invite email says so.

**The one prompt** (paste into Claude Code or Codex):

```
Clone https://github.com/<org>/agent-starter-kit to ~/agent-starter-kit, read START-HERE.md, and follow it with me.
```

The agent clones (using `gh auth login --web` first if the repo is private), reads `START-HERE.md`, and runs `/onboard`. Next time: `cd ~/agent-starter-kit`, then `claude` or `codex`, then `/onboard resume`. Or just `kit status` / `kit grind status` from anywhere.

---

## 0. Baseline machine

| Item | Requirement | Evidence / note |
|---|---|---|
| Hardware | **Apple silicon Mac** (M1 or later) | The reference setup is arm64. `voice-print.ts` ships only a `sherpa-onnx-darwin-arm64` native binary. ⚠️ Intel Macs untested; onboarding says so if it detects `x86_64`. |
| macOS | **macOS 26 (Tahoe) tested**; 14+ probably fine | ⚠️ macOS 14/15 untested. |
| Disk | **≥10 GB free** (typical setup uses 2–5 GB), plus room for the Takeout zip (roughly the size of your mailbox) | Roughly 5 GB per ~1M messages; 1–2 years of mail typically lands around 1–2 GB. |
| RAM | 16 GB recommended | Parallel agent sessions are the real load. |
| User account | An admin account, for **one** password entry when Homebrew is first needed (semantic search's SQLite, Ollama) | The gate, seed, and pause need no admin password ⚠️ (except possibly the Xcode Command Line Tools dialog). Keyword-only grind needs none either. |
| One Mac | **Default.** Everything runs on your laptop, including the grind. | If you own an always-on desktop or Mini, the grind can run there instead (it needs only the Takeout zip and the toolkit). A second "employee" machine is optional and later. |
| Windows/Linux | Not supported in v1 | launchd, AppleScript, macOS Contacts, and the Beeper/iMessage paths are macOS-only. |

---

## 1. Requirements by module

Modules in the order they now land: **0 Foundation** (end of the pause), **1 Deep Context** (seed on day 1, grind over days 1–7), then **2 Skills, 3 Console, 4 Employee (optional)**, each proposed from what the seed and grind revealed. Legend: **R** = required for that module, **O** = optional (and what you lose without it).

### Module 0: Foundation (day 1, placed at the end of the pause)

| Software / account | R/O | Version seen | Why |
|---|---|---|---|
| **Claude Code CLI** *or* **Codex CLI** | R (one) | claude 2.1.295; codex-cli 0.159.3 | Runs the onboarding and everything after. Claude Code's native installer goes to `~/.local/bin/claude`. |
| **Claude Max** *or* **ChatGPT Pro** | R (one) | n/a | The subscription spine ($200/mo; see the short version). Headless `claude -p` / `codex exec` under launchd run on this login. ⚠️ ChatGPT Plus also runs Codex, but its limits are too low for scheduled jobs plus interactive use. |
| Xcode Command Line Tools | R | Apple Git 2.39–2.50 | Provides git. `xcode-select --install` pops a macOS dialog: you click **Install** (~5–10 min download). |
| **Bun** ≥ 1.3 | R | 1.3.6 / 1.3.11 | Every kit tool is Bun/TypeScript. `curl -fsSL https://bun.sh/install \| bash`; no sudo. |
| gh (GitHub CLI) | R while the kit repo is private | 2.85 / 2.88 | Day 1 installs the release binary into `~/.local/bin` (no Homebrew, no sudo). `gh auth login --web` shows an 8-character code that you type into a browser page. |
| `kit` CLI | R | ships in the kit | A shim at `~/.local/bin/kit` → `bun ~/agent-starter-kit/tools/kit.ts` (§6). |
| Google Chrome + **Claude in Chrome** | O | Chrome 152 | The agent's hands for web steps (§4): opening Takeout with the right boxes ticked, and the optional Google Cloud console path. Without it, you click through a numbered checklist yourself. |

### Module 1: Deep Context (seed on day 1; grind over days 1–7; `labenz/deep-context-toolkit`)

See the table under "Module 2: Deep Context" below (kept under its old heading so cross-references hold). Day-1 additions:

| Software / account | R/O | Why |
|---|---|---|
| **Google Takeout** (Mail + Calendar, .zip) | R unless you choose OAuth | The no-Google-Cloud-console path to your **full history**: you tick two boxes and click Export; Google emails a zip (minutes for small mailboxes, hours for large ones). `bun ~/deep-context-toolkit/tools/import-takeout.ts <zip> --since 30d` seeds the last 30 days; the same zip with no `--since` is the grind's input. §3 Path 5. |
| **claude.ai connectors: Gmail + Google Calendar** (Claude Code only) | O, recommended | The **fastest seed** while Takeout churns: the agent reads your last 30 days read-only through Anthropic's verified app (one consent in claude.ai → Settings → Connectors, no 7-day expiry). Also live mail for drafts and the daily brief. Verified 2026-10-12: connector servers load in a headless `claude -p` run (§9). |
| `tools/import-takeout.ts` in the pinned toolkit | R for the Takeout path | New in October 2026; `deps.json` must pin a tag that has it. Its `--help` is the contract. ⚠️ Not yet verified on a real multi-GB export. |

### Module 2: Skills pack (proposed from evidence; usually days 2–7)

| Software / account | R/O | Why |
|---|---|---|
| Module 0 | R | Skills are `SKILL.md` + tools, linked into `~/.claude/skills` and `~/.agents/skills`. |
| **`skills/situational-awareness`** | installed with Foundation, not here | The daily brief that reads `profile.yaml → watch`. It is the **first day-2 automation** and is scheduled at the end of the pause (§9), before any other module. |
| Deep Context seed | R | `comms-scan` and the brief read the index; the connectors add today's mail. |
| Search API keys (Exa / Brave / Parallel) | O | The built-in WebSearch works with no key. |
| Gemini / OpenAI image keys | O | Family bundle (worksheets). ⚠️ No subscription-only path for image generation. |

Codex-only users have no connectors, so their seed waits for the Takeout zip (§3 Path 5), which for most mailboxes arrives within the hour. The orientation brief then comes from `import-takeout.ts --since 30d`. OAuth (§3 Path 1/2) is an option on day 1 only at terminal comfort 4–5.

### Module 2: Deep Context, requirements (seed day 1; grind days 1–7)

| Software / account | R/O | Why | Evidence |
|---|---|---|---|
| Bun + `bun install` in `~/deep-context-toolkit/tools` | R | csv-parse, email-reply-parser, html-to-text, js-yaml | public `tools/package.json` |
| Data paths | R | The kit sets `DEEP_CONTEXT_DB` and `DEEP_CONTEXT_VECTOR_DB` to `~/.agent-kit/data/deep-context/`, so data never lives in the repo | env vars present in the public toolkit (2026-10-12) |
| **Google Takeout zip** *or* a **Google Cloud OAuth client** (Desktop type) | R (one) for Gmail/Calendar **history** | Takeout is the default (§3 Path 5): no developer console, no 7-day token death. OAuth (§3 Path 1/2) adds live incremental refresh of history without re-exporting; the agent can add it later. | `tools/import-takeout.ts`, `_lib/google_oauth.ts` |
| **Keyword search, `recent`, todos** | needs **no key** | Works on the subscription alone. | public README "Keys" table |
| Gemini API key | O, **only with consent** | **Enrichment** (quality/topics scoring) uses Gemini today, and so does full-history enrichment. This is the honest exception to "subscription only." Also the default embeddings provider. Google's terms for its free tier allow use of submitted content to improve its products, so use a paid key for sensitive mail ([Gemini API terms](https://ai.google.dev/gemini-api/terms); ⚠️ re-read per cohort). | README |
| **Ollama** (`ollama pull nomic-embed-text`) | O | **Local** embeddings for meaning-based search; nothing leaves the Mac. `DEEP_CONTEXT_EMBED_PROVIDER=ollama`. | README |
| Homebrew `sqlite` | R for semantic search | Apple's SQLite can't load `sqlite-vec`. Keyword search works without it. | deep-context semantic search |
| Voyage API key | O, only with consent | Rerank (free tier is enough for one person ⚠️). Without it, results stay in vector order. | README |
| Slack app per workspace (user token `xoxp-`) | O | Search needs a **user** token (`search:read`). The public README says "Bot Token", which is wrong. Admin-restricted workspaces need an approval you can't speed up. | `slack.ts:844-867` |
| **Beeper Desktop** + API token | O (the path for iMessage/WhatsApp/Signal) | Must be **running** whenever extraction runs. ⚠️ Its iMessage bridge likely asks for Full Disk Access. | `beeper.ts` |
| iMessage direct (chat.db) | O, not recommended | Needs Full Disk Access granted by hand to one binary. **Often fails** (exit 4, `authorization denied`) until the grant is made by hand. Prefer Beeper. | ⚠️ |
| macOS Contacts | O | Phone-to-name resolution; one Contacts prompt. | `contacts.ts` |
| **Fireflies** (Business plan) + API key | O | The **only supported** transcript source. Granola, Otter, and Zoom are **not yet supported**: onboarding records them but doesn't promise them. | README |
| launchd | R for the grind and for freshness | The grind (`kit grind start`) and the scheduled refresh both run `claude -p` / `codex exec` on your subscription with a lock, a deadline, and a resumable state file (`tools/deep-context-cron.sh`; §2b). On the Takeout path, "freshness" after the grind comes from the connectors (today's mail) or a later OAuth add-on; a re-export is the fallback. | README |

### Module 3: Agent Console (proposed from evidence, e.g. travel or "while I'm away")

| Software / account | R/O | Why | Evidence |
|---|---|---|---|
| Bun + `bun install` in `~/agent-starter-kit/console` | R | marked, js-yaml, **web-push**, sherpa-onnx-node (voice prints only) | console `package.json` |
| **Tailscale** (Mac + phone, free personal plan) | **Recommended day-2 add**; R for phone access | Phone push needs HTTPS, which comes from `tailscale serve`. Without Tailscale, the Console works at `http://127.0.0.1:8787` on the Mac only. | `tailscale serve status` |
| Tailnet settings: MagicDNS + HTTPS Certificates on | R for phone | Admin-console toggles; the agent can click them. ⚠️ exact UI wording | |
| iPhone (iOS 16.4+) or Android, Console added to Home Screen | R for phone push | iOS only delivers Web Push to Home-Screen web apps. | ⚠️ |
| VAPID keypair, per-agent bus tokens | R | Generated locally; never printed. | `bus-server.ts`, `bus.ts` |
| iTerm2 | O | Only for "type a Console reply into my live terminal session." Needs Automation → iTerm2. | `session-dashboard.ts` |
| OpenAI API key | O | Live voice and transcription in the Console. Without it, the mic button says "not configured." | `bus-server.ts` |
| Gemini/Voyage keys | O, only with consent | Semantic thread search in the Console; keyword search otherwise. | `bus-server.ts` |
| Telegram bot | O | An extra push path for `kit distress`. Web Push via the Console covers the same ground. | `distress.ts` |
| Password-manager broker (1Password service account) | O, later | Advanced. Everyone starts with `~/.agent-kit/.env` (mode 600). AGENTS.md mentions a broker only once one is installed. | `op.ts` |

### Module 4: Employee agent (optional, later)

| Item | R/O | Why / reference setup |
|---|---|---|
| Always-on Mac (Mac Mini M-series, 16 GB) | R | 16 GB is enough |
| Separate macOS user for the agent | R | The agent never shares your login keychain, browser, or files |
| FileVault off + auto-login + no sleep + auto-restart | R for unattended reboots | **Honest tradeoff:** with FileVault on, a reboot stops at the unlock screen. Turning it off is a human decision about physical security. |
| Remote Login + Screen Sharing; Tailscale | R | |
| **Runner: Claude Code or Codex** on the bus | R | v1 runs the employee as a Claude Code or Codex runner on the bus. **OpenClaw is not in v1.** ⚠️ Check Anthropic/OpenAI terms on one subscription across machines. |
| Agent identity accounts (Google, GitHub, optional capped card) | R for the "employee" role | Google signup usually needs SMS verification (human). A card is a human decision. |

### Keys at a glance (subscription-first)

| Key | Needed for | Without it | Cost |
|---|---|---|---|
| *(none)* | Foundation, Skills, Deep Context keyword search, Console, scheduled jobs | — | $0 beyond the subscription |
| Gemini | Deep Context **enrichment** + default embeddings; Console semantic search | No enrichment; use Ollama for embeddings, or keyword search only | pay-as-you-go; most of the $0–20 |
| Voyage | Rerank | Vector order | free tier ⚠️ |
| OpenAI | Live voice in the Console | No voice | usage |
| Fireflies | Transcripts | No transcripts | Business plan (separate subscription) |

### Accounts at a glance

| Account | Needed for | Human-only part |
|---|---|---|
| Claude Max *or* ChatGPT Pro | Everything | Payment, login click |
| GitHub | Kit repo (pilot) | Accept invite; type the device code |
| claude.ai connectors | Fast 30-day seed; live mail for drafts and the daily brief | One Google consent click |
| Google Takeout | Full mail + calendar history (default path) | Tick Mail + Calendar, click Export, download the zip when Google emails you |
| Google Cloud project (personal Gmail or self-admin Workspace) | Optional: live incremental history refresh | Cloud ToS (first time), consent clicks |
| Company-managed Workspace | — | **Your admin** must allow the OAuth app; until then, connectors only. Onboarding drafts the request for you. |
| Slack app per workspace | Slack source | "Allow"; admin approval if restricted |
| Beeper | iMessage/WhatsApp/Signal | Network logins, QR scans on phone |
| Tailscale | Phone | Login; VPN approval on Mac **and** phone |

---

## 2. The journey

Who: **T** = agent in the terminal, **CU** = agent driving the browser (Claude in Chrome / Codex browser use), **H** = you.

Ordering principle for **every** install session: the agent does everything it can → **one sitting of Mac approvals** → sign-ins one at a time with the agent driving the browser → verify. Payments, passwords, 2FA, and consent decisions are never delegated.

### Session 1: Day 1 (~1 hour of your attention, in two sittings)

| # | Step | Who | Notes | Est. |
|---|---|---|---|---|
| 1 | Paste the one prompt | H | After installing Claude Code or Codex (above) | 1 min |
| 2 | Xcode Command Line Tools, if git is missing: a dialog appears, click **Install** | T triggers, H clicks | ⚠️ May ask for your Mac password on some setups | 1 min attention, ~10 min wall |
| 3 | Install `gh` (release binary → `~/.local/bin`), `gh auth login --web`, clone the kit | T + H (type the 8-char code) | Only while the repo is private | 2 min |
| 4 | **Privacy gate**: codenames for confidential matters, the never-touch list, where text may go | T (conversation) | Nothing is read before this | **2 min** |
| 5 | **Discover sources** (read-only inventory) → "yes to these?" | T, then H (one answer) | Nothing opened or logged into | 3 min |
| 6 | **Request Google Takeout** (Mail + Calendar, export once, .zip): the agent opens the page with the boxes ticked; you click Export | CU + H | No Google Cloud console. The zip is both the seed's and the grind's input. Google emails you when it's ready | 2 min |
| 7 | Install Bun; clone `deep-context-toolkit` at the pinned tag, `bun install`, `deep-context init` under `~/.agent-kit/data/` | T | | 3 min, unattended |
| 8 | **Seed**: last 30 days via the claude.ai connectors (one consent at claude.ai → Settings → Connectors) or, on Codex / without connectors, from the Takeout zip when it lands (`import-takeout.ts --since 30d`) | T (+ H for one consent click) | §3 Path 4 / 5 | 10–20 min, unattended |
| 9 | **Orientation brief** opens in your browser: "here's your world as I see it" | H reads | The first wow; every line links to its source | **10 min** |
| 10 | **The pause**: you correct the inferred profile, rank priorities, confirm VIPs and lines, pick what to watch, say what you'd hand off | T + H (conversation) | At most 3 questions a turn, 4–5 turns | **10 min** |
| 11 | Generate AGENTS.md, permissions, config; AGENTS.md opens in your browser; a 10-line rules card in the terminal. Install the `kit` shim; `kit apply --place` merges into your runtime config (§7) | T | Backs up anything it touches | 3 min |
| 12 | **Approvals sitting (~5 min):** Contacts prompt (name resolution); "Background Items Added" when the daily brief / doctor / grind jobs load (leave them on); allow notifications for kit alerts ⚠️; Keychain "Always Allow" if the first headless run asks | H | The agent opens each thing and verifies it before moving on | 5 min |
| 13 | **Schedule the daily situational-awareness brief**; run it once through launchd in baseline mode (`kit run brief --now --baseline`) and confirm it read the index | T | §9 explains the fallback; the baseline run reports only what it will watch | 2 min |
| 14 | **Start the grind** (`kit grind start`) once the Takeout zip is in: estimate shown (messages, years, days, disk), keep-awake guidance, "start now?" | T + H (one yes) | §2b | 2 min |

### Session 2+: build-out (as proposed; 5–20 minutes each)

Each accepted module is its own short session: preview → install → `kit doctor` → first result. Typical order and what each asks of you:

| Module | Agent does | You do | Est. |
|---|---|---|---|
| **Skills pack** (comms triage, research, drafts) | Links skills, adds the comms-scan schedule, runs the first scan over your real VIPs | "Background Items Added" | 5 min |
| **Chat sources** (Slack, Beeper for iMessage/WhatsApp/Signal) | Slack app manifest per workspace; Beeper install and token; adds them to the grind | Slack "Allow" (admin approval may take days; started, never blocking); Beeper network logins and QR scans from your phone; Beeper's own permissions. **Full Disk Access is not on this list**; Beeper is the chat path | 10–15 min |
| **Semantic search / enrichment** (if the gate allows) | **Homebrew**: the agent opens a new Terminal window with the installer already running (a `.command` file); then `brew install sqlite` (plus `ollama` if local). Optional keys you consented to (Gemini via AI Studio, Voyage) written to `.env` without echoing | Type your Mac password once; accept the key provider's ToS | 10 min |
| **Console + phone** | Bun deps, bus tokens and VAPID keys into `.env`; Tailscale admin (MagicDNS + HTTPS), `tailscale serve`; a test decision card | Tailscale "Allow VPN Configuration" on the Mac; on the phone: install Tailscale, open the Console URL, Add to Home Screen, Allow Notifications; answer the test card | 15 min |
| **Google OAuth** (optional, for live incremental history after the grind) | §3 Path 1/2 driven in your signed-in Chrome; mandatory "In production" step plus a day-5.5 canary | Cloud ToS (first time), consent clicks | ~12 min, ~3 of yours |
| **Employee agent** (optional, later) | Module 4 | A spare Mac and a weekend | |

### 2b. The grind: what to expect

| | |
|---|---|
| **What runs** | G1 import (Takeout zip or OAuth backfill; other approved sources as access lands) → G2 enrich (only if you said Gemini OK) → G3 embed (Ollama locally by default; Gemini if consented; skipped if "none") → G4 monthly summaries → G5 annual roll-ups → G6 wiki articles (`~/.agent-kit/wiki/{people,organizations,projects,positions,expertise,timeline}`) → G7 a second brief, "your world, now with history" |
| **Order** | Newest first, month by month, so the last year is searchable and summarized before older years start. Every month is a checkpoint. |
| **Duration** | Wall-clock depends on history size and your plan's limits. Working ranges to state, not promise: 1–2 years of mail → **1–3 days**; 5+ years or multiple sources → **3–7 days**. The agent prints an estimate (messages, years, days, disk) from the seed and the zip before asking "start now?" ⚠️ These ranges are from early reference runs; calibrate on the first pilot installs. |
| **Tokens** | It runs through `claude -p` / `codex exec` on **your subscription**, no API key. The summaries are the expensive part (a month's export can be ~1M tokens in). Expect your usage meter to sit near the top for the duration; interactive sessions share the same limits, so your agent may feel slow that week. When a limit is hit the job backs off and resumes at the reset; it never fails for that. `kit grind pause` / `resume` any time. The $100 Max tier works; it takes longer. |
| **Keep awake (laptops)** | The job pauses when the Mac sleeps and resumes at wake; nothing is lost, it's just slower. For the fastest run: lid open (or an external display), on power, and System Settings → Battery → Options → "Prevent automatic sleeping on power adapter when the display is off" on. The wrapper holds `caffeinate -i -s` while it runs. If you own an always-on desktop or Mini, say so: the grind can run there (it needs only the zip and the toolkit). |
| **Progress** | `kit grind status` any time; a line in every daily brief ("Grind: 2019–2024 imported, 2024 summarized, wiki has 38 people"); `~/.agent-kit/grind/progress.md`; logs in `~/.agent-kit/logs/grind/`. |
| **Resumable** | `~/.agent-kit/grind/state.json` holds the stage and month cursor; a restart, a sleep, a rate limit, or a crash resumes from there. Nothing re-runs a finished month. |
| **Privacy** | Enrichment and cloud embeddings run only if you said so at the gate; confidential keywords and privileged domains are skipped by the toolkit's skip-list (if the pinned toolkit lacks it, those stages stay off and the agent says why). The wiki is a **private** tree; a derived shared tree with sensitive content stripped is created only if you ever add an employee agent. |
| **Done** | `kit doctor deep-context` green, `briefs/world.md` opened in your browser, `kit apply` so AGENTS.md says the history is searchable. |

---

## 3. The Google OAuth problem

### Why it stalls people

1. **The console is a maze:** project → enable APIs → Google Auth Platform (Branding / Audience / Clients / Data access) → test users → client type. Mistakes surface later as opaque errors (`redirect_uri_mismatch`, `access_denied`, `org_internal`).
2. **Client type.** The code uses a loopback redirect (`http://127.0.0.1:{8080-8089}/oauth`), which needs a **Desktop app** client. (The function is misleadingly named `start_device_auth`.)
3. **"Testing" status = refresh tokens die every 7 days.** Observed in practice: a token granted on day 0 returned `invalid_grant` exactly 7 days later. **A friend who leaves the project in Testing will have their setup silently die on day 8.**
4. **The scary unverified-app screen** ("Google hasn't verified this app"). That's fine for personal use, but people bail there unless they've been warned.
5. **Over-broad scopes.** Request only what the chosen modules need (Deep Context: `gmail.readonly` + `calendar.readonly`).

### Which path, by person

| Person | Seed (day 1, first hour) | Full history (the grind) | Live refresh later (optional) |
|---|---|---|---|
| Claude Code, terminal comfort 1–3 | **Path 4** (connectors, read-only) | **Path 5** (Takeout zip) | Path 1 or 2, agent-driven, ~3 minutes of clicks, proposed only if they want incremental history without re-exporting |
| Comfort 4–5 | Path 4, or Path 1/2 if they want OAuth today | Path 5, or the OAuth backfill | — |
| Company-managed Workspace | Path 4 | **Path 5** (Takeout usually works unless the admin disabled it ⚠️) | Only after their admin approves an OAuth app. Onboarding drafts the request as a doc. |
| Codex-only | **Path 5** with `--since 30d` as soon as the zip lands | Path 5 | Path 1 or 2 |

**Path 1 (personal @gmail.com): the agent drives the Cloud console through Claude in Chrome; the human clicks only the decision points.**

| Step | Who | Detail |
|---|---|---|
| 1 | CU | New project "my-agents" in the person's signed-in Chrome. |
| 2 | **H** | First-time Cloud Terms of Service. The agent stops and asks; it never accepts terms for you. |
| 3 | CU | Enable Gmail API and Google Calendar API (Drive only if a module needs it). |
| 4 | CU | Branding: app name, support and developer email = theirs. |
| 5 | CU | Audience: **External**, add them as a test user, then **Publish app → In production**. ⚠️ Exact banner copy may differ; verification is not required to stop the 7-day expiry. |
| 6 | CU | Clients → Create → **Desktop app** → Download JSON, moved to `~/.agent-kit/google_credentials.json` (mode 600). The secret never passes through the chat. |
| 7 | T + **H** | The loopback listener starts (backgrounded; 5-minute window). The person clicks Advanced → Go to my-agents → scopes → Continue. |
| 8 | T | Doctor G1–G3; record the grant time so `kit doctor` warns at day 5.5. |

**Path 2 (Workspace you administer yourself): "Internal" user type.** No test users, no unverified screen, no expiry.

**Path 3 (`gws auth setup`):** requires gcloud (~500 MB) plus a second consent. ⚠️ Not verified end to end. Not recommended; test during the pilot.

**Path 4 (no Google OAuth; the fast seed):** claude.ai Gmail + Calendar connectors, authorized once in claude.ai settings. The agent reads the last 30 days read-only to build the orientation brief, and uses them afterwards for live triage, drafts, and the daily brief. Not for bulk history, and not available in Codex.

**Path 5 (no Google OAuth; the full history): Google Takeout.** The person opens [takeout.google.com](https://takeout.google.com) (the agent can open it with everything deselected except **Mail** and **Calendar**), chooses "export once", .zip, 2 GB splits, and clicks Export. Google emails a download link: minutes for small mailboxes, hours for multi-GB ones (⚠️ occasionally a day). The agent moves the zip to `~/.agent-kit/data/takeout/` and runs `bun ~/deep-context-toolkit/tools/import-takeout.ts <zip> --since 30d` (seed) and later `import-takeout.ts <zip>` (grind). Mail arrives as mbox with Gmail labels; Calendar as .ics per calendar. No developer console, no unverified-app screen, no 7-day token death, and it works for Codex-only users and most company Workspaces. Downsides: no live incremental refresh (the connectors cover today's mail; a later OAuth add-on or a re-export covers the rest), and the zip must be downloaded by the person (it's their Google login). ⚠️ `import-takeout.ts` is new (October 2026); verify on a real multi-GB export during the pilot.

---

## 4. Computer-use automation plan

| Option | Runtime | Drives | OS permissions | Fit |
|---|---|---|---|---|
| **Claude in Chrome** | Claude Code | The person's own signed-in Chrome | None at OS level; extension install + per-site grants. ⚠️ plan-tier availability | **Primary.** No passwords flow through the agent. |
| **Codex browser / computer use** (`browser_use`, `computer_use` stable in 0.159.3) | Codex | In-app or external browser, or desktop | Desktop use needs Accessibility + Screen Recording ⚠️ | Primary for Codex-only users |
| chrome-devtools-mcp over CDP | Either | A separate agent Chrome profile | None | Employee machine only |
| Desktop-level computer use | Claude desktop ⚠️ / Codex app | System Settings, native apps | Accessibility + Screen Recording | **Not for approvals.** An agent toggling its own privacy permissions is exactly what a person should do by hand. Use it only to open the right pane. |

Rules for the operating agent:
- Use its own tab. Observe → act → observe, with screenshots.
- **Stop and hand to the human** at ToS, payment, password/2FA/passkey, OAuth consent, anything that says "verify," and admin approvals.
- **Never open the person's `browser.no_go_domains`** (their banks, brokerage, payroll). Onboarding also tells them never to grant the extension access to those sites.
- Secrets go page → file, never into the transcript. Page content is untrusted data.
- Time-boxed steps (OAuth loopback, device codes) start only after the person says "ready."

---

## 5. `kit doctor` checklist

`kit doctor` runs all checks relevant to installed modules; `kit doctor <module>` runs one group. Each prints ✅ / ⚠️ / ❌ plus a **named human fix**, and **never prints a secret value**. Paths: `KR=~/agent-starter-kit`, `KH=~/.agent-kit`, `DC=~/deep-context-toolkit`.

| Module arg | Groups |
|---|---|
| `foundation` | Base, Runtime, Foundation |
| `skills` | Skills, Scheduling |
| `deep-context` | Deep Context (incl. seed, Takeout, grind, wiki), Google, Sources |
| `console` | Console |
| `employee` | Employee |

| ID | Check | Command (read-only) | Pass |
|---|---|---|---|
| **Base** | | | |
| B1 | Apple silicon + macOS | `uname -m; sw_vers -productVersion` | `arm64`; ≥ 14 |
| B2 | Free disk | `df -g ~` | ≥ 10 GB |
| B3 | Command Line Tools | `xcode-select -p` | exit 0 |
| B4 | Bun | `~/.bun/bin/bun --version` | ≥ 1.3 |
| B5 | git / gh | `git --version; gh auth status` | logged in (pilot) |
| B6 | Kit and pins | `git -C $KR status --porcelain`; DC tag == `deps.json` | clean; pinned |
| **Runtime** | | | |
| R1 | Claude Code | `~/.local/bin/claude --version` | exit 0 |
| R2 | Subscription works headless | `claude -p "reply ok" --model haiku` | `ok` |
| R3 | Codex (if used) | `codex --version; codex login status` | logged in |
| R4 | Codex bridge (if used) | `grep -q "BEGIN agent-starter-kit" ~/.codex/AGENTS.md && test -f ~/.codex/rules/agent-starter-kit.rules` | present |
| **Foundation** | | | |
| F1 | Instructions placed | `test -s $KH/AGENTS.md && grep -q "BEGIN agent-starter-kit" ~/.claude/CLAUDE.md` | both |
| F2 | Deny list merged | `jq '.permissions.deny\|length' ~/.claude/settings.json` | ≥ kit's count |
| F3 | Secrets file locked | `stat -f %Lp $KH/.env` | `600` |
| F4 | Trash dir | `test -d $KH/_trash` | exists |
| F5 | `.env` read denies present | `jq '[.permissions.deny[]\|select(test("env"))]\|length' ~/.claude/settings.json` | ≥ 20 |
| F6 | Import resolves | `claude -p "From your instructions: the codename of my first confidential matter, or none" --model haiku` | matches `profile.yaml` |
| F7 | AGENTS.md is current | `kit.config.json generated_at` ≥ `profile.yaml` mtime; installed list == `modules.status` | true (else `kit apply`) |
| **Skills** | | | |
| K1 | Skills linked | each kit skill resolves under `~/.claude/skills` and/or `~/.agents/skills` | all |
| K2 | Connectors (connectors path) | `claude -p` probe: Gmail + Calendar connector tools listed **and** a 1-result search succeeds | ✅; ⚠️ = "authorize in claude.ai → Settings → Connectors" |
| **Deep Context** | | | |
| D1 | Deps | `test -d $DC/tools/node_modules` | exists |
| D2 | DB fresh | `DEEP_CONTEXT_DB=$KH/data/deep-context/deep_context.db bun $DC/tools/deep-context.ts status` | each source < 24 h |
| D3 | Homebrew SQLite (semantic only) | `test -f /opt/homebrew/opt/sqlite/lib/libsqlite3.dylib` | exists |
| D4 | Embeddings provider matches consent | `kit.config consent.embeddings` vs env | equal |
| D5 | Gemini key (only if consented) | 1-token test call ⚠️ exact command | 200 |
| D6 | Seed index present | `deep-context.ts status` shows ≥1 source with rows in the last 30 days; `$KH/briefs/orientation.md` exists | both |
| D7 | Takeout zip (Takeout path) | `test -s $KH/data/takeout/*.zip` | present |
| D8 | Grind state sane | `$KH/grind/state.json` parses; status ∈ {running, paused, rate-limited, done}; cursor advanced in the last 24 h unless paused/done | ✅; ⚠️ = stalled (named fix: `kit grind resume`, or keep the Mac awake) |
| D9 | Wiki built | `$KH/wiki/people/` non-empty once `grind.stages.wiki_articles == done` | non-empty |
| **Google** | | | |
| G1 | Client creds present | `test -f $KH/google_credentials.json` | present |
| G2 | Token works | `gmail.ts search "in:inbox" --limit 1 </dev/null` with a 60 s timeout | exit 0, no prompt |
| G3 | Not stuck in Testing | warn if `google_granted_at` > 5.5 days and G2 never succeeded past day 7 | ✅ after day 8 |
| **Sources** | | | |
| S1 | Slack per workspace | `slack.ts -w <name> list channels --limit 1` | exit 0 |
| S2 | Beeper running + token | `curl localhost:23373` + authed probe | 200 |
| S3 | Contacts export | `test -s $KH/data/deep-context/contacts.db` | non-empty |
| **Console** | | | |
| C1 | Bus up | `bun $KR/tools/bus.ts whoami` | identity |
| C2 | Console local | `curl -s -o /dev/null -w '%{http_code}' http://127.0.0.1:8787/app` | 200/302 |
| C3 | Tailscale up (if installed) | `tailscale status --json \| jq -r .BackendState` | `Running` |
| C4 | HTTPS serve | `tailscale serve status` | `:8443 → localhost:8787` |
| C5 | Phone push end to end | test decision card, wait ≤ 5 min | answered |
| **Scheduling** | | | |
| L1 | Kit LaunchAgents loaded | `launchctl print gui/$UID/<label>` | last exit 0 |
| L2 | Background Items not disabled | `launchctl print-disabled gui/$UID` | none disabled |
| L3 | Daily brief produced | `$KH/briefs/latest.html` < 26 h old | fresh |
| L4 | Watch list present | `profile.yaml → watch.monitors` non-empty and includes `system-health` | true |
| **Employee** | | | |
| M1 | Reachable | `ssh -o BatchMode=yes <agent>@<tailscale-ip> true` | exit 0 |
| M2 | Survives reboot | FileVault / autologin / sleep / autorestart | as configured |
| M3 | Runner login | `claude -p "ok"` or `codex exec "ok"` over ssh | ok |

`kit doctor` runs daily (scheduled at the end of the pause, SKILL Phase 3). On any ❌ it puts one plain-language fix at the top of the next brief and calls `kit distress` if the fix is time-sensitive.

---

## 6. The `kit` CLI

Installed in Foundation as `~/.local/bin/kit`, a shim that runs `bun ~/agent-starter-kit/tools/kit.ts "$@"`. It is allow-listed in the kit permissions. Every command is safe to run any time.

| Command | What it does |
|---|---|
| `kit status` | One screen: each module's status, last brief and last doctor run, pending human fixes, the kit version, and the pinned deep-context tag. Read-only. |
| `kit doctor [module…] [--json] [--quiet]` | The §5 checks for installed modules, or the named ones. Exit 0 = all green. Never prints secret values. |
| `kit update` | `git -C ~/agent-starter-kit pull --ff-only` → check out the deep-context tag pinned in `deps.json` → `bun install` where lockfiles changed → `kit apply` → `kit doctor`. Refuses if the kit repo has local edits (it tells you to move them into `~/.agent-kit/skills/`). Prints a changelog, including any model-pin change. |
| `kit apply [--place] [--dry-run]` | Regenerates every file from `~/.agent-kit/profile.yaml` + `modules.status` (SKILL Phase 3, "Generate"), shows a diff, backs up, and re-places the kit blocks (§7). Idempotent. Runs automatically after every module install and every `kit update`. |
| `kit grind start\|status\|pause\|resume` | Starts the multi-day history build as a launchd job (lock, `caffeinate`, state file, per-stage logs, rate-limit back-off), or reports/controls it. `status` prints stage, month cursor, counts, last error, and an updated estimate. Safe to run any time. |
| `kit distress "<msg>" [--kind blocked\|stuck\|error\|question] [--wait <min>]` | Reaches the person through the channels **actually installed**, in order: Web Push (Console + Tailscale), Telegram (if set up), a macOS notification ⚠️ (`osascript display notification`), and always the top of the next brief + `~/.agent-kit/logs/distress.jsonl`. It prints which channels delivered. Self-throttles. Before the Console is on the phone, **it does not reach the phone**, and AGENTS.md says so. |

Helpers: `kit run <routine> [--now]` (run a scheduled routine through its launchd wrapper), `kit routines [list|on|off <name>]`, `kit render <file.md>` (render to HTML and open it in the browser).

---

## 7. Placing generated files (merge, never clobber)

`kit apply --place` (SKILL Phase 3, "Place") does this. Before touching any target, it copies the original to `~/.agent-kit/_trash/pre-kit-<timestamp>/`.

| Target | Absent | Present |
|---|---|---|
| `~/.claude/CLAUDE.md` | Created with only the kit block | Kit block appended, or replaced in place if one already exists. Content outside `<!-- BEGIN agent-starter-kit -->`…`<!-- END agent-starter-kit -->` is never touched. The block imports `@~/.agent-kit/AGENTS.md` and `@~/.agent-kit/AGENTS.local.md`. ⚠️ Home-relative `@~/` imports are believed to work in current Claude Code; doctor F6 proves it on each machine. |
| `~/.claude/settings.json` (user scope) | Created with kit permissions only | `permissions.allow/deny/ask` union-merged and de-duplicated; your entries are kept. Deny wins in Claude Code, so a kit deny overrides a broader allow of yours (`kit apply` lists any such overlaps). |
| `~/.codex/AGENTS.md` | Created with a kit block holding the **full text** of `AGENTS.md` + `AGENTS.local.md` (Codex has no imports) | Kit block appended or replaced |
| `~/.codex/config.toml` | Created from the kit block | Block inserted at the top. Your existing values are kept unless they're less safe (`danger-full-access`, or `approval_policy = "never"` for interactive use). In that case it asks once. |
| `~/.codex/rules/agent-starter-kit.rules` | Written | Replaced (kit-owned file) |
| `~/.claude/skills/<name>`, `~/.agents/skills/<name>` | Symlink to the kit or personal skill | A name collision with a non-kit entry is skipped and reported |

To remove the kit: delete the delimited blocks and the kit-owned rules file and skill symlinks. Your backups are in `_trash`.

---

## 8. Codex configuration surface

Checked against `codex --help` / `codex exec --help` on codex-cli 0.159.3 (2026-10-12):

| Setting | Values | Kit choice |
|---|---|---|
| `sandbox_mode` (`-s`) | `read-only`, `workspace-write`, `danger-full-access` | `workspace-write` |
| `approval_policy` (`-a`, interactive only) | `on-request`, `never` | `on-request` interactive. Scheduled `codex exec` runs pass `-c approval_policy="never"`, so a blocked command fails back to the model instead of hanging. |
| `[sandbox_workspace_write]` | `network_access`, `writable_roots` | `network_access = true` (kit tools call APIs), `writable_roots = ["~/.agent-kit"]` |
| `~/.codex/rules/*.rules` | `prefix_rule(pattern, decision = allow\|prompt\|forbidden)` | `agent-starter-kit.rules`: forbid recursive `rm`, `sudo`, force-push, `launchctl unload/remove/bootout`, keychain reads, `printenv`/`env`; prompt for `brew install`, `launchctl load/bootstrap`, `git reset/clean` |
| Never used | `--dangerously-bypass-approvals-and-sandbox` | — |

**Honest gaps on Codex:** prefix rules can't match a filename anywhere in a command, so the Claude-side `.env` read denies have no exact equivalent. Draft-only email is enforced by AGENTS.md, not by a rule. The claude.ai connectors aren't available. Codex users get the same instructions, with fewer mechanical backstops.

---

## 9. Scheduling: model pins, brief channels, and the headless-connector question

- **Wrapper:** each routine is a launchd job running a small shell wrapper (lock, deadline, per-run log in `~/.agent-kit/logs/`, and a ledger line `kit status` reads).
- **Model pins** (in `kit.config.json`; `kit update` announces any change): Claude `claude -p --model claude-opus-5-5`; Codex `codex exec -m gpt-6-astra -s workspace-write -c approval_policy="never"`. Both run on the subscription, with no API key.
- **The daily brief is the situational-awareness skill** (`~/agent-starter-kit/skills/situational-awareness/`), scheduled at the end of the pause as the **first day-2 automation**. It reads `profile.yaml → watch` (the monitors the person picked), the index, the calendar, the live connectors if present, the web for field/entity monitors, and the system's own health (`kit doctor`, grind state), and reports *movement* only. Its **first run is baseline-only** (`kit run brief --now --baseline`: watermarks + per-person baselines, no backlog reported as new), and its **Friday edition appends a ≤8-line watchlist review** (what fired, what was acted on, proposed drops/adds). The skill's `SessionStart` injector (`tools/sa-inject.ts`) is merged into `~/.claude/settings.json` hooks (and `~/.codex/hooks.json`) by `kit apply --place` without touching the person's own hooks. Later modules (comms scan, doctor) feed it rather than replacing it.
- **Brief channels:** `browser-page` (default: `~/.agent-kit/briefs/latest.html`, opened when the run finishes and at login), `email-to-self` (only with explicit consent; it's a *draft* to yourself if you chose draft-only), `console` (once installed), or `terminal-file`.
- **The grind wrapper** (`kit grind`) is the same shape: a launchd job with a lock, a state file, `caffeinate -i -s` for the duration, per-stage logs, and back-off on rate limits (§2b).
- **Laptop asleep:** launchd runs a missed calendar job when the Mac next wakes. If the lid is closed at 7:00, the brief appears when you open it. Onboarding says this plainly.
- **Headless connectors (QA fix 3), what we know:** a headless `claude -p` test run listed the claude.ai connector servers (Gmail, Google Calendar, Drive) and loaded the authorized ones' tools. So the connectors *do* load headless. ⚠️ Still unverified: an authorized Gmail read inside `claude -p` **under launchd** (a different environment, with keychain access). Because of that, the end of the pause (SKILL Phase 3) requires `kit run brief --now` through the real wrapper and checks that mail was read. If it wasn't, the brief falls back to the seed index plus calendar (or calendar only), and the person is told which they'll get.

---

## Findings worth acting on

1. ⚠️ **Direct iMessage reads usually fail** (exit 4, `authorization denied`) until a human grants Full Disk Access to the reader binary (System Settings → Full Disk Access → `+`). This is also why the kit prefers Beeper.
2. **Testing-mode 7-day token death** is the #1 thing to design out (§3): a mandatory publish step plus a canary.
3. **Public deep-context README:** the Slack section says "Bot Token" but search needs a user token, and it calls the loopback flow "device auth."
4. Rename `start_device_auth` → `start_loopback_auth`.
5. Make Google scopes module-driven, not `ALL_GOOGLE_SCOPES`.
6. **Deep Context needs a third-party skip-list** (confidential keywords/domains excluded from enrichment and embeddings) and a "calendar yes, messages no" family mode. Until those exist, `kit apply` refuses enrichment and Gemini embeddings when confidential matters are recorded.
7. Verify an authorized claude.ai Gmail connector read under launchd at the first pilot install (§9).
8. The only API key between Deep Context and "subscription only" is **Gemini, for enrichment**. Keyword search needs nothing, and embeddings can be local (Ollama).
9. **Google Takeout is the day-1 history path** (Path 5): it removes the Cloud console, the unverified-app screen, and the 7-day token death from the critical path, and it works for Codex-only users. `tools/import-takeout.ts` (mbox + .ics, `--since`) is the piece being built; the pilot must verify it on a multi-GB export.
10. **The grind is a product surface, not a background detail**: estimate before starting, newest-first, resumable state, progress in the brief, keep-awake guidance for laptops, and an offer to run it on an always-on Mac if they have one (§2b).

## Sources (local evidence)

- [deep-context-toolkit](https://github.com/labenz/deep-context-toolkit) `README.md` (keys table, Ollama option, `DEEP_CONTEXT_*` env vars), `tools/_lib/google_oauth.ts`, `tools/slack.ts`, `tools/beeper.ts`, `tools/contacts.ts`
- Agent Console source (`package.json`, `bus-server.ts`, `bus.ts`, `distress.ts`)
- `codex --help`, `codex exec --help`, `codex execpolicy --help` (codex-cli 0.159.3), Codex `.rules` syntax, 2026-10
- Headless connector probe: `claude -p … --setting-sources local`, 2026-10
- [Gemini API Additional Terms](https://ai.google.dev/gemini-api/terms)
