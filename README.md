```markdown
<p align="center">
  <img src="https://files.catbox.moe/bke9ik.jpg" width="200" style="border-radius: 50%;" alt="Lyyncode Logo"/>
</p>

<h1 align="center">@lyyncode/lyyn-bail</h1>

<p align="center">
  <strong>WhatsApp Web API Library</strong><br>
  Fork kustom dari Baileys dengan optimasi untuk produksi
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@lyyncode/lyyn-bail"><img src="https://img.shields.io/npm/v/@lyyncode/lyyn-bail.svg?style=flat-square&color=CB3837" alt="npm version"/></a>
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square" alt="License"/></a>
  <img src="https://img.shields.io/badge/node-%3E%3D18.0.0-339933?style=flat-square&logo=nodedotjs" alt="Node.js"/>
</p>

---

## Tentang Package Ini

Library ini adalah hasil fork dari Baileys yang dimodifikasi untuk kebutuhan sistem WhatsApp Bot skala production. Dibuat dengan fokus pada stabilitas koneksi dan kemudahan implementasi.

## Instalasi

```bash
npm install @lyyncode/lyyn-bail
```

Penggunaan Dasar

```javascript
const { default: makeWASocket, useMultiFileAuthState } = require('@lyyncode/lyyn-bail');

async function startBot() {
  const { state, saveCreds } = await useMultiFileAuthState('session');
  
  const sock = makeWASocket({
    printQRInTerminal: true,
    auth: state,
    defaultQueryTimeoutMs: undefined
  });

  sock.ev.on('connection.update', (update) => {
    const { connection } = update;
    if (connection === 'open') console.log('Bot terhubung');
  });

  sock.ev.on('creds.update', saveCreds);
}

startBot();
```

Fitur Utama

Fitur	Keterangan	
Multi-Device	Support WhatsApp multi-device	
Auto Reconnect	Reconnect otomatis saat disconnect	
Media Support	Kirim teks, gambar, video, dokumen	
Group Management	Kelola grup dengan mudah	
Newsletter Auto-Follow	Otomatis follow channel saat konek	

Struktur Event

```javascript
// Connection status
sock.ev.on('connection.update', ({ connection, qr }) => {
  // 'connecting' | 'open' | 'close'
});

// Incoming messages
sock.ev.on('messages.upsert', ({ messages }) => {
  // Handle pesan masuk
});

// Credentials update
sock.ev.on('creds.update', saveCreds);
```

Contoh Kirim Pesan

```javascript
// Teks biasa
await sock.sendMessage(jid, { text: 'Halo' });

// Dengan mention
await sock.sendMessage(jid, { 
  text: '@user',
  mentions: ['628xx@s.whatsapp.net']
});

// Gambar
await sock.sendMessage(jid, {
  image: fs.readFileSync('foto.jpg'),
  caption: 'Keterangan'
});
```

Kontak

- Telegram: [@Lyyncode](https://t.me/Lyyncode)
- Website: [lyyncode.xyz](https://lyyncode.xyz)
- Email: support@lyyncode.xyz

Lisensi

MIT License - Lihat file [LICENSE](LICENSE) untuk detail lengkap.

------
