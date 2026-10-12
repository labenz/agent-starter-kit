# The conversation: gate, "yes to these?", and the pause

The person doesn't type out their life. They answer **two minutes of lines** before anything is read (the gate), say **yes to a list** of sources, read a brief about **their own world**, and then **correct** what the agent inferred. Everything below is wording and method for those moments. Timing in the headings is the person's attention, not wall-clock.

Rules for every turn:
- **At most 3 questions per turn.** For terminal comfort 1–2, at most 2.
- **Every pause question is anchored in the brief.** Quote the brief line, then ask. If the brief already settles a question, don't ask it; confirm it in the summary.
- Accept "skip", "not sure", "later", or "maybe". Record the conservative default and move on.
- Use their words back at them. Capture near-verbatim where the template says so.
- Save `profile.yaml` after every turn.

---

## Gate (~2 minutes, Phase 0): before anything is read

*Why you're asking:* "Three quick things before I read anything, because the next step is me reading your last month of mail. These are the lines I won't cross. Cautious answer is the default; you can loosen any of it later."

**Tap 1: confidential matters**
> Is there anything going on right now that only some people know about: a deal, a hire, a sale, a health matter? I don't need to know what it is. Give me a **codename**, a few **keywords** I'll never put in anything others could see, and **who's in the loop and who isn't**.

If none: "Good, moving on." Never pry. If they name a law firm or banker, record the domain as privileged. Store in `confidential.matters[]`.

**Tap 2: never-touch**
> React to this list; add or remove: bank and brokerage accounts · payroll and HR · medical · your email's own settings · specific people or chats you want kept out entirely (family, an ex, a therapist).

For anything financial, ask for the website ("Chase? Schwab? Gusto?"): those become browser no-go and fetch denies. Store `risk.never_touch`, `risk.never_touch_domains`, `risk.exclude_people`, `risk.exclude_chats`.

**Tap 3: where text may go**
> Searching your history works best when an AI service reads the message text. Pick one:
> **Local only** (default): nothing leaves this Mac. Keyword search, plus meaning-based search run locally. No AI scoring of messages.
> **Gemini OK**: message text goes to Google's Gemini API, on a paid key, for scoring and search. Anything matching your codenames and privileged domains is skipped.
> **Gemini OK, except these people or chats**: same, with exclusions.

One line, once: Google's *free* tier terms allow it to use submitted content to improve its products; the kit uses a paid key if they choose Gemini ([Gemini API terms](https://ai.google.dev/gemini-api/terms); ⚠️ re-read before each cohort). Store `consent.*`.

**State, don't ask** (one line, they can change it at the pause): "Email written as you is always a draft; you press send. I never spend money. Family stuff: I can see family events on your calendar, but family message content stays out of search."

Set `privacy_gate.answered_at`. Then: "That's the gate. Now I'll look at what's on this Mac. Nothing gets opened or logged into."

---

## "Yes to these?" (Phase 1, one turn)

After the silent inventory, one message with a short table (source · account · what I'd read · when), ending:

> I'd start with **these** today: [mail + calendar for the account(s) I'm confident are yours]. The rest can come in during the long build. **Yes to these?** Anything missing, or anything that should stay out?

Take "yes", edits, or "not that one." If they mention an account you didn't find, add it. If a VIP's traffic lives somewhere you can't read yet (texts, WhatsApp, Slack), say so now in one line and mark that source `grind` or `later`.

Then one plain line about the Takeout request (real mode): "To get your full history without any Google developer setup, I'll open Google Takeout. You tick Mail and Calendar and click Export. Google emails you a zip; small mailboxes take minutes, big ones take hours. Meanwhile I'll index your last 30 days another way so you don't wait."

---

## Pause (~10 minutes, Phase 3): after they've read the orientation brief

*Why you're asking:* "You've read how I see your world. I'd rather you correct me than start from scratch. A few questions, each one because of something in the brief."

