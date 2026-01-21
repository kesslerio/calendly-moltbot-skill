# Calendly Clawdbot Skill

Clawdbot skill for Calendly integration. List events, check availability, manage meetings via the Calendly API.

## Features

- **User Info**: Get authenticated user details
- **Event Management**: List, view, and cancel scheduled events
- **Invitee Management**: View event invitees
- **Organization**: List organization memberships

## Installation

```bash
# Clone the repo
git clone https://github.com/kesslerio/calendly-clawdbot-skill.git
cd calendly-clawdbot-skill

# The CLI is self-contained (generated via mcporter from MCP server)
chmod +x calendly
```

## Configuration

Add your Calendly Personal Access Token to your environment:

```bash
export CALENDLY_API_KEY="your-pat-token"
```

Get your token from: https://calendly.com/integrations/api_webhooks

## Usage

### Get Your Profile

```bash
./calendly get-current-user
```

### List Events

```bash
./calendly list-events --user-uri "<YOUR_USER_URI>"
```

### Get Event Details

```bash
./calendly get-event --event-uuid "<EVENT_UUID>"
```

### Cancel Event

```bash
./calendly cancel-event --event-uuid "<EVENT_UUID>" --reason "Rescheduling needed"
```

## Available Commands

- `get-current-user` - Get authenticated user details
- `list-events` - List scheduled events
- `get-event` - Get event details
- `cancel-event` - Cancel an event
- `list-event-invitees` - List invitees for an event
- `list-organization-memberships` - List organization memberships
- OAuth tools (`get-oauth-url`, `exchange-code-for-tokens`, `refresh-access-token`)

## Integration with Clawdbot

Add to your Clawdbot skills directory:

```bash
ln -s $(pwd) ~/.clawdbot/skills/calendly
```

Or for the main agent workspace:

```bash
ln -s $(pwd) ~/clawd/skills/calendly
```

Then use in conversations:
- "What meetings do I have?"
- "Cancel my 2pm meeting"
- "Who's attending my next call?"

## Development

This skill wraps the [calendly-mcp-server](https://github.com/meAmitPatil/calendly-mcp-server) MCP server via [mcporter](https://github.com/steipete/mcporter).

To regenerate the CLI (if the upstream MCP server updates):

```bash
# Configure mcporter
cat > mcporter.json <<EOF
{
  "mcpServers": {
    "calendly": {
      "command": "npx",
      "args": ["-y", "calendly-mcp-server"],
      "env": {
        "CALENDLY_API_KEY": "\${CALENDLY_API_KEY}"
      }
    }
  }
}
EOF

# Generate CLI
MCPORTER_CONFIG=./mcporter.json mcporter generate-cli --server calendly --output calendly
```

## License

MIT

## Credits

- MCP Server: [meAmitPatil/calendly-mcp-server](https://github.com/meAmitPatil/calendly-mcp-server)
- CLI Generator: [mcporter](https://github.com/steipete/mcporter)
- Clawdbot: [docs.clawd.bot](https://docs.clawd.bot)
