---
name: get-started
description: Get started with Anam after connecting it. Use when the user has just installed or connected Anam, asks what Anam can do, or wants to make their first AI avatar persona.
compatibility: Needs the Anam MCP server connected to an Anam account (the Anam plugin in Claude, or https://lab.anam.ai/mcp elsewhere).
---

# Get started with Anam

Anam personas are real-time AI avatars that talk with people over video. This
plugin manages the user's Anam account: personas, avatars, voices, knowledge,
share links and past sessions.

## 1. Check the connection

Call `anam_connection_info`. Tell the user which organization is connected and
whether the access is full or read only. If it's read only, say that creating
or changing things needs a new connection with full access, and continue with
read-only help.

## 2. Offer a first persona

Ask what the persona is for, in one question: for example a receptionist,
support agent, sales assistant or a demo of Anam itself. This plugin
doesn't set up personas that interview or assess real job candidates, or
that make or recommend decisions about people, such as triaging patients
or judging who qualifies for credit, or that give medical, legal or
financial advice: if that's what the user wants, say so and point them
to Anam Lab.
Otherwise:

1. Optionally call `anam_options` to suggest an avatar and voice. Use only IDs
   it returns.
2. If the role is a sales assistant, support agent, receptionist or a
   demo of Anam, call `anam_quickstart` with a name, that `template`
   (`sales`, `support`, `receptionist` or `anam-demo`) and any chosen
   IDs. For any other role, use the `create-persona` skill with the
   user's description instead. Quickstart also creates a share link
   unless `createShareLink` is false. Then call `anam_persona_get` for
   its prompt (`brain`), greeting (`initialMessage`) and `updatedAt`.
   The greeting should say it's an AI, and the prompt should have a
   line telling the persona it is an AI and to say so when asked. Fix
   what's missing with `anam_persona_update` (`id`, `initialMessage`,
   the whole `systemPrompt` with the line added, and
   `expectedUpdatedAt`). If the prompt ends with a `[cut: …]` marker,
   don't send it back: tell the user to add the line in Anam Lab.
3. Give the user the share link URL from the result so they can talk to the
   persona in their browser, and the persona's name, avatar and voice.

Sessions started from a share link use the organization's minutes, so say so
when you hand over the link.

For a fuller walkthrough, use the `create-persona` skill.

## 3. Suggest next steps

Offer one or two of these, based on what the user wants:

- Pick a different look or voice: `pick-voice-and-avatar`.
- Teach it about their product or business: `add-knowledge`.
- See how calls went and fix what callers got stuck on: `review-sessions` and
  `improve-persona`.
- Put the persona on a website or in an app: `add-to-website`.

## Rules

- Never ask the user to paste a password or API key into the chat.
- Only use images and voices the user has the right to use. Don't recreate a
  real person without their consent.
- Ask before deleting anything.
