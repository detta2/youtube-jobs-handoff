---
id: dantechannel-shorts-siang
title: Shorts siang DanteKids jam 12
enabled: true
owner: goal:video-dongeng-harian-dantechannel
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 12:00:00
delivery:
  - chat_id: d8481642-b644-44ae-8756-1825407cb31a
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC99F0D99B1F3C6102F9ADD7D285CD4E","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Buat & upload YouTube Short BARU untuk channel DanteChannel (kejar subscriber).

ATURAN PENTING (user 2026-10-03): Shorts harus video BARU yang digenerate dari nol — JANGAN potong/clip dari video panjang yang sudah diupload.

KONTEKS:
- Info channel: /home/hatch/workspace/dantechannel/CHANNEL.md

LANGKAH:
1. Baca CHANNEL.md.
2. Tulis mini-dongeng ORIGINAL baru (jangan pakai ulang cerita yang sudah jadi video panjang): fabel hewan sederhana untuk anak dengan 1 pesan moral yang jelas. Buat direktori /home/hatch/workspace/dantechannel/work/YYYY-MM-DD-shorts-siang-<id>/ (tanggal hari ini WIB) dan tulis script.json berisi 6 scenes (±50 detik total, ±8-9 detik/scene). Tiap scene: n, title, narration (Bahasa Indonesia, bahasa sederhana untuk anak, ±110-130 karakter), image_prompt (Bahasa Inggris + "vertical 9:16 portrait composition" + deskripsi karakter KONSISTEN di semua scene + ANATOMI BENAR: sebutkan jumlah kaki/tangan yang tepat untuk tiap hewan, mis. "turtle with exactly four legs", "rabbit with two arms and two legs" + style anchor: "Cheerful children's storybook 3D animation style, soft pastel colors, bright sunny lighting, Pixar-like render, cute and friendly, anatomically correct animals, no text"), anim_prompt (deskripsi gerakan). Alur: pembuka → konflik kecil → klimaks → penyelesaian → pesan moral (scene 6). ATURAN ANATOMI (penonton anak kecil — karakter cacat DILARANG TAYANG): tiap hewan harus punya jumlah kaki/tangan yang benar dan natural; tidak boleh ada kaki/tangan ekstra, ganda, atau menyatu secara aneh.
3. Delegasikan generasi ke subagent:
   - 6 gambar: `media-generation --media-subagent-output-type image --conversation-json '[{"text":"<image_prompt>"}]' --output-dir <scene dir> --timeout-secs 600`
   - Animasi: upload `media-generation --upload-only --image-file <gambar>` dapat id, lalu `media-generation --media-subagent-output-type video --conversation-json '[{"image":"<id>"},{"text":"<anim_prompt>"}]' --output-dir <scene dir> --timeout-secs 900`
   - Narasi: `tts speak --language id --voice avocado_v2:MAI_01 --text "<narration>" --output <scene dir>/nar.mp3` (pakai --text-stdin bila ada tanda kutip)
   - Gabung per scene: ffmpeg klip + narasi → done.mp4 (tpad clone frame terakhir bila narasi lebih panjang; -shortest; re-encode libx264 yuv420p + aac), lalu pastikan vertikal 720x1280 (crop/scale bila perlu)
   - Concat 6 done.mp4 → /home/hatch/workspace/dantechannel/outbox/YYYY-MM-DD-shorts-siang-<id>.mp4
   - Verifikasi: ffprobe 720x1280, durasi 40-60 detik (syarat Shorts: <=60 detik), ada audio. QC ANATOMI WAJIB: extract frame tiap ~5 detik dan LIHAT semuanya — hitung kaki/tangan tiap karakter; scene dengan anggota badan ekstra/ganda/menyatu aneh = cacat berat → generate ulang gambarnya (maks 1x per scene). Kalau tidak ada hasil bersih sama sekali → laporkan ke chat "Shorts siang dibatalkan — karakter cacat." dan Selesai (jangan upload yang cacat).
4. Judul: momen paling seru + " #shorts" (maks 100 char). Deskripsi: 1-2 kalimat sinopsis + pesan moral + "\n\n#shorts #dongenganak". Tulis metadata outbox/YYYY-MM-DD-shorts-siang-<id>.json = {"title": ..., "description": ..., "tags": [...], "made_for_kids": true, "visibility": "public"}.
5. Daftarkan ke dashboard: python3 /home/hatch/workspace/dantechannel/video_log.py register <YYYY-MM-DD> shorts-siang <id> "<judul>"
6. Spawn browser task upload via YouTube Studio (sesi Google sudah login):
   - initial_url: https://studio.youtube.com/
   - task: "Upload SHORTS ke channel YouTube 'DanteChannel'. LANGKAH: (1) Pastikan channel aktif DanteChannel (ganti via channel switcher bila perlu). JANGAN upload ke channel lain. (2) Upload file shorts yang dilampirkan (via files). (3) Isi Title & Description dari metadata. (4) Audience: 'Yes, it's made for kids'. (5) Visibility: Public. (6) Publish, laporkan URL. Video vertikal <=60 dtk otomatis jadi Shorts. Kalau Google minta verifikasi ulang, BERHENTI dan laporkan 'butuh verifikasi'."
   - files: [{"path": "<path shorts mp4>"}]
7. Sukses: python3 /home/hatch/workspace/dantechannel/video_log.py uploaded <YYYY-MM-DD> shorts-siang <URL> — SELESAI, jangan lapor ke chat (aturan user 2026-10-04: "gak usah laporan", yang penting dashboard ter-update). Gagal: laporkan.

JANGAN upload via API. JANGAN ke channel selain DanteChannel.
