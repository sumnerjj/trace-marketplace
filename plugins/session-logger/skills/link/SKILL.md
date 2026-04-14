---
description: Link this plugin to your Trace account using a code from the web app
disable-model-invocation: true
---

Link this plugin to a Trace account. The user should provide a link code as an argument.

If no code was provided in "$ARGUMENTS", ask the user for their link code. Tell them they can get one at https://trace-web-app.fly.dev/link

Once you have the code, run this command to link:

```bash
# Read user_id from the canonical location, with fallbacks for older installs
if [ -f "${HOME}/.trace/user-id" ]; then
  PLUGIN_USER_ID=$(cat "${HOME}/.trace/user-id")
elif [ -f "${CLAUDE_PLUGIN_DATA:-/dev/null}/session-logger-user-id" ]; then
  PLUGIN_USER_ID=$(cat "${CLAUDE_PLUGIN_DATA}/session-logger-user-id")
elif [ -f "${HOME}/.claude/session-logger-user-id" ]; then
  PLUGIN_USER_ID=$(cat "${HOME}/.claude/session-logger-user-id")
else
  PLUGIN_USER_ID=""
fi

if [ -z "$PLUGIN_USER_ID" ]; then
  echo "ERROR: No plugin user ID found. Run a session first to generate one, then try linking again."
  exit 1
fi

curl -s -X POST https://trace-web-app.fly.dev/api/link/claim \
  -H "Content-Type: application/json" \
  -d "{\"code\": \"$ARGUMENTS\", \"plugin_user_id\": \"$PLUGIN_USER_ID\"}"
```

If the response contains "Linked successfully" or "Already linked", tell the user they're all set and their sessions will now appear in their Trace dashboard at https://trace-web-app.fly.dev/dashboard

If the response contains an error, show the error message and suggest they generate a new code at https://trace-web-app.fly.dev/link
