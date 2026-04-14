---
description: Upload past Claude Code session transcripts from this machine to Trace
disable-model-invocation: true
---

Upload historical session transcripts from `~/.claude/projects/` to Trace. This runs the same PII redaction as live captures and skips any sessions that were already captured live (won't overwrite them).

First, get a count of what's on disk so the user knows what they're agreeing to:

```bash
if [ -d "${HOME}/.claude/projects" ]; then
  TRANSCRIPT_COUNT=$(find "${HOME}/.claude/projects" -type f -name "*.jsonl" 2>/dev/null | wc -l | tr -d ' ')
  TRANSCRIPT_SIZE=$(find "${HOME}/.claude/projects" -type f -name "*.jsonl" -exec cat {} + 2>/dev/null | wc -c | tr -d ' ')
  echo "Found $TRANSCRIPT_COUNT transcripts ($(echo "scale=1; $TRANSCRIPT_SIZE / 1048576" | bc) MB total)"
else
  echo "No Claude Code transcripts directory found"
fi
```

Tell the user how many transcripts were found and ask them to confirm the upload.

If they confirm, start the backfill in the background:

```bash
nohup "${CLAUDE_PLUGIN_ROOT}/bin/backfill-transcripts" > /dev/null 2>&1 &
echo "Backfill started (PID $!)"
```

Tell the user:
- It runs in the background and takes about 1 second per session (paced to avoid rate limits)
- They can watch progress: `tail -f ~/.trace/backfill.log`
- Uploaded sessions will appear in their dashboard gradually at https://trace-web-app.fly.dev/dashboard
- Already-uploaded sessions (from live capture) are skipped automatically — no duplicates
- It's safe to run again — the script tracks which files it's already processed

If the user says no, just tell them they can run `/session-logger:import-past` any time to upload past sessions.
