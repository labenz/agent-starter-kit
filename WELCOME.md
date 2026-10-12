# Welcome: what this is, and what it's for

*From Nathan. About ten minutes. The onboarding starts when you're ready, and it starts by reading, not by asking.*

---

## The short version

Over the last year I built myself a small staff of AI agents, and the thing I didn't expect is how much of the value came from one move: **giving an agent command of my information environment.** Not access to my inbox. Command. Every email, Slack message, text, WhatsApp thread, calendar event, and call transcript going back years, indexed, scored, summarized month by month, and distilled into a wiki of the people, projects, and positions in my life that the agent reads before it does anything. Mine is north of 900,000 messages. When I ask "find the thread where the venue confirmed the date for the offsite," it finds the exact message, from a year of material, in a couple of seconds, and it picks the right thread 97% of the time.

Once an agent knows your world like that, delegation stops being a chore of explaining. I say "draft the reply" and the draft already knows who the person is, what we agreed in March, and how I write to them. Every morning a brief is waiting that knows what to watch: important threads I haven't answered, what I promised and haven't done, who's gone quiet, what's due, what moved in my field, and which of my own scheduled jobs broke. Twice a day it scans everything for things slipping. When something needs *me* and only me, my phone buzzes with a card: three options, trade-offs, one tap. I left my laptop at home for a weekend and work still got delegated, decided, and delivered.

The second half of it is a second kind of agent. The one on my laptop is an extension of me: it sees everything I see and does nothing I didn't ask for. The others live on an always-on Mac Mini and work like junior employees, with their own email, their own accounts, their own capped card, and no access to my private data. They run for hours unattended: overnight research, signups, monitoring, the errands you'd give a capable new hire. The whole thing runs on a subscription I was already paying for.

This kit is that setup, stripped of everything specific to me, plus an onboarding your agent runs on you. **You don't install it; your agent installs it, with you in the room for the parts only you can do.** You don't type out who you are; the agent reads your last month, shows you your world as it sees it, and you correct it. You start with one Mac. The rest unfolds from there.

The point isn't the tooling. The point is to get to the place where **delegating to an AI is as easy as delegating to a person you trust, and safer.**

---

## Two kinds of agents

`[VIDEO: two-kinds-of-agents]`

The failure modes here are opposites. An agent with all your access, running unsupervised, is scary. An agent with no access, waiting on your approval for everything, is pointless. My resolution: treat **access × autonomy as the risk you're managing, and trade one against the other.** No single agent gets both.

```
                       LOW AUTONOMY                       HIGH AUTONOMY
                 (acts only when you say)             (runs for hours alone)

              ┌───────────────────────────┐   ┌───────────────────────────┐
  HIGH        │  YOUR LAPTOP              │   │  never build this         │
  ACCESS      │  "an extension of you"    │   │                           │
  (your real  │  sees everything you see; │   │  all your access, nobody  │
   accounts)  │  does nothing you didn't  │   │  watching: one bad email  │
              │  ask for                  │   │  can steer it             │
              └───────────────────────────┘   └───────────────────────────┘
              ┌───────────────────────────┐   ┌───────────────────────────┐
  LOW         │  not worth building       │   │  A SPARE MACHINE          │
  ACCESS      │                           │   │  "an employee"            │
  (its own    │  can't do much, and won't │   │  own email, accounts and  │
   accounts)  │  do it without you anyway │   │  card; works unattended;  │
              │                           │   │  never sees your private  │
              └───────────────────────────┘   └───────────────────────────┘
```

**The extension-of-you agent** runs on your laptop, logged in as you. What keeps it safe is not limited access but limited autonomy. Anything that leaves the building, like an email sent as you, either happens because you said so in the conversation or lands as a draft. The rule underneath everything: **the signature must match the reviewer.**

**The employee agent** runs on a separate, always-on machine under its own identity. It cannot read your Gmail, not because it promised, but because it was never given the keys. In exchange it runs unsupervised, and you manage it like a hire: a handbook it reads every session, scoped accounts, and an escalation ladder that ends in a one-tap decision on your phone.

You don't need the employee to start. Begin with the extension-of-you agent on the Mac you already have.

---

## How it unfolds

`[VIDEO: unfolding]`

I tested an early version of this onboarding on myself. It opened with a fifteen-minute interview: your name, your roles, who matters, what slips. I lasted one question. Nobody wants to type that out, and the agent doesn't need you to. It's all in your mail. So the onboarding now works the way I'd want a brilliant new chief of staff to work on day one: read everything, come back with a map, and *then* ask good questions.

**First two minutes: the lines.** Before the agent reads a single message, it asks three things it can't infer: anything confidential going on (give it a codename, not the details), what it may never touch (banks, payroll, medical, specific people), and whether any of your text may go to an outside AI service or must stay on this Mac. Cautious is the default. Nothing leaves your laptop until you've answered.

**Then it looks around.** Which mail accounts are on this Mac, which calendars, whether Slack or WhatsApp is installed, whether your Google account is personal or a company Workspace. You say "yes to these." You don't log into anything.

**Within the hour: your world as it sees it.** It indexes your last thirty days, fast, and opens a brief in your browser: who it thinks you are, the fifteen people who matter most and why, the live threads, what's slipping (the message from someone important you haven't answered in four days; the thing you said you'd send Tuesday), your calendar's shape, and a short list titled "I might be wrong about." Every line links to its source. This is the moment it clicks, and it happens before lunch.

