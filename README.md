# Trace Marketplace

A Claude Code plugin marketplace for session logging and observability.

## Plugins

| Plugin | Description |
|--------|-------------|
| **session-logger** | Automatically uploads Claude Code session transcripts to Supabase |

## Install

Add this marketplace to Claude Code:

```
/plugin marketplace add https://github.com/justinfleet/trace-marketplace
```

Then install the plugin:

```
/plugin install session-logger@trace-marketplace
```

You'll be prompted to configure:

- **API Endpoint** — your Supabase project URL
- **API Token** — your Supabase anon/publishable key

## Requirements

- Claude Code v2.1+
- `jq` and `curl` installed
- A Supabase project with a `session_logs` table (see [setup instructions](./plugins/session-logger/README.md))
