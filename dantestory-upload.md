---
id: dantestory-upload
title: Upload DanteStory jam 10
enabled: true
owner: goal:new-youtube-channels-dantestory-dantejr-dantekids
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 10:00:00
timeout_secs: 3600
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC8E192CE47D2F40D5B8083B65DE729E","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Upload video misteri penampakan CCTV slot harian ke channel YouTube DanteStory (format baru mulai 2026-10-07: kuntilanak, pocong, tempat angker & fenomena misterius; audience umum/dewasa, BUKAN untuk anak).

KONTEKS:
- Video: /home/hatch/workspace/dantestory/outbox/YYYY-MM-DD-dantestory-<id>.mp4 + .json (dibuat job dantestory-generate jam 04:00; tanggal = hari ini WIB)
- Info channel: /home/hatch/workspace/dantestory/CHANNEL.md

LANGKAH:
1. Baca CHANNEL.md. Kalau file TIDAK ADA → JANGAN upload ke channel lain (terutama JANGAN ke JuanChoxters atau DanteChannel). Laporkan ke chat: "Upload DanteStory hari ini dibatalkan — channel belum siap." Selesai.
2. Cari file outbox/<YYYY-MM-DD hari ini>-dantestory-*.mp4 + .json-nya. Kalau TIDAK ADA → laporkan ke chat: "Upload DanteStory hari ini dibatalkan — video belum jadi." Selesai. JANGAN upload video tanggal lain.
3. Verifikasi video dengan ffprobe (ada stream video+audio, 1920x1080).
4. Spawn browser task untuk upload via YouTube Studio (sesi Google sudah login di profil browser):
   - initial_url: https://studio.youtube.com/
   - task: "Upload video ke channel YouTube 'DanteStory'. LANGKAH: (1) WAJIB pastikan channel aktif adalah DanteStory — kalau bukan, ganti via channel switcher (ikon profil) dan VERIFIKASI nama 'DanteStory' tampil di halaman. JANGAN upload ke channel lain (JANGAN ke JuanChoxters, DanteChannel, DanteJr, atau DanteKids). Kalau channel DanteStory tidak bisa dipilih, BERHENTI dan laporkan. (2) Klik Create/Upload, upload file video yang dilampirkan (via files). (3) Isi Title, Description, Tags persis dari metadata yang diberikan. (4) Audience: pilih 'No, it's not made for kids' (channel ini untuk umum). (5) Visibility: Public. (6) Publish, lalu laporkan URL video. Kalau Google meminta verifikasi ulang (kode/ketuk HP), BERHENTI dan laporkan 'butuh verifikasi' — jangan coba cara lain."
   - files: [{"path": "<path mp4 hari ini>"}]
   - Sertakan isi file .json (title, description, tags) di dalam task.
5. Kalau sukses: catat `python3 /home/hatch/workspace/dantestory/video_log.py uploaded <YYYY-MM-DD> harian <URL video>` (otomatis sinkron dashboard) — SELESAI, jangan lapor ke chat. Kalau gagal: laporkan apa yang gagal.

JANGAN upload via API/OAuth (token yang ada milik channel JuanChoxters). JANGAN upload ke channel selain DanteStory.
