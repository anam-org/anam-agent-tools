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

- **New persona**: use the `create-persona` skill with the chosen
  `avatarId` and `voiceId`.
- **Existing persona**: find it with `anam_persona_list`, then call
  `anam_persona_update` with `id`, `avatarId` and/or `voiceId`, and
  `languageCode` if the language changes. A new voice resets the voice's
  generation settings, and a new avatar may reset the avatar model if the
  avatar doesn't support it. The change is published at once, and
  replaces any unpublished draft of the persona in Anam Lab: if someone
  is editing it there, ask them to publish or discard their draft first.

Then, to hear and see the result, give the user an active share link
for the persona. Call `anam_persona_get`: if its `primaryShareLink` is
turned off (`enabled` false), the organization has stopped sharing the
persona, so don't make a new link and suggest trying it in Anam Lab.
If it's on and its entry in `shareLinks` (the one with `isPrimary`)
has at least 30 minutes left and hasn't used up its `usageLimit`, the link is
https://lab.anam.ai/share/ followed by its `token`. Otherwise use a
link from `anam_share_link_list` (`personaId`) with `type` `LINK` that
is enabled, has at least 30 minutes left and isn't used up. If there isn't one, ask before
creating one with `anam_share_link_create`, since a new link is
public.

## Rules

- Only use a real person's face or voice with their consent.