### Turn 1: correct the profile
1. "I have you as **<name>, <roles>, working on <live projects>**. What's wrong?"
2. Walk the brief's **"I might be wrong about"** list, one line each: "I guessed <X>; right?"
3. (Confirm, don't ask) "Your Mac says <timezone>; I'll use that."

Profile: `person.*` (set `inferred.person: false` once corrected).

### Turn 2: rank what matters
4. "Of the **live threads** and **what's slipping**, which three matter most this month?" (Capture their ranking and words: `goals.priorities_now`.)
5. "I put **<names>** at the top of your VIP list, because <evidence>. Who's wrong, who's missing? And anyone whose email must **never** slip, including whole companies or law firms?" (`person.vips.*`)
6. "Where does the important stuff with **<top VIP>** actually happen: email, texts, WhatsApp, Slack, calls?" If not email: "Then that source goes into the long build; I'll propose it when it's time." (`person.vips.threads_live_in`, and mark the source.)

### Turn 3: confirm the lines in context
7. "Did anything in the brief touch a confidential matter? Use the codename." If yes, confirm it was handled right (private index only, not in the brief), add keywords if they offer them.
8. "Anyone in the brief who should be **out** of search entirely?" (`risk.exclude_people`.)
9. "Email as you: still **draft-only**? Family: still **calendar yes, messages no**?" One line each on what the alternatives mean: **send-with-CC** = routine replies go out with them CC'd, anything novel or sensitive stays a draft; **family in-scope** = family messages searchable; **excluded** = family names never enter search at all, so no family scheduling either.

Profile: `confidential.*`, `risk.email_identity`, `risk.family_rule`, `risk.exclude_*`.

### Turn 4: what should I keep watch on for you?
*Why you're asking:* "Most of what goes wrong is something nobody was watching. I'll watch a short list every day and tell you only when something moves. Here's what I'd watch, from what I saw; pick, change, or add."

Propose 4–8 monitors, **each with its evidence from the seed**, as a numbered list they can answer with "1, 2, 4, and add X". Only propose what the data supports. Candidate kinds:
- **People going quiet**: "Priya usually writes twice a week; nothing in nine days." (VIPs, key clients, a co-founder.)
- **Commitments coming due**: "You told Marco 'by Friday' on the 3rd; nothing since." And **what's owed to you**: "Counsel said 'next week' 12 days ago."
- **Calendar conflicts and prep**: a double-booking in the next 14 days; three lines of context before tomorrow's meeting with someone in the index.
- **Money and admin deadlines**: invoices unpaid, renewals, filings, a lease, a bill, visible in mail. ("Your Workspace renews on the 28th.")
- **Travel**: a flight or hotel confirmation in the next 30 days (check-in windows, a missing night).
- **Family logistics**, inside the family rule: school dates, travel, a recurring pickup.
- **Your field or industry**: the topics and sources the seed shows they read or get asked about. Name two or three.
- **Specific companies or people they track**: competitors, a portfolio company, a prospect, a counterparty (never a confidential-matter counterparty by codename in the clear).
- **AI tools relevant to their work**: only if the seed shows they use or discuss them.
- **The agent system itself**: always on the list. Failed jobs, a stalled grind, an expiring login, a brief that didn't run.

Mention once, not as proposals: "Also available if you ever want them: appointment reminders (dates only, never medical content) and mentions of you or your company online."

For each accepted monitor record: what, why (the evidence), sources (index / calendar / web / system / feeds), sensitivity (set by kind), cadence (daily default), and how to surface (brief section, "needs you today," or a card once the console exists). Also propose the 3–8 **newsletters the seed shows they actually open** as `watch.feeds.newsletters`, and name the 1–3 fields for `watch.world.domains` (that keeps the agent itself current on terms that postdate its training). Write `profile.yaml → watch` (full schema: `~/agent-starter-kit/skills/situational-awareness/watchlist.template.yaml`). One line: "These feed a daily brief at <time>; the first run is a baseline, so it won't dump a month of backlog on you. Nothing else is sent anywhere."

### Turn 5: hand-offs, "working," and path facts
10. "From what I saw, you spend time on **<chores visible in the seed: scheduling back-and-forth, follow-ups, triage, research, …>**. Which would you hand off first, if you trusted the hand-off? Up to three." (`goals.chores_to_hand_off`, near-verbatim.)
11. "Thirty days from now, what moment would make you say 'this is working'?" (`goals.working_in_30_days`, near-verbatim.)
12. Path facts, in one breath: terminal comfort 1–5 ("1 = I'd rather not see it"); which subscription (Claude Max / ChatGPT Pro); GitHub username, if any (needed only while the kit is private). Google account type is usually already inferred from discovery; confirm it in the summary, and if it's a company-managed Workspace, ask who the admin is.

**(opt)** "Have you tried anything like this before? What made you stop?" If terminal pastes or Google keys breaking: "Day 1 here doesn't touch the Google Cloud console, and your full history comes from a zip Google emails you."

Profile: `goals.*`, `setup.*`.

### Contradiction check (mandatory, before generating)

Read `risk`, `confidential`, `consent`, `person.people`, `sources.approved` together. Resolve each conflict with **one** question:
- A family member is excluded from search, but the family rule is logistics-only. Ask: "Can I see Sofia's events on your calendar but not search her messages? Or keep her out entirely?"
- Gemini consent while a confidential matter exists. Ask: "I'll skip anything matching Atlas's keywords and <firm>.com from Gemini. OK?"
- A never-touch item with no domain ("my bank"). Ask for the website.
- A VIP whose traffic lives in a source marked `never`. Ask: "Marco's WhatsApp is on your never list, but you said his threads must never slip. Which wins?"
- A confidential matter whose keywords appeared in the orientation brief (your error). Say so, remove them, note it in `rehearsal-log.md`/`logs/`.

Record each resolution in `risk.contradictions_resolved`. Never save a profile that contradicts itself.

**Pause summary to reflect** (one message, ~10 lines): who they are (corrected), the VIP list and where it lives, the three priorities, what they'd hand off, the 30-day moment, what you'll keep watch on, never-touch, email rule, confidential matters by codename with who's not read in, family rule, where text may go, and the path facts. Then: "That's enough to write your agent's instructions. Give me a minute."

---

## Optional short dives (any time, `/onboard deepen <name>`)

Most of what the old deep dives asked is now read from the data (systems, hardware, rhythms). Two remain worth asking:

### Voice (~3 min): makes drafts sound like them
1. "How do you like things written for you? Short or thorough, formal or casual, anything you hate?"
2. "I can learn your voice from your own sent mail in the index; want me to? Or paste a few paragraphs you were pleased with." Write any pasted sample verbatim to `~/.agent-kit/voice/sample.md`; set `voice.sample_captured`. If they say "use my sent mail," set `voice.learn_from_sent: true` and build `voice/profile.md` from the grind (sent messages only, no quoted text).

### Routines (~2 min)
1. "When should the daily brief be ready, and where: a page that opens in your browser (default), an email to yourself (needs your OK), or the console app once it's on your phone?"
2. "How often should I scan for dropped threads? Default weekdays 08:00 and 16:00."

---

## If they want to stop midway

Save, and tell them exactly how to resume: `/onboard resume` (or `/onboard rehearsal resume`). Never make them repeat a turn. If the grind is running, say so and that it keeps going without them.
