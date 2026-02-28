```markdown
# @lyynnotyourgod/lyyn-bail

Lyyn Fork of Baileys - Websocket WhatsApp API untuk Node.js.

[![npm version](https://img.shields.io/npm/v/@lyynnotyourgod/lyyn-bail.svg)](https://www.npmjs.com/package/@lyynnotyourgod/lyyn-bail)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Instalasi

```bash
npm install @lyynnotyourgod/lyyn-bail
```

Penggunaan Dasar

```javascript
const makeWASocket = require('@lyynnotyourgod/lyyn-bail');
const { useMultiFileAuthState } = require('@lyynnotyourgod/lyyn-bail');
const pino = require('pino');

async function startBot() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info');
    
    const sock = makeWASocket({
        printQRInTerminal: true,
        auth: state,
        logger: pino({ level: 'silent' })
    });

    sock.ev.on('creds.update', saveCreds);
    
    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update;
        if (connection === 'close') {
            console.log('Koneksi tertutup');
        } else if (connection === 'open') {
            console.log('Bot berhasil terhubung!');
        }
    });

    sock.ev.on('messages.upsert', async (m) => {
        const msg = m.messages[0];
        if (!msg.key.fromMe && m.type === 'notify') {
            console.log('Pesan masuk:', msg.message?.conversation);
        }
    });
}

startBot();
```

Fitur Utama

- Multi-Device Support: Full support WhatsApp Multi-Device
- WebSocket: Koneksi real-time stabil
- Media Support: Handling audio dengan music-metadata
- Timezone: Moment-timezone untuk manajemen waktu
- Caching: Node-cache & LRU-cache untuk performa
- Queue System: P-queue untuk manajemen request

Export Modules

```javascript
const { 
    makeWASocket,
    proto,
    makeInMemoryStore,
    useMultiFileAuthState,
    DisconnectReason,
    getContentType,
    downloadMediaMessage
} = require('@lyynnotyourgod/lyyn-bail');
```

Requirements

- Node.js >= 20.0.0

Dependencies

- `@skycodee/libsignal`
- `protobufjs`
- `ws`
- `pino`
- `axios`
- `music-metadata`
- `moment-timezone`
- `lru-cache`
- `p-queue`

Peer Dependencies (Optional)

- `jimp` >=0.16.0
- `link-preview-js`
- `audio-decode`

Author

LyynNotYourGod
- GitHub: [@LyynNotYourGod](https://github.com/LyynNotYourGod)

License

MIT License - Copyright (c) 2025 TsmXeuka

Acknowledgments

Forked dari [@whiskeysockets/baileys](https://github.com/whiskeysockets/baileys)
