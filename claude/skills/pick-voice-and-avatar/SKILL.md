---
name: pick-voice-and-avatar
description: Find and compare Anam avatars and voices by language, accent, gender, look and tone, with preview images and voice samples, then apply the choice to a persona. Use when the user wants to choose, browse or change the face or voice of an AI avatar, or needs a voice that speaks a particular language or accent.
compatibility: Needs the Anam MCP server connected to an Anam account (the Anam plugin in Claude, or https://lab.anam.ai/mcp elsewhere).
---

# Pick a voice and an avatar

The organization can use Anam's stock avatars and voices and any it has made.
Recommend from what the tools return; use only IDs they return.

## 1. Narrow down what the user wants

Ask only about what matters to them:

- **Voice**: language, accent (a country such as `GB`, `US` or `MX`), gender,
  and tone (warm, calm, energetic).
- **Avatar**: realistic, 3D-animated or illustrated; the look of the person
  and the setting (office, home, outdoors).
- Whether it's for a new persona or an existing one.

## 2. Search

Call `anam_options` with what you know:

- `language` (ISO 639-1, such as `es`), `gender` (`male`, `female`,
  `neutral`) and `country` (the accent) filter voices.
- `avatarStyle` (`realistic`, `animated_3d`, `illustrated`) filters avatars.
- `search` matches names, descriptions and tags. Broad words work best
  (`woman`, `office`, `friendly`, `calm`); role words such as `receptionist`
  rarely match.
- `includeMedia: true` adds image and voice sample URLs, so the user can see
  and hear the options.
- `include` limits the lists, such as `["voices"]`.

Each list names Anam's recommended pick when it matches the filters
(`recommended`). To page through more, use `anam_voice_list` or
`anam_avatar_list`, which also take `search`; `anam_voice_get` and
`anam_avatar_get` show one in full.

If nothing fits, drop a filter and try again. If nothing fits even then,
offer the closest matches and say how they differ from what the user asked
for.

## 3. Recommend

Offer two or three avatars and two or three voices, each with its name, a
short reason tied to what the user asked, and its preview link. Check that
each voice lists the persona's language.

## 4. Apply the choice

- **New persona**: if it will interview real job candidates, for
  screening or hiring, don't create it here. Use the `interview-practice`
  skill with the chosen `avatarId` and `voiceId`: it sets up what
  candidates are told and who makes the decision before any persona or
  link exists. Otherwise pass `avatarId` and `voiceId` to
  `anam_quickstart` or `anam_persona_from_brief` (see `create-persona`).
- **Existing persona**: find it with `anam_persona_list`, then call
  `anam_persona_update` with `id`, `avatarId` and/or `voiceId`, and
  `languageCode` if the language changes. A new voice resets the voice's
  generation settings, and a new avatar may reset the avatar model if the
  avatar doesn't support it. The change is published at once.

Then give the user a share link to hear and see the result: from
`anam_share_link_list` (`personaId`), or a new one from
`anam_share_link_create`.

## Rules

- Only use a real person's face or voice with their consent.
