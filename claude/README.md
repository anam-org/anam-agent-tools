# Anam for Claude

[Anam](https://anam.ai) makes personas: AI avatars that people talk to over
live video, for customer support, sales, reception and more. With this
plugin, Claude sets personas up and looks after them in your Anam account. It
writes their prompts, picks an avatar and a voice, adds knowledge, creates
share links, reviews past conversations and helps you put a persona on your
site.

People talk to a persona in a browser, through its share link or your own
site or app. Those conversations run on Anam, not in Claude.

The plugin adds Anam's connector and 7 skills. Each skill tells Claude how to
use the connector's tools for one task.

## Skills

| Skill                   | Use it to                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------- |
| `get-started`           | Check the connection and make a first persona after connecting Anam.                            |
| `create-persona`        | Create a persona from a short brief and get a share link to talk to it.                         |
| `add-knowledge`         | Give a persona a new knowledge folder from web pages, public files or pasted text, and test it. |
| `pick-voice-and-avatar` | Find avatars and voices by language, accent and look, and apply them.                           |
| `improve-persona`       | Find where callers got stuck in recent sessions and fix the prompt, greeting or knowledge.      |
| `review-sessions`       | Report on a period's sessions: volume, errors, slow replies, failed tool calls and quotes.      |
| `add-to-website`        | Put a persona on a site with a share link, the widget or code with a server-side token.         |

Ask in your own words, or pick a skill. For example:

- "Make a receptionist for my dental clinic and give me a link to try it."
- "Add the FAQ below to a new knowledge folder for my receptionist."
- "Which of my personas had the most sessions last week, and what did people
  ask about most?"

Not in this plugin: making an avatar from a photo, sending a persona into a
video meeting, cloning or importing a voice, removing knowledge from a
persona, and starting a conversation from inside Claude. Use
[Anam Lab](https://lab.anam.ai) for those.

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

Then run `/mcp`, select the Anam server and sign in.

## Where your data goes

The plugin is configuration and instructions: a manifest, one connector
entry, the skills and this README. It has no hooks, scripts, commands or
agents, so nothing from it runs on your computer.

### The plugin connects to one server

That server is Anam's MCP server at `https://lab.anam.ai/mcp/claude`, run by
Anam. You sign in with OAuth in your browser and choose one Anam organization
and full or read-only access. The plugin holds no API key. Claude acts
only in the Anam organization you chose, with the access you chose.
Read-only access still reads everything in it, including session
transcripts, recordings and knowledge; full access can also change and
delete things.

Claude sends Anam what you ask it to work with: persona names, prompts and
settings, knowledge text, the URLs of pages or files to add, search terms and
documentation questions. Anam returns what is in your account, such as
personas, knowledge, share links, session transcripts and usage.

### Services Anam's server passes data to

Anam's server, not the plugin or your computer, sends some of that data on:

- **Persona descriptions.** When Claude asks Anam to write a persona from a
  description, the description goes to Google Gemini through Vercel's AI
  Gateway. Anam's request tracing in Grafana records that request, including
  the description and the prompt Gemini writes, for troubleshooting.
- **Knowledge.** Documents you add are stored in Cloudflare R2 and indexed
  for search in Upstash Vector. To build the index, Anam sends their text to
  an embedding model: OpenAI's on Microsoft Azure, or Voyage AI's hosted by
  MongoDB. A search sends your search terms to the same embedding model, then
  sends the terms and the passages it found to Voyage AI, hosted by MongoDB,
  to rank them. Anam's server fetches a page or file you add by URL from that
  site.
- **Documentation searches.** These go to Mintlify, which hosts Anam's docs,
  without your account details.
- **Acceptable-use checks.** After a persona has had conversations, Anam can
  check its system prompt with Google Gemini, or with Anthropic's Claude when
  Gemini is unavailable, through Vercel's AI Gateway. Anam's request tracing
  in Grafana records these checks, prompts included, and Upstash Redis keeps
  each result (a hash of the prompt, the decision, its reason and severity)
  for three days. If a check flags a prompt, part of it and the check's
  written reason may be shared with Anam's team in Slack for review, with
  your organization's name and the session's ID.
- **Visitor feedback.** If someone gives feedback after a conversation on a
  share link, their ratings, comment and any email they enter go to Anam's
  team in Slack, with the persona, your organization and the link.
- **Call quality checks.** After some conversations with a persona, Anam
  sends the transcript (all of a short call, or parts of a longer one) and
  the persona's settings to an OpenAI model through Vercel's AI Gateway to
  score how the call went. Conversations with zero data retention on are
  skipped; that setting can differ between personas.
- **Email.** If you connect an app and don't use it for an hour, Anam may
  email you through Resend with tips; the email names the app and the
  organization.
- **Voice deletion.** If you ask Claude to delete a custom voice, Anam deletes
  it from your account and then tries to delete it at its provider, Cartesia
  or ElevenLabs, once no other Anam voice uses it. That provider step covers
  eligible voices only and can fail.
- **Usage analytics.** PostHog receives a record of each tool call: the tool,
  whether it worked, how long it took, the size of the result, the names of
  the arguments sent but not their values, the access level, which app made
  the call and the ID of the persona it acted on, with your Anam organization
  ID and, where available, your user ID. It also receives events when
  personas and share links are created (for a share link, its usage limit
  and expiry) and when apps connect or disconnect.

If you connect your own language model or give a persona a webhook tool, Anam
sends conversation content to the URL you chose, with the key or secret you
saved for it. Enter keys and secrets in Anam Lab, not in a chat.

Anam's hosting, database and logging providers, and the speech and language
providers that live conversations use, are in the full list at
[trust.anam.ai/subprocessors](https://trust.anam.ai/subprocessors). Anam's
[privacy policy](https://anam.ai/privacy-policy) covers AI apps in section 12
and retention in section 6.

### Code and costs

`add-to-website` can write starter code into your project or hand it to you.
That code is yours to run: its server calls Anam's API with your own Anam API
key to start conversations. Claude leaves creating the key and starting the
app to you.

Knowledge documents count towards your plan's monthly upload allowance, and
conversations use your organization's minutes, as they do in Anam Lab. The
skills tell you before adding knowledge. Creating a persona also creates a
share link, good for 10 conversations over 7 days by default, which uses
minutes only when someone opens it and talks.

Manage or disconnect apps in Anam Lab under Settings, Connected apps
([lab.anam.ai/ai-apps](https://lab.anam.ai/ai-apps)). See Anam's
[terms of service](https://anam.ai/terms-of-service).

## Support

- [Anam MCP server docs](https://anam.ai/docs/resources/mcp)
- [Setup guide](https://lab.anam.ai/mcp)
- Support: [support@anam.ai](mailto:support@anam.ai)

## License

MIT; see the LICENSE file. The license covers the plugin's files, not the
Anam service.
