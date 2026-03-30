# Clio Skills for Claude

Claude skills for integrating with [Clio Manage](https://www.clio.com/), the legal practice management platform.

## What Are Claude Skills?

Skills are instruction files that teach Claude how to use connected tools in a consistent, repeatable way. Drop a skill file into a Claude Project (or reference it in your system prompt) and Claude will follow its workflow automatically whenever the trigger phrases are detected.

## Requirements

- Claude Pro, Team, or Enterprise (for Projects)
- **Zapier MCP** connected to Claude with Clio actions enabled
- A Clio Manage account with API access via Zapier

## Skills

### `clio-time-entry-SKILL.md`

Logs a billable time entry to Clio from plain English or a slash command.

**Triggers:** `/time`, "log time", "bill time", "I spent X hours on", "log X hours to", or any plain-language description of billable work.

**Example:**
```
log 0.5 hours on the Smith matter for reviewing the draft trust
```

Claude will find the matter, confirm with you, then post the entry. See `clio-time-entry-setup.md` for setup instructions.

## Setup

See `clio-time-entry-setup.md` for step-by-step Zapier MCP configuration.

## Contributing

Pull requests welcome. Skills should follow the same structure: frontmatter with `name` and `description`, plain-English workflow steps, and explicit rules at the bottom.

## License

MIT
