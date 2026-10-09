---
id: yt-studio-upload
title: Upload video ke YouTube jam 19:00
enabled: true
owner: goal:youtube-shorts-pov-cctv-channel
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 19:00:00
delivery:
  - chat_id: d8481642-b644-44ae-8756-1825407cb31a
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC99F0D99B1F3C6102F9ADD7D285CD4E","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Upload video CCTV harian ke YouTube channel JuanChoxters jam 19:00 WIB (slot malam).

LANGKAH:
1. Cari di /home/hatch/workspace/yt-studio/backend/outbox/ file YYYY-MM-DD-cctv.mp4 + YYYY-MM-DD-cctv.json untuk TANGGAL HARI INI (WIB).
   - Kalau tidak ada: laporkan "tidak ada video CCTV untuk hari ini, upload dibatalkan" dan STOP.
2. Baca metadata JSON (title, description, tags).
3. Upload langsung PUBLIC (--public). Tidak perlu menunggu approval (aturan user 2026-10-03: "langsung aja gak usah nungguin").
4. Jalankan: python3 /home/hatch/workspace/yt-studio/backend/upload.py <mp4> --title "<title>" --desc "<description>" --tags "<tags koma>" --public
5. Jika sukses: pindahkan mp4+json ke /home/hatch/workspace/yt-studio/backend/uploaded/, lalu jalankan python3 /home/hatch/workspace/yt-studio/backend/sync.py.
6. SUKSES = diam, jangan lapor ke chat (aturan user 2026-10-04: "gak usah laporan"). Yang penting dashboard ter-update — sync.py di langkah 5 sudah menanganinya.

Jika upload gagal, JANGAN retry lebih dari 2x — laporkan errornya. Jika gagal karena auth, laporkan bahwa koneksi YouTube perlu dihubungkan ulang.
