# Anam agent skills

Skills for building and running [Anam](https://anam.ai) personas from AI
assistants and coding agents such as ChatGPT, Claude Code, Cursor, Codex and
VS Code. Anam makes personas: real-time AI avatars that talk with people over
video.

Each skill is a recipe for one task, written for the tools of Anam's MCP
server, which Anam hosts at `https://lab.anam.ai/mcp`. The skills follow the
[Agent Skills](https://agentskills.io) format, and this repository is also a
Cursor Marketplace plugin. It contains configuration and instructions only.

## Skills

| Skill                   | Use it to                                                                                      |
| ----------------------- | ---------------------------------------------------------------------------------------------- |
| `get-started`           | Check the connection and make a first persona after connecting Anam.                           |
| `create-persona`        | Create a persona from a short brief and get a share link to talk to it.                        |
| `ai-receptionist`       | Set up a receptionist that greets visitors, answers from the business's facts and takes notes. |
| `support-agent`         | Set up a support agent that answers from help articles and says how to reach the team.         |
| `tutor`                 | Set up a tutor for a subject, level or language.                                               |
| `interview-practice`    | Set up a mock or screening interviewer for a role.                                             |
| `add-knowledge`         | Give a persona knowledge from web pages, public files or pasted text, and test it.             |
| `pick-voice-and-avatar` | Find avatars and voices by language, accent and look, and apply them.                          |
| `avatar-from-photo`     | Make a custom avatar from a photo the user has the rights to.                                  |
| `improve-persona`       | Find where callers got stuck in recent sessions and fix the prompt, greeting or knowledge.     |
| `review-sessions`       | Report on a period's sessions: volume, errors, slow replies, failed tool calls and quotes.     |
| `send-to-meeting`       | Send a persona into a Google Meet, Zoom or Microsoft Teams call.                               |
| `add-to-website`        | Put a persona on a site with a share link, the widget or code with a server-side token.        |

The same skills ship in the Anam AI Avatars plugin for ChatGPT. They name
only tools that both the ChatGPT plugin and `https://lab.anam.ai/mcp` serve.
`avatar-from-photo` takes an attached photo only in ChatGPT; elsewhere it
needs a public image URL. Anam's Claude directory connector leaves out avatar
creation and meeting invites, so `avatar-from-photo` and `send-to-meeting`
need the full server URL above.

## Install

You need an Anam account. Sign up at [lab.anam.ai](https://lab.anam.ai).
Skills don't connect the MCP server; connect it too, as below.

### ChatGPT

Add the [Anam AI Avatars plugin](https://chatgpt.com/plugins/plugin_asdk_app_6aedcff4354481918e2c08d4a4d79d66).
It includes these skills and the connection.

### Cursor

Install **Anam** from the Cursor Marketplace. It adds the MCP server and the
skills. Or add only the MCP server with this link:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=anam&config=eyJ1cmwiOiJodHRwczovL2xhYi5hbmFtLmFpL21jcCJ9
```

### Skills for Claude Code, Codex and other agents

```bash
npx skills add anam-org/anam-agent-tools
```

Add `--skill create-persona` for one skill, `-a claude-code` or `-a cursor`
for one agent, and `-g` to install for your user.

### Connect the MCP server

Claude Code:

```bash
claude mcp add --transport http --scope user anam https://lab.anam.ai/mcp
claude mcp login anam
```

Codex:

```bash
codex mcp add anam --url https://lab.anam.ai/mcp
codex mcp login anam
```

VS Code:

```bash
code --add-mcp '{"name":"anam","type":"http","url":"https://lab.anam.ai/mcp"}'
```

Any other MCP client: add a remote (Streamable HTTP) server at
`https://lab.anam.ai/mcp`. It signs in with OAuth.

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
creation, meeting invites and knowledge uploads count towards your Anam plan,
as they do in Anam Lab, and the skills ask before that work. See the
[privacy policy](https://anam.ai/privacy-policy) and
[terms of service](https://anam.ai/terms-of-service).

## Documentation and support

- [Setup guide](https://lab.anam.ai/mcp) for every app, API keys and access
- [MCP server docs](https://anam.ai/docs/resources/mcp)
- [Anam docs](https://anam.ai/docs)
- [Security](https://anam.ai/security)
- Support: [support@anam.ai](mailto:support@anam.ai)

## License

See [LICENSE](LICENSE). The license covers this repository's files, not the
Anam service.
