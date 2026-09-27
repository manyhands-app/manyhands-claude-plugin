# Manyhands for Claude Code

Use your Manyhands HQ from Claude Code. Your own Claude does the thinking; Manyhands runs
the tools as you, with the same permissions you have in the app.

## Install

In Claude Code:

```
/plugin marketplace add manyhands-app/manyhands-claude-plugin
/plugin install manyhands@manyhands
```

Then run `/mcp`, choose `manyhands`, and sign in with your Manyhands account.

You need a Manyhands account. Without one, the plugin installs but every tool call is
refused.

## What you get

- The `manyhands` MCP server (`https://hub.pavilion.markets/mcp`): list, read and search
  tasks, documents, decisions and projects; apply task and project changes; undo them;
  comment; list and add to your private notes; run the onboarding interview.
- Skills that tell Claude when and how to use those tools:
  - `manyhands`: the tools and the rules for using them.
  - `notes-to-tasks`: turn rough notes into task changes.
  - `jot`: send one line to the right note, or make it a task.
  - `interview`: load your company's work into your HQ, one office at a time.
- Three MCP prompts, which carry the in-app rules word for word:
  `/mcp__manyhands__notes-to-tasks`, `/mcp__manyhands__jot` and `/mcp__manyhands__interview`.

## Claude.ai and Claude Desktop

These do not use plugins. Add `https://hub.pavilion.markets/mcp` as a custom connector
in Settings, then sign in.

## Without the plugin

`claude mcp add --transport http manyhands https://hub.pavilion.markets/mcp` gives you the
tools without the skills.

## Development

`npm test` checks the manifests and skills. `claude plugin validate .` checks them the
way Claude Code will.
