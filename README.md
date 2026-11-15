<div align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&amp;color=128C7E&amp;height=180&amp;section=header&amp;text=CFC-CCH&amp;fontSize=48&amp;fontColor=ffffff&amp;animation=fadeIn&amp;fontAlignY=38&amp;desc=Create%20Custom%20History&amp;descAlignY=58&amp;descSize=16" alt="CFC-CCH banner" width="100%" />
</div>

<h1 align="center">CFC-CCH</h1>

<p align="center">
  <strong>Create Custom History</strong><br />
  A single-file WhatsApp group-history builder for Baileys.
</p>

<p align="center">
  <b><a href="https://github.com/MyPioGit/cfc-cch/wiki/CFC%E2%80%90CCH_WIKI_EN">English</a></b> |
  <a href="https://github.com/MyPioGit/cfc-cch/wiki/CFC%E2%80%90CCH_WIKI_ID">Bahasa Indonesia</a> |
  <a href="https://github.com/MyPioGit/cfc-cch/wiki/CFC%E2%80%90CCH_WIKI_EG">العربية (مصر)</a> |
  <a href="https://github.com/MyPioGit/cfc-cch/wiki/CFC%E2%80%90CCH_WIKI_BR">Português (Brasil)</a> |
  <a href="https://github.com/MyPioGit/cfc-cch/wiki/CFC%E2%80%90CCH_WIKI_IN">हिन्दी (भारत)</a> |
  <a href="https://github.com/MyPioGit/cfc-cch/wiki/CFC%E2%80%90CCH_WIKI_MY">Bahasa Melayu (Malaysia)</a>
</p>

> **Documentation:** Full documentation is available in the language selector above.

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-20%2B-339933?style=for-the-badge&amp;logo=node.js&amp;logoColor=white" alt="Node.js 20+" />
  <img src="https://img.shields.io/badge/Baileys-7.x-25D366?style=for-the-badge" alt="Baileys 7.x" />
  <img src="https://img.shields.io/badge/JavaScript-ESM-F7DF1E?style=for-the-badge&amp;logo=javascript&amp;logoColor=black" alt="JavaScript ESM" />
  <img src="https://img.shields.io/badge/License-Personal%20Use-lightgrey?style=for-the-badge" alt="Personal Use License" />
</p>

**CFC-CCH (Create Custom History)** is a single-file library for creating and sending custom WhatsApp group-history bundles through Baileys.

It supports text, image, raw messages, and message reactions with custom senders, timestamps, and quoted messages.

