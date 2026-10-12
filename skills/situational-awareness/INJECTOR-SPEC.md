# Session-start injection: spec

The write path (the daily run) is only half the system. The read path is the half that prevents the failure. A doc the agent has to remember to open is a doc it won't open, because the whole problem is that it doesn't know it doesn't know. So the glossary head has to be **in context before any work starts**, in every session, in both runtimes.

## The script: `tools/sa-inject.ts`

**Input:** `~/.agent-kit/context/SITUATIONAL_AWARENESS.md` (path overridable via `watch.world.doc`), plus `~/.agent-kit/profile.yaml → watch.world.injection` and `~/.agent-kit/briefs/latest.md`.

**Parsing:**
- Glossary = from the `## Glossary` heading to `<!-- SA:GLOSSARY-END -->`. Entries are top-level `- **Name**` bullets; continuation lines belong to the preceding entry.
- `updated:` comes from the frontmatter.

**Output, in order:**
1. Header (≤6 lines): "Situational awareness: named things in <owner>'s fields (<domains>) that postdate your training cutoff. Updated <date>." Then the rule: if a term is here, treat it as known and don't over-explain it; if it isn't here and could postdate your training, say you're unsure and search. Never assert absence from ignorance.
2. **Newest `max_full_entries` (default 12), compressed** to the recognition core: `- **Name** *(a.k.a. …)* — DATE — first sentence` (cut at the first sentence boundary or ~260 chars; drop the link tail).
3. **Recognition tail:** every older entry *by name only*, newest first, capped at `max_tail_names` (default 200), with "+N more: use sa-lookup." Truncating newest-first silently expires the terms that have been in circulation longest. Those are the *most* embedded, not the least. A bare name is enough to stop the confident-denial failure.
4. **Brief pointer** (one line, only if `briefs/latest.md` is from today): "Today's SA brief: ~/.agent-kit/briefs/latest.md (<N> items, <M> need you today)." Counts only, **never item content**. High-sensitivity items must not ride along into every session's context.
5. Lookup line: `Detail on one term: bun <KIT_ROOT>/skills/situational-awareness/tools/sa-lookup.ts <term>. Exit 1 = not in the glossary, so search.`

**Budget:** ≤ `token_budget` (default 1,500 tokens; estimate chars/4). If over, drop full entries first (down to 6), then shrink the tail cap. Never drop the header rule or the oldest-term canary from the tail.

**Failure behavior:** silent no-op (exit 0, no output) if the doc is missing, empty, or malformed, or if `SA_INJECT_OFF=1`. **A hook must never break session startup.**

**Env tuning:** `SA_INJECT_MAX`, `SA_INJECT_TAIL`, `SA_INJECT_FULL=1` (uncompressed entries, for debugging), `SA_INJECT_OFF=1`.

**Emit format** (both runtimes accept it):
```json
{"hookSpecificOutput":{"hookEventName":"SessionStart","additionalContext":"<text>"}}
```

## Claude Code

`kit apply --place` union-merges into `~/.claude/settings.json` (never replacing the person's existing hooks):

```json
{
  "hooks": {
    "SessionStart": [
      { "hooks": [ { "type": "command",
                     "command": "~/.bun/bin/bun ~/agent-starter-kit/skills/situational-awareness/tools/sa-inject.ts" } ] }
    ]
  }
}
```

The onboarding `settings.kit.json.tmpl` is permissions-only today. It needs this hooks block and a merge rule: append the kit entry if no entry with the same command exists, and never remove theirs.

Verify: `claude -p "List the three newest terms in your situational-awareness glossary, or say none." --model <cheap pin>` answers from context without reading files.

## Codex

**Primary: the SessionStart hook.** codex-cli 0.159.3 reports `hooks` as a **stable, enabled** feature (`codex features list`). The binary carries a `hooks.json` config with events `PreToolUse, PermissionRequest, PostToolUse, PreCompact, PostCompact, SessionStart, SessionEnd, UserPromptSubmit, SubagentStart, SubagentStop, Stop`. Its `SessionStart` output schema has `hookSpecificOutput.additionalContext`, the same shape Claude Code uses. Hooks are trust-gated (a stored `trusted_hash`), so the person approves the hook once.

- Place: `~/.codex/hooks.json` with a `SessionStart` entry running the same command (mirror Claude Code's `{matcher?, hooks:[{type:"command", command}]}` structure; confirm the exact file schema with `codex --help`/docs at build time).
- ⚠️ **Unverified end to end** (this spec didn't modify a live `~/.codex`). Verify at the first pilot install: place the hook, trust it, then `codex exec "List the three newest terms in your situational-awareness glossary, or say none."`. Record the result in `kit doctor`.

**Fallback: static include** (`watch.world.injection.codex: static`), used if the hook doesn't fire, can't be trusted, or the Codex version predates hooks:
- Codex reads `~/.codex/AGENTS.md` (global) and has no `@import`. Onboarding already places the full kit AGENTS.md text inside a `<!-- BEGIN agent-starter-kit -->` block there.
- Each daily run re-renders the injector's output (minus the brief pointer, which would be stale) between `<!-- BEGIN kit-sa -->` / `<!-- END kit-sa -->` **inside** the kit block. It touches nothing outside those markers and backs up first to `~/.agent-kit/_trash/`.
- Size: Codex truncates project docs at `project_doc_max_bytes` (default 32 KiB). Cap the SA block at **6 KB** (≈ 8 full entries + ≤120 tail names), and have `kit doctor` warn if the kit block plus the SA block exceeds 28 KiB.
- The fallback is at most one day stale. That's acceptable, since the glossary changes daily at most.

## Health checks (`sa-health.ts`)

- Run the injector, measure its output tokens, and warn if > budget.
- **Canary:** the *oldest* glossary term must appear in the injected text. If it doesn't, the tail is broken. This is a CRIT.
- Claude Code: the hook entry exists in `~/.claude/settings.json`. Codex: the hook exists and is trusted, or the static block exists and its `updated:` is ≤ 26h old.
