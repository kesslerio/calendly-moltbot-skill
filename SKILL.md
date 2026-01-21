---
name: calendly
description: Calendly scheduling integration. List events, check availability, manage meetings via Calendly API.
---

# Calendly Skill

Interact with Calendly scheduling via MCP-generated CLI.

## Quick Start

```bash
# Get your Calendly profile (returns user URI)
calendly get-current-user

# List your scheduled events
calendly list-events --user-uri "<YOUR_USER_URI>"

# Get event details
calendly get-event --event-uuid <UUID>

# Cancel an event
calendly cancel-event --event-uuid <UUID> --reason "Rescheduling needed"
```

## Available Commands

### User Info
- `get-current-user` - Get authenticated user details

### Events
- `list-events` - List scheduled events (requires --user-uri)
- `get-event` - Get event details (requires --event-uuid)
- `cancel-event` - Cancel an event (requires --event-uuid, optional --reason)

### Invitees
- `list-event-invitees` - List invitees for an event (requires --event-uuid)

### Organization
- `list-organization-memberships` - List organization memberships

## Configuration

API key is stored in `~/.clawdbot/.env`:
```
CALENDLY_API_KEY="<your-pat-token>"
```

Get your Personal Access Token from: https://calendly.com/integrations/api_webhooks

## Usage in Clawdbot

When user asks about:
- "What meetings do I have?" → `list-events`
- "Cancel my 2pm meeting" → Find with `list-events`, then `cancel-event`
- "Who's attending X meeting?" → `list-event-invitees`

## Getting Your User URI

Run `calendly get-current-user` to get your user URI. Example:
```json
{
  "resource": {
    "uri": "https://api.calendly.com/users/<YOUR_USER_UUID>",
    "scheduling_url": "https://calendly.com/<your-username>"
  }
}
```

## Examples

```bash
# List next 10 events
calendly list-events \
  --user-uri "<YOUR_USER_URI>" \
  -o json | jq .

# Get event details
calendly get-event \
  --event-uuid "<EVENT_UUID>" \
  -o json

# Cancel with reason
calendly cancel-event \
  --event-uuid "<EVENT_UUID>" \
  --reason "Rescheduling due to conflict"
```

## Notes

- All times in API responses are UTC (convert to Pacific for display)
- Event UUIDs are found in `list-events` output
- OAuth tools available but not needed with Personal Access Token

---

**Generated:** 2026-01-20
**Source:** meAmitPatil/calendly-mcp-server via mcporter
