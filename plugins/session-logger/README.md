# session-logger

A Claude Code plugin that automatically uploads session transcripts to a remote endpoint.

## What it does

- **Automatic upload**: Every time Claude finishes responding, the full session transcript (JSONL) is POSTed to your configured endpoint.
- **Manual export**: Use `/session-logger:export-session` to trigger an upload on demand.
- **Configurable**: Set your own endpoint and auth token on install — no file editing needed.

## Install

### From a local directory (development)

```bash
claude --plugin-dir ./session-logger
```

### Permanent install

```bash
claude plugin install session-logger
```

You'll be prompted to configure:

- **api_endpoint** — where to POST transcripts (default: `https://httpbin.org/post`)
- **api_token** — optional Bearer token for authentication

## Usage

Once installed, the plugin works automatically. Every time Claude stops responding, the current session transcript is uploaded in the background.

To manually export:

```
/session-logger:export-session
```

## Configuration

The endpoint and token are set via `userConfig` when the plugin is enabled. To change them later, disable and re-enable the plugin, or edit your Claude Code settings directly:

```json
{
  "pluginConfigs": {
    "session-logger": {
      "options": {
        "api_endpoint": "https://your-endpoint.example.com/logs"
      }
    }
  }
}
```

## Plugin structure

```
session-logger/
├── .claude-plugin/
│   └── plugin.json         # Manifest + userConfig
├── hooks/
│   └── hooks.json          # Stop hook for auto-upload
├── bin/
│   └── upload-transcript   # Upload script (curl-based)
├── skills/
│   └── export-session/
│       └── SKILL.md        # Manual export skill
└── README.md
```

## Requirements

- `jq` (for parsing hook input JSON)
- `curl` (for uploading)

## License

MIT
