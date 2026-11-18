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

- **One server.** The plugin itself connects only to Anam's MCP server at
  `https://lab.anam.ai/mcp/claude`, run by Anam. It carries no API key or
  other credential. The services listed under "Anam's processors" below are
  reached by Anam's server, not by the plugin or your computer.
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
- **Anam's processors.** Anam's server passes some of that data on. These
  are the destinations the repository shows for this connector's tools:
  - **Persona design.** The `anam_persona_from_brief` tool (which
    `create-persona`, `tutor`, `interview-practice` and
    `pick-voice-and-avatar` can use) sends the description to Google's
    Gemini model, through Vercel's AI Gateway, to write the persona's prompt
    and greeting.
  - **Knowledge.** Pasted text and the content of a page or file fetched
    from a URL you give are stored as a document in Anam's Cloudflare R2
    storage. Anam's knowledge service (rag-service, run by Anam in its own
    regional clusters) then splits it into chunks, turns them into
    embeddings and keeps them for search. Its default embedding model is
    OpenAI's `text-embedding-3-large` through Azure OpenAI; Anam can choose
    `voyage-4` for an organization's new uploads instead. Knowledge searches
    send the search text to the same service. Fetching a URL means Anam's
    server requests it from that site.
  - **Documentation.** Searches go to Mintlify, which hosts Anam's docs.
    Send documentation terms, not customer content.
  - **Session moderation.** After people use a persona, Anam's
    acceptable-use check can send the system prompt its sessions used to
    Google's Gemini model, with Anthropic's Claude Haiku as a fallback, both
    through Vercel's AI Gateway. Flagged prompts, with the organization's
    name, are posted to Anam's internal Slack for review. This runs on
    sessions, whichever app created the persona.
  - **Product analytics (PostHog).** Each tool call sends an event with
    the tool's name, whether it worked, how long it took, the size of the
    result, the names (not the values) of its arguments, the persona's ID if
    there is one, the kind of connection and access, and the connected app:
    the kind of client, a hash of its client ID, its web host when it
    signs in with a client metadata document, the publisher's name when
    Anam verified it, and its name as one of a fixed set of labels or
    "other". Your Anam organization and user IDs go with it. Some actions
    send their own events too: creating a share link sends its ID, type,
    usage limit, expiry and persona ID; creating a persona sends a
    `persona_created` event with its ID and the tool that made it; using a
    starter prompt sends the prompt's name and which arguments were filled
    in; and connecting, the first request, disconnecting, refusing consent
    and a duplicate connection each send an event with the same app
    details and access level.
- **Code for your app.** `add-to-website` writes code into your project or
  hands it to you. That code is yours to run: its server calls Anam's API
  with your own Anam API key to start sessions, and every session uses your
  organization's minutes. Claude leaves starting it and creating the key to
  you.
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
