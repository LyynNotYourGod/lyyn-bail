<div align="center">

<img src="https://files.catbox.moe/bke9ik.jpg" width="180" height="180" style="border-radius: 50%; object-fit: cover;" alt="Lyyncode"/>

# <code>@lyyncode/lyyn-bail</code>

**WhatsApp Web API Library** — *Fork optimasi dari Baileys*

[![npm](https://img.shields.io/npm/v/@lyyncode/lyyn-bail?style=for-the-badge&color=red)](https://www.npmjs.com/package/@lyyncode/lyyn-bail)
[![license](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)
[![node](https://img.shields.io/badge/node-%3E%3D20-339933?style=for-the-badge&logo=nodedotjs)](https://nodejs.org/)

</div>

---

## 📦 Install

```bash
npm i @lyyncode/lyyn-bail
```

🚀 Quick Start

```javascript
const { default: makeWASocket, useMultiFileAuthState } = require('@lyyncode/lyyn-bail');

async function init() {
  const { state, saveCreds } = await useMultiFileAuthState('session');
  
  const sock = makeWASocket({
    printQRInTerminal: true,
    auth: state
  });

  sock.ev.on('connection.update', ({ connection }) => {
    if (connection === 'open') console.log('✓ Connected');
  });

  sock.ev.on('creds.update', saveCreds);
}

init();
```

✨ Fitur

Fitur	Status	
Multi-Device	✅	
Auto Reconnect	✅	
Kirim Media	✅	
Group Management	✅

📝 Contoh Penggunaan

Kirim Pesan Teks

```javascript
await sock.sendMessage('628xx@s.whatsapp.net', { text: 'Halo' });
```

Kirim Gambar

```javascript
await sock.sendMessage(jid, {
  image: fs.readFileSync('foto.jpg'),
  caption: 'Keterangan'
});
```

🔗 Links

- Telegram: [t.me/Lyyncode](https://t.me/Lyyncode)
- Website: [lyyncode.xyz](https://lyyncode.xyz)
- NPM: [@lyyncode/lyyn-bail](https://www.npmjs.com/package/@lyyncode/lyyn-bail)

---
