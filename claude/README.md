# Anam for Claude

[Anam](https://anam.ai) makes personas: real-time AI avatars that talk with
people over video, for customer support, sales, reception, tutoring and
interview practice. This plugin lets Claude build and run them in your Anam
account. It adds Anam's connector and skills, recipes that tell Claude how to
use the connector's tools for one task each.

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
| `improve-persona`       | Find where callers got stuck in recent sessions and fix the prompt, greeting or knowledge.     |
| `review-sessions`       | Report on a period's sessions: volume, errors, slow replies, failed tool calls and quotes.     |
| `add-to-website`        | Put a persona on a site with a share link, the widget or code with a server-side token.        |

Ask in your own words, such as "Make a receptionist for my dental clinic and
give me a link to try it", or pick a skill. Conversations with a persona
happen in your browser through its share link, or in your own app, not in
Claude.

Claude's directory policy excludes connectors that generate images, video or
audio with AI, so this version leaves out making an avatar from a photo,
sending a persona into a video meeting, and cloning or importing a voice. They
stay available in [Anam Lab](https://lab.anam.ai).

## Install

You need an Anam account. Sign up at [lab.anam.ai](https://lab.anam.ai).

From Anthropic's directory, add Anam, then connect the Anam connector on the
plugin's Connectors tab. On Team and Enterprise plans an Owner adds the
connector for the organization first.

In Claude Code, add this repository as a marketplace and install the plugin:

```text
/plugin marketplace add anam-org/anam-agent-tools
/plugin install anam@anam-agent-tools
```

Then run `/mcp`, choose `anam` and sign in.

## What it connects to

The plugin holds configuration and instructions only: its manifest, one MCP
server entry, the skills and this README. It has no hooks, scripts, commands
or agents, so nothing from it runs on your computer.

- **One server.** It connects only to Anam's MCP server at
  `https://lab.anam.ai/mcp/claude`, run by Anam. It carries no API key or
  other credential.
- **Sign-in.** You sign in with OAuth in your browser and choose one Anam
  organization and full or read-only access. Claude acts
  only in the Anam organization you chose, with the access you chose.
  Read-only access still reads everything in it, including session
  transcripts, recordings and knowledge; full access can also change and
  delete things.
- **What goes to Anam.** What Claude sends to the connector's tools: persona
  names, prompts and settings, knowledge text and the URLs of pages or files
  to add, search terms, and questions about Anam's documentation. Claude
  receives what the tools return, such as personas, knowledge, share links,
  session transcripts and usage.
- **What Anam's server reaches.** It fetches the public web pages and files
  you ask to add as knowledge, and sends documentation searches to Mintlify,
  which hosts Anam's docs.
- **Billing.** Knowledge documents count towards your plan's monthly upload
  allowance and conversations through share links use your organization's
  minutes, as they do in Anam Lab. The skills ask before that work.

Manage or disconnect apps in Anam Lab under Settings, Connected apps
([lab.anam.ai/ai-apps](https://lab.anam.ai/ai-apps)). See Anam's
[privacy policy](https://anam.ai/privacy-policy) and
[terms of service](https://anam.ai/terms-of-service).

## Support

- [Anam MCP server docs](https://anam.ai/docs/resources/mcp)
- [Setup guide](https://lab.anam.ai/mcp)
- Support: [support@anam.ai](mailto:support@anam.ai)

## License

MIT; see [LICENSE](LICENSE). The license covers the plugin's files, not the
Anam service.