> **Important:** Read [Group history requirements](#group-history-requirements) before using CFC-CCH. The target's join session and timestamps determine whether the history is shown.

## Requirements

- Node.js 20+
- `@whiskeysockets/baileys`

## Installation

Copy `cFC.js` into your project.

```js
import { CFC } from './cFC.js'
```

## Quick start

```js
const cfc = new CFC(sock)
const b = cfc.builder()

const m1 = b.addText('selamat datang', {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914860
})

const m2 = b.addText('omaga sepuh', {
  participant: '11111111@s.whatsapp.net',
  timestamp: 1792914861,
  quoted: m1
})

await b.send(m.chat, targetJid)
```

`send(groupJid, targetJid)` sends the bundle to the group and addresses the history to the target.

## Message fields

Required:

```js
participant
timestamp
```

Optional:

```js
fromMe
messageId
pushName
```

`fromMe`, `participant`, `messageId`, and `timestamp` are stored outside `message`.

## fromMe

`fromMe` belongs in `key.fromMe`.

```js
b.addText('halo', {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914860,
  fromMe: false
})
```

```js
b.addText('gw juga', {
  participant: targetJid,
  timestamp: 1792914861,
  fromMe: true
})
```

`participant` is still required when `fromMe` is `true`.

Group-history rendering may place `fromMe: true` and `fromMe: false` differently depending on the target client. Test the desired combination on the target device.

## Text

```js
const m1 = b.addText('halo', {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914860
})
```

With optional fields:

```js
const m1 = b.addText('halo', {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914860,
  fromMe: false,
  messageId: '3EB0CUSTOMID123',
  pushName: 'Nama'
})
```

## Reaction

Add a reaction to a message with `addReaction()`.

```js
const m = b.addText('halo', {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914860
})

b.addReaction(m, '👍', {
  participant: '6285899292940@s.whatsapp.net',
  timestamp: 1792914861
})
```

Reaction by message ID:

```js
b.addReaction('3EB0CUSTOMID123', '👍', {
  participant: '6285899292940@s.whatsapp.net',
  timestamp: 1792914861
})
```

Chain style:

```js
const m = b.addText('halo', {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914860
}).addReaction('👍', {
  participant: '6285899292940@s.whatsapp.net',
  timestamp: 1792914861
})
```

Reaction fields:

```js
participant  // reactor
timestamp    // reaction time, Unix seconds
fromMe       // optional, default false
messageId    // optional reaction key ID
```

The target message must already be in the builder.

## Image

The image helper accepts a local path, URL, Buffer, or prepared `imageMessage`.

### Local path

```js
const img = await b.addImage('C:/images/foto.jpg', {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914862,
  caption: 'nih fotonya'
})
```

### URL

```js
const img = await b.addImage('https://example.com/foto.jpg', {
  participant: '6285899292940@s.whatsapp.net',
  timestamp: 1792914863,
  caption: 'foto dari internet'
})
```

### Buffer

```js
const img = await b.addImage(buffer, {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914864
})
```

### Prepared imageMessage

```js
const img = await b.addImage(imageMessage, {
  participant: '0@s.whatsapp.net',
  timestamp: 1792914865
})
```

## Raw message

```js
const m1 = b.addRaw({
  participant: '0@s.whatsapp.net',
  timestamp: 1792914860,
  fromMe: false,
  messageId: '3EB0CUSTOMID123',
  pushName: 'Nama',
  message: {
    conversation: 'halo'
  }
})
```

`messageId` is optional. If omitted, CFC-CCH generates one.

A custom message ID can be used as a quote target:

```js
b.addText('balas', {
  participant: '11111111@s.whatsapp.net',
  timestamp: 1792914861,
  quoted: '3EB0CUSTOMID123'
})
```

## Quoted messages

`quoted` accepts:

### Builder message

```js
const m1 = b.addText('halo', { ... })

b.addText('balas', {
  ...,
  quoted: m1
})
```

### Builder message ID

```js
const m1 = b.addText('halo', { ... })

b.addText('balas', {
  ...,
  quoted: m1.key.id
})
```

When the ID belongs to a message in the builder, CFC-CCH resolves the participant and quoted message automatically.

### Raw WebMessageInfo

```js
b.addText('balas', {
  ...,
  quoted: {
    key: {
      remoteJid: m.chat,
      fromMe: false,
      id: '3EB0ABC...',
      participant: '0@s.whatsapp.net'
    },
    message: {
      conversation: 'halo'
    }
  }
})
```

### Raw minimal

```js
b.addText('balas', {
  ...,
  quoted: {
    id: '3EB0ABC...',
    participant: '0@s.whatsapp.net',
    message: {
      conversation: 'halo'
    }
  }
})
```

A Baileys `m.quoted` object can also be passed directly.

## Fluent usage

Builder methods can be chained.

```js
const m1 = new CFC(sock)
  .builder()
  .addText('halo', {
    participant: '0@s.whatsapp.net',
    timestamp: 1792914860
  })

await m1
  .addText('balas', {
    participant: '11111111@s.whatsapp.net',
    timestamp: 1792914861,
    quoted: m1.key.id
  })
  .send(m.chat, targetJid)
```

Available on a message object:

```js
m1.addText()
m1.addImage()
m1.addRaw()
m1.addReaction()
m1.send()
m1.builder
```

## Group history requirements

- The target must be in the group when the bundle is sent.
- Timestamps must fall in a period when the target was not in the group.
  - New member: after group creation, before joining.
  - Rejoin: during the period the target was absent.
  - Original member: no new join event, so history may not appear.
- A join session should receive one bundle.
- Before resending for the same join session, delete or revoke the previous bundle and its notice.
- If deduplication persists, the target must leave and rejoin, or a new group must be used.

## Methods

```js
const cfc = new CFC(sock)
const b = cfc.builder()

b.addText(text, options)
await b.addImage(source, options)
b.addRaw(options)
b.addReaction(target, emoji, options)

b.build()
await b.send(groupJid, targetJid)
```

Required:

```js
participant
timestamp
```

Optional:

```js
fromMe
messageId
pushName
quoted
```

## Notes

- Reactions are handled with `addReaction()`.
- Sticker helpers are not included; use `addRaw` with `stickerMessage`.
- Unofficial library. WhatsApp protocol changes may break it.

## Disclaimer

Use CFC-CCH only with accounts, groups, and recipients you own or have permission to use.

---

## License

Personal and educational use only. Resale, paid redistribution, or commercial
distribution is not permitted without prior written permission from the author.

<p align="center">Made by <a href="https://github.com/MyPioGit">popioo</a></p>
