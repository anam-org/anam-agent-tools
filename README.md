# Anam agent tools

Skills and MCP configuration for building with [Anam](https://anam.ai) from
coding agents such as Cursor, Claude Code, Codex and VS Code. Anam makes
personas: real-time AI avatars that talk with people over video.

This repository is a Cursor Marketplace plugin and an
[Agent Skills](https://agentskills.io) collection. It contains configuration
and instructions only. The Anam MCP server it connects to is hosted by Anam at
`https://lab.anam.ai/mcp`.

## What's included

| Path                                   | What it does                                                                   |
| -------------------------------------- | ------------------------------------------------------------------------------ |
| `mcp.json`                             | Connects your agent to the Anam MCP server.                                    |
| `skills/build-anam-persona`            | Create or improve a persona from a brief: avatar, voice, prompt and knowledge. |
| `skills/integrate-anam-into-an-app`    | Add a persona to a website or app with the server-side session-token pattern.  |
| `skills/review-anam-sessions`          | Find sessions, read transcripts and summarize what people asked.               |
| `.cursor-plugin/plugin.json`, `assets` | The Cursor plugin manifest and logo.                                           |

## Install

You need an Anam account. Sign up at [lab.anam.ai](https://lab.anam.ai).

### Cursor

Install **Anam** from the Cursor Marketplace. It adds the MCP server and the
skills. Or add only the MCP server with this link:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=anam&config=eyJ1cmwiOiJodHRwczovL2xhYi5hbmFtLmFpL21jcCJ9
```

### Skills for any agent

```bash
npx skills add anam-org/anam-agent-tools
```

Add `--skill build-anam-persona` for one skill, `-a cursor` or
`-a claude-code` for one agent, and `-g` to install for your user. Skills don't
connect the MCP server; add it as below.

### Claude Code

```bash
claude mcp add --transport http --scope user anam https://lab.anam.ai/mcp
claude mcp login anam
```

### Codex

```bash
codex mcp add anam --url https://lab.anam.ai/mcp
codex mcp login anam
```

### VS Code

```bash
code --add-mcp '{"name":"anam","type":"http","url":"https://lab.anam.ai/mcp"}'
```

## Signing in

The first time your agent uses Anam, it opens a browser so you can sign in.
You choose the Anam organization to connect and whether the agent gets full
or read-only access. Manage or disconnect apps at
[lab.anam.ai/ai-apps](https://lab.anam.ai/ai-apps).

Agents that run without a browser can send an Anam API key instead, as
`Authorization: Bearer <key>`. Keep the key in an environment variable, never
in a committed file.

## Data and billing

The agent acts only in the Anam organization you chose, with the access you
chose. Read-only access still reads everything in it, including session
transcripts, recordings and knowledge; full access can also change and delete
things. With an API key, the agent has that key's permissions. Sessions, avatar
videos and knowledge uploads count towards your Anam plan, as they do in Anam
Lab. See the [privacy policy](https://anam.ai/privacy-policy) and
[terms of service](https://anam.ai/terms-of-service).

## Documentation and support

- [Setup guide](https://lab.anam.ai/mcp) for every app, API keys and access
- [Anam docs](https://anam.ai/docs)
- [Security](https://anam.ai/security)
- Support: [support@anam.ai](mailto:support@anam.ai)

## License

See [LICENSE](LICENSE). The license covers this repository's files, not the
Anam service.
