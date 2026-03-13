```markdown
<div align="center">

# @lyyncode/lyyn-bail

[![npm version](https://img.shields.io/npm/v/@lyyncode/lyyn-bail.svg)](https://www.npmjs.com/package/@lyyncode/lyyn-bail)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js Version](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen.svg)](https://nodejs.org/)

**WhatsApp Web API Library - Baileys Fork by Lyyncode**

[Installation](#installation) • [Usage](#usage) • [Features](#features) • [API](#api)

</div>

---

## 📦 Installation

```bash
npm install @lyyncode/lyyn-bail
```

or

```bash
yarn add @lyyncode/lyyn-bail
```

---

🚀 Quick Start

```javascript
const { default: makeWASocket, DisconnectReason, useMultiFileAuthState } = require('@lyyncode/lyyn-bail');
const { Boom } = require('@hapi/boom');

async function connectToWhatsApp() {
    const { state, saveCreds } = await useMultiFileAuthState('auth_info');
    
    const sock = makeWASocket({
        printQRInTerminal: true,
        auth: state
    });

    sock.ev.on('connection.update', (update) => {
        const { connection, lastDisconnect } = update;
        
        if (connection === 'close') {
            const shouldReconnect = (lastDisconnect.error instanceof Boom)?.output?.statusCode !== DisconnectReason.loggedOut;
            console.log('Connection closed due to:', lastDisconnect.error, ', reconnecting:', shouldReconnect);
            if (shouldReconnect) connectToWhatsApp();
        } else if (connection === 'open') {
            console.log('Opened connection');
        }
    });

    sock.ev.on('messages.upsert', async ({ messages }) => {
        const m = messages[0];
        if (!m.key.fromMe) {
            console.log('New message:', m.message?.conversation || m.message?.extendedTextMessage?.text);
        }
    });

    sock.ev.on('creds.update', saveCreds);
}

connectToWhatsApp();
```

---

✨ Features

- 🔐 Multi-device support - Works with WhatsApp multi-device beta
- 📱 QR Code authentication - Easy pairing via terminal QR
- 💾 Persistent sessions - Auto-save credentials with `useMultiFileAuthState`
- 📨 Send messages - Text, media, documents, stickers
- 👥 Group management - Create, join, leave, manage groups
- 📥 Auto newsletter follow - Built-in channel subscription
- ⚡ Fast connection - Optimized WebSocket handling
- 🛡️ Anti-ban measures - Smart reconnect logic

---

📚 API Reference

Connection

Method	Description	
`makeWASocket(options)`	Create new WhatsApp socket connection	
`useMultiFileAuthState(path)`	File-based auth state management	
`useSingleFileAuthState(path)`	Single file auth (deprecated)	

Messaging

```javascript
// Send text
await sock.sendMessage(jid, { text: 'Hello!' });

// Send image
await sock.sendMessage(jid, { 
    image: fs.readFileSync('image.jpg'),
    caption: 'My image'
});

// Send document
await sock.sendMessage(jid, {
    document: fs.readFileSync('file.pdf'),
    fileName: 'document.pdf',
    mimetype: 'application/pdf'
});
```

Group Methods

```javascript
// Get group metadata
const metadata = await sock.groupMetadata(groupId);

// Send message to group
await sock.sendMessage(groupId, { text: 'Hello group!' });

// Create group
const group = await sock.groupCreate('My Group', [user1, user2]);
```

---

🔧 Configuration Options

```javascript
const sock = makeWASocket({
    printQRInTerminal: true,      // Show QR in terminal
    auth: state,                   // Auth credentials
    logger: pino({ level: 'silent' }), // Logger instance
    browser: ['Ubuntu', 'Chrome', '20.0.04'], // Browser fingerprint
    connectTimeoutMs: 60000,       // Connection timeout
    keepAliveIntervalMs: 30000,    // Keep alive ping interval
    defaultQueryTimeoutMs: 20000,  // Query timeout
    markOnlineOnConnect: true,     // Mark user online
    syncFullHistory: false,        // Sync full chat history
    shouldSyncHistoryMessage: false // Disable history sync
});
```

---

📝 Environment Variables

```bash
# Optional: Set custom browser name
BROWSER_NAME=Lyyn-Bail

# Optional: Debug mode
DEBUG=baileys
```

---

🔄 Connection Events

```javascript
sock.ev.on('connection.update', ({ connection, lastDisconnect, qr }) => {
    // connection: 'connecting' | 'open' | 'close'
    // qr: QR code data URI (when pairing)
    // lastDisconnect: Error info on close
});

sock.ev.on('messages.upsert', ({ messages, type }) => {
    // type: 'notify' | 'append'
    // messages: Array of WAMessage
});

sock.ev.on('presence.update', ({ id, presences }) => {
    // Presence updates
});
```

---

⚠️ Disclaimer

This project is not affiliated, associated, authorized, endorsed by, or in any way officially connected with WhatsApp or any of its subsidiaries or its affiliates. The official WhatsApp website can be found at https://whatsapp.com. "WhatsApp" as well as related names, marks, emblems and images are registered trademarks of their respective owners.

---

👤 Author

Lyyncode (Marsel Hidayat Aditama)
- Telegram: [@Lyyncode](https://t.me/Lyyncode)
- Website: [lyyncode.xyz](https://lyyncode.xyz)
- GitHub: [@LyynNotYourGod](https://github.com/LyynNotYourGod)

---

📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

[⬆ Back to Top](#lyyncodelyyn-bail)

Made with ❤️‍🔥 by Lyyncode
