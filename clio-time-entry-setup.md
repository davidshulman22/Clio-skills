# Clio Time Entry Skill — Setup Guide

This skill lets Claude log billable time entries to Clio Manage using plain English or a slash command.

## Requirements

- **Clio Manage** account with API access
- **Zapier MCP** connected to Claude with the following Clio actions enabled:
  - `clio_find_matter`
  - `clio_create_time_entry_activity`

## Zapier MCP Setup

1. Go to [zapier.com/mcp](https://zapier.com/mcp) and create or open your MCP server.
2. Add the **Clio** app and authenticate with your Clio account.
3. Enable these two actions:
   - **Find Matter** (`clio_find_matter`)
   - **Create Time Entry Activity** (`clio_create_time_entry_activity`)
4. Copy your Zapier MCP server URL.

## Claude Setup

### Claude.ai (Projects)
1. Open your Project → Settings → Integrations.
2. Add your Zapier MCP server URL.
3. Upload `clio-time-entry-SKILL.md` to the Project files.

### Claude Code / API
Add the Zapier MCP server to your `claude_desktop_config.json` or MCP config, then include the skill file in your system prompt or context.

## Usage

Once connected, trigger the skill with any of these:

- `/time smith 0.5 reviewed draft trust`
- `log 1.2 hours on the Jones matter for telephone conference`
- `bill 0.3 to Johnson for reviewing correspondence`

Claude will find the matter, ask you to confirm, and post the entry. It will never post without your confirmation.

## Notes

- Time defaults to today's date. Override by including a date in YYYY-MM-DD format after the matter name.
- Rate is always pulled from the matter's default in Clio — never passed explicitly.
- All entries are posted as billable.
