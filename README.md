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
- Hooks that report each Claude Code session to Manyhands as an agent run. See "Run tracking".

## Run tracking

When the plugin is installed and you are signed in to `manyhands` with `/mcp`, each
Claude Code session shows in the Agents section of your HQ as one run of an agent named
`Claude Code (` plus the part of your email before the `@` plus `)`. Four hooks do this.
No command runs on your machine and no token is stored.

- Your first prompt in a session starts the run.
- The end of each reply saves that reply as the run summary. Manyhands keeps the first
  1500 characters.
- When a subagent that has a type stops, the run's subagent count goes up by 1.
- When the session ends, the run is marked as succeeded. Claude Code gives this hook 1.5
  seconds. If it is missed, the run shows as stale after 10 minutes and as timed out
  after 60 minutes.

Only you can read the summary and details of your own sessions. Other members of your
HQ see counts. Claude Code does not give hooks a cost, so the cost of a session is blank.

The hooks name no HQ. When you belong to several HQs, the run goes to the oldest of
them. If you are not signed in to `manyhands`, Claude Code shows a hook error on each
prompt: run `/mcp` and sign in. To stop reporting, disable the plugin with `/plugin`.

## Claude.ai and Claude Desktop

These do not use plugins. Add `https://hub.pavilion.markets/mcp` as a custom connector
in Settings, then sign in.

## Without the plugin

`claude mcp add --transport http manyhands https://hub.pavilion.markets/mcp` gives you the
tools without the skills.

## Development

`npm test` checks the manifests, hooks and skills. `claude plugin validate .` checks
them the way Claude Code will.
