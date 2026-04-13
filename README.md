# Trace Marketplace

A Claude Code plugin marketplace for session logging and observability.

## Plugins

| Plugin | Description |
|--------|-------------|
| **session-logger** | Automatically uploads Claude Code session transcripts to Supabase |

## Install

Add this marketplace to Claude Code:

```
/plugin marketplace add https://github.com/sumnerjj/trace-marketplace
```

Then install the plugin:

```
/plugin install session-logger@trace-marketplace
```

## Requirements

- Claude Code v2.1+
- `jq` and `curl` installed