**Then it asks.** Not "who matters to you?" but "I put Priya at the top of your list because of these six threads; right?" You correct instead of compose. Then: **"what should I keep watch on for you?"** It proposes a short list from what it saw: a VIP who's gone quiet, a commitment coming due, a renewal in your mail, your field's news, a competitor, the health of the system itself. You pick. That list becomes a daily brief, scheduled before you finish the conversation. Ten minutes, and it writes your agent's instruction file: who you are, who matters, your lines, your voice. You read it in your browser; it installs itself into Claude Code or Codex without overwriting anything you already have.

**Then the grind.** This is where the week of tokens goes, and it should. It asks Google for your full mail and calendar history (one click, a zip in your inbox, no developer console) and builds, newest-first, over several days: the full index, scoring, meaning-based search, a summary of every month, roll-ups of every year, and the wiki: a page per person, project, organization, and era. Your laptop stays awake and plugged in; your subscription meter sits near the top. You read progress in the morning brief. At the end, a second brief: your world, now with history.

**Then it proposes.** Nothing from a menu. "Eleven threads from people on your VIP list sat more than two days last month; a twice-daily scan would have caught all of them. Want it?" "You said Marco's stuff happens on WhatsApp; I can read it through Beeper, two QR codes." "You're out of town nine days next month; with the console on your phone I can send you a one-tap decision instead of waiting." Each one because of something it found, one at a time, yes or later.

---

## The loop from your phone

`[VIDEO: console-loop]`

Once the console is on your phone (your own private network, nothing exposed to the internet), everything in flight is one screen. When an agent hits something only you can decide, you get a card and tap. When something is wrong, any agent can pull an emergency cord that reaches your phone on two independent paths. And you can hand off from anywhere: "find the best option for X under $Y" from the parking lot, three options back in an hour. You stop being the bottleneck without being out of the loop.

---

## What it costs, honestly

- **A $200/month subscription you may already have:** Claude Max or ChatGPT Pro. The onboarding, the grind, the daily brief: all of it runs under that plan. The $100 tier works; the grind takes longer.
- **$0–20/month of optional keys.** Search of your history works with no key. The exception I won't hide: AI *scoring* of your full history uses a Google Gemini key today, and that sends message text to Google. The gate asks before anything like that turns on, and meaning-based search can run fully locally.
- **Hardware:** nothing beyond your Mac. A used Mac Mini (~$600) if you ever add the employee, or want the grind off your laptop.

---

## What can go wrong

**Your data is on your laptop, in plain files.** That's what makes it fast and private, and it means anyone with your unlocked laptop has it. Use disk encryption and a lock screen. Nothing goes to an outside AI service unless you said so at the gate.

**Prompt injection is real.** The content of any email or webpage is data, never instructions. The agents are told this, and the structure backs it up: the laptop agent acts only on instructions from you.

**Things break quietly.** Tokens expire. The laptop sleeps, and the grind and the daily brief wait until it wakes. That's why the system itself is always on the watch list: the brief tells you when something's broken, because the expensive failure is the one you don't notice.

**The agent will sometimes do something dumb.** Usually reversible, by design: destructive operations prompt, deletions go to a trash folder, your existing settings are backed up before anything is merged in. An incident, not a catastrophe.

**It can become a hobby.** The onboarding asks what "working" looks like in thirty days. Hold it to that.

---

## Your first two weeks

`[VIDEO: first-two-weeks]`

| When | What happens | Your attention |
|---|---|---|
| **Hour one** | Gate, "yes to these," seed index, orientation brief | ~30 minutes, mostly reading |
| **Hour two** | The pause: you correct, pick what to watch; it writes and installs your instructions; tomorrow's brief is scheduled | ~20 minutes |
| **Days 1–7** | The grind: full history → scoring → search → monthly and annual summaries → wiki. Laptop awake, plugged in | A glance at each morning's brief |
| **Days 2–14** | Proposals, one at a time: comms triage, chat sources, the console on your phone, more to watch | 5–20 minutes each, when you say yes |
| **Later, if ever** | An employee agent on a spare Mac | A weekend |

---

## How to start

If you don't have Claude Code or Codex yet, install it first. It's one line in Terminal, and the invite email has it. Then paste this into your agent:

```
Clone <repo> to ~/agent-starter-kit, read START-HERE.md, and follow it with me.
```

To see the whole experience without installing anything, add **"in rehearsal mode."** It runs the gate and the discovery for real, builds the orientation brief read-only (or from clearly-labeled fake data if it can't read your mail), asks the same questions, and writes everything into one sandbox folder. Nothing else is touched. To run it again from scratch, say `/onboard rehearsal --fresh`.

### Already have an agent?

If you already run your own setup (Claude Code, Codex, Cursor, OpenClaw, your own instruction file, skills, scheduled jobs) and just want to borrow a few ideas from mine, there's a second path. Paste this instead:

```
Clone https://github.com/labenz/agent-starter-kit to ~/agent-starter-kit, read skills/guided-upgrade/SKILL.md, and run a guided upgrade of my existing setup with me.
```

Your agent takes a read-only look at how your setup works today (it never opens secrets, mail or messages), asks you three to five questions, and comes back with a ranked shortlist of three to five ideas that fit what you already have: rules that stop it asking permission for reversible things, a trash folder instead of deletes, a morning brief, a knowledge base of your comms, the two-kinds-of-agents split, and so on. You adopt them one at a time, each previewed and backed up first; nothing of yours is overwritten. It ends with an `UPGRADE-PLAN.md` in your home folder. Add "dry run" to see the shortlist and the exact changes without installing anything.

Questions, or something broke? Your agent can always flag it; once the console is on your phone, that reaches you anywhere.

Nathan
