---
id: dantechannel-upload-malam
title: Upload malam DanteChannel jam 7
enabled: true
owner: goal:video-dongeng-harian-dantechannel
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
Upload video dongeng anak slot MALAM ke channel YouTube DanteChannel.

KONTEKS:
- Video: /home/hatch/workspace/dantechannel/outbox/YYYY-MM-DD-malam-<id-cerita>.mp4 + .json (dibuat job dantechannel-generate jam 05:30; tanggal = hari ini WIB)
- Info channel: /home/hatch/workspace/dantechannel/CHANNEL.md

LANGKAH:
1. Baca CHANNEL.md. Kalau file TIDAK ADA atau channel DanteChannel belum jadi → JANGAN upload ke channel lain (terutama JANGAN ke JuanChoxters). Laporkan ke chat: "Upload malam ini dibatalkan — channel DanteChannel belum siap." Selesai.
2. Cari file outbox/<YYYY-MM-DD hari ini>-malam-*.mp4 + .json-nya. Kalau TIDAK ADA → laporkan ke chat: "Upload malam ini dibatalkan — video belum jadi." Selesai. JANGAN upload video slot/tanggal lain.
3. Verifikasi video dengan ffprobe (ada stream video+audio).
4. Spawn browser task untuk upload via YouTube Studio (sesi Google sudah login di profil browser):
   - initial_url: https://studio.youtube.com/
   - task: "Upload video ke channel YouTube 'DanteChannel'. LANGKAH: (1) Pastikan channel aktif adalah DanteChannel — kalau bukan, ganti via channel switcher (ikon profil). JANGAN upload ke channel lain. (2) Klik Create/Upload, upload file video yang dilampirkan (via files). (3) Isi Title, Description, Tags dari metadata yang diberikan. (4) Audience: pilih 'Yes, it's made for kids'. (5) Visibility: Public. (6) Publish, lalu laporkan URL video. Kalau Google meminta verifikasi ulang (kode/ketuk HP), BERHENTI dan laporkan 'butuh verifikasi' — jangan coba cara lain."
   - files: [{"path": "<path mp4 malam hari ini>"}]
   - Sertakan isi file .json (title, description, tags) di dalam task.
5. Kalau sukses: catat ke dashboard `python3 /home/hatch/workspace/dantechannel/video_log.py uploaded <YYYY-MM-DD> malam <URL video>` (otomatis sinkron dante.json) — SELESAI, jangan lapor ke chat (aturan user 2026-10-04: "gak usah laporan", yang penting dashboard ter-update). Kalau gagal: laporkan apa yang gagal.

JANGAN upload via API/OAuth (token yang ada milik channel JuanChoxters). JANGAN upload ke channel selain DanteChannel.
