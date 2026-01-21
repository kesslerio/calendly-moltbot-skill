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

## Scheduling Workflow

```bash
# 1. List available event types
calendly list-event-types

# 2. Check availability for a specific event type
calendly get-event-type-availability --event-type "<EVENT_TYPE_URI>"

# 3. Schedule a meeting (requires paid Calendly plan)
calendly schedule-event \
  --event-type "<EVENT_TYPE_URI>" \
  --start-time "2026-01-25T19:00:00Z" \
  --invitee-email "client@company.com" \
  --invitee-name "John Smith" \
  --invitee-timezone "America/New_York"
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

### Scheduling API (requires paid plan)
- `list-event-types` - List available event types for scheduling
- `get-event-type-availability` - Get available time slots for an event type
- `schedule-event` - Schedule a meeting programmatically

## Configuration

API key is stored in `~/.clawdbot/.env`:
```
CALENDLY_API_KEY="<your-pat-token>"
```

Get your Personal Access Token from: https://calendly.com/integrations/api_webhooks

**Note:** Scheduling API features (list-event-types, get-event-type-availability, schedule-event) require a paid Calendly plan (Standard or higher).

## Usage in Clawdbot

When user asks about:
- "What meetings do I have?" → `list-events`
- "Cancel my 2pm meeting" → Find with `list-events`, then `cancel-event`
- "Who's attending X meeting?" → `list-event-invitees`
- "What event types do I have?" → `list-event-types`
- "Check my availability next Tuesday" → `get-event-type-availability`
- "Schedule a meeting with john@company.com tomorrow at 2 PM" → `schedule-event`

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

# List your event types
calendly list-event-types -o json | jq .

# Check availability for a 30-min consultation
calendly get-event-type-availability \
  --event-type "<EVENT_TYPE_URI>" \
  --start-time "2026-01-21T00:00:00Z" \
  --end-time "2026-01-28T23:59:59Z" \
  -o json | jq .
```

## Notes

- All times in API responses are UTC (convert to Pacific for display)
- Event UUIDs are found in `list-events` output
- OAuth tools available but not needed with Personal Access Token
- Scheduling API requires paid Calendly plan (Standard or higher)

---

**Generated:** 2026-01-20  
**Updated:** 2026-01-21 (Added Scheduling API support)  
**Source:** meAmitPatil/calendly-mcp-server via mcporter
