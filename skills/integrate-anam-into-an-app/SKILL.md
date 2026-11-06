---
name: integrate-anam-into-an-app
description: Add an Anam persona (a real-time AI video avatar) to a website or app. Use when the user wants to embed an Anam avatar in a web page, React or Next.js app, LiveKit or Pipecat voice agent, Python service, ElevenLabs agent or mobile app, or asks how Anam session tokens work.
compatibility: Needs the Anam MCP server (https://lab.anam.ai/mcp) connected with an Anam account, and the user's codebase.
metadata:
  author: anam
  version: '1.0.0'
---

# Integrate Anam into an app

## The one rule

The Anam API key stays on the server. The server exchanges it for a
short-lived session token for each conversation, and only that token reaches
the browser or app. Never put the API key in client code, a mobile app, a
committed file or the conversation.

## 1. Pick the persona and stack

1. Call `anam_connection_info` to confirm the organization.
2. Find the persona with `anam_persona_list`, or create one with the
   `build-anam-persona` skill. Note its ID.
3. Work out the stack from the user's code (`package.json`, `requirements.txt`
   and so on) or ask: `javascript`, `react`, `livekit`, `pipecat`, `python`,
   `elevenlabs`, `widget` or `mobile`.

## 2. Get current instructions

Call `anam_integration_guide` with `stack` and `personaId`. It returns the
package to install, a minimal snippet with the persona ID filled in, and the
guides to follow. For a Next.js, Express or FastAPI app, `anam_scaffold`
returns ready-to-run files instead: a server route that mints session tokens
and a client page, with the persona ID filled in. Read the guides with
`anam_docs_read` before writing more than that; search with `anam_docs_search` for anything else. Don't
rely on remembered SDK APIs: check them in the docs.

## 3. Write the code

For browser apps (`javascript`, `react`, `elevenlabs`):

1. A server endpoint (for example `POST /api/session-token`) that reads the
   API key from an environment variable such as `ANAM_API_KEY` and calls
   Anam's session-token endpoint with the persona ID, as the guide shows.
2. Client code that fetches a token from that endpoint, creates the client
   with `@anam-ai/js-sdk`, streams to a `<video>` element, and stops streaming
   when the page or component goes away.
3. `ANAM_API_KEY` in the server's environment and in `.env.example` without a
   value. Make sure real `.env` files are ignored by git.

For server-side stacks (`livekit`, `pipecat`, `python`), follow the guide; the
key lives in the agent's environment.

For `widget`, no code is needed beyond the embed snippet. The user publishes
the widget from the persona's Embed tab in Anam Lab and adds their site's
domain to the allowed domains.

## 4. Test

The user creates an API key at https://lab.anam.ai/api-keys and sets it in
their environment themselves. Run the app and check that the avatar appears
and answers. Every session uses the organization's minutes.

If the user wants to try the persona before the app is ready, a share link is
quicker: `anam_share_link_create`.

## Troubleshooting

- 401 from the session-token call: the key is wrong, revoked or missing from
  the server's environment.
- No video: check the `<video>` element has `autoplay` and `playsinline`, and
  that the page asked for microphone access.
- Look up errors with `anam_docs_search` before guessing.
