# Calendly Clawdbot Skill

Clawdbot skill for Calendly integration. List events, check availability, manage meetings via the Calendly API.

## Features

- **User Info**: Get authenticated user details
- **Event Management**: List, view, and cancel scheduled events
- **Invitee Management**: View event invitees
- **Organization**: List organization memberships
- **Scheduling API**: Discover event types, check availability, and schedule meetings programmatically

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

**Note:** Scheduling API features require a paid Calendly plan (Standard or higher).

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

### Scheduling Workflow

```bash
# 1. List your available event types
./calendly list-event-types

# 2. Check availability for a specific event type
./calendly get-event-type-availability --event-type "<EVENT_TYPE_URI>"

# 3. Schedule a meeting at an available time (requires paid plan)
./calendly schedule-event \
  --event-type "<EVENT_TYPE_URI>" \
  --start-time "2026-01-25T19:00:00Z" \
  --invitee-email "client@company.com" \
  --invitee-name "John Smith" \
  --invitee-timezone "America/New_York"
```

## Available Commands

### Event Management
- `get-current-user` - Get authenticated user details
- `list-events` - List scheduled events
- `get-event` - Get event details
- `cancel-event` - Cancel an event
- `list-event-invitees` - List invitees for an event
- `list-organization-memberships` - List organization memberships

### Scheduling API (requires paid plan)
- `list-event-types` - List available event types for scheduling
- `get-event-type-availability` - Get available time slots for an event type
- `schedule-event` - Schedule a meeting programmatically

### OAuth
- `get-oauth-url` - Generate OAuth authorization URL
- `exchange-code-for-tokens` - Exchange authorization code for tokens
- `refresh-access-token` - Refresh access token

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
- "What event types do I have available?"
- "Check my availability for next Tuesday"
- "Schedule a meeting with john@company.com tomorrow at 2 PM"

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

**Note:** If using unreleased features from GitHub, point to the repository:
```bash
# Use latest from GitHub (for unreleased v2.0+ features)
"args": ["github:meAmitPatil/calendly-mcp-server"]
```

## License

MIT

## Credits

- MCP Server: [meAmitPatil/calendly-mcp-server](https://github.com/meAmitPatil/calendly-mcp-server)
- CLI Generator: [mcporter](https://github.com/steipete/mcporter)
- Clawdbot: [docs.clawd.bot](https://docs.clawd.bot)
