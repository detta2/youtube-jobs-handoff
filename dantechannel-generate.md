---
id: dantechannel-generate
title: Generate video dongeng harian DanteChannel
enabled: true
owner: goal:video-dongeng-harian-dantechannel
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 05:30:00
delivery:
  - chat_id: d8481642-b644-44ae-8756-1825407cb31a
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC344F862E802269F4BDBB6E7390B018","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Generate 2 video dongeng anak baru untuk channel YouTube DanteChannel — 1 untuk slot PAGI (upload 07:00) dan 1 untuk slot MALAM (upload 19:00 oleh job dantechannel-upload-malam). Durasi tiap video ±4 MENIT (24 adegan).

LANGKAH:
1. Baca /home/hatch/workspace/dantechannel/stories.json — pilih 2 cerita berbeda dengan `used` null (belum pernah dipakai). Kalau kurang dari 2 yang null, ambil yang `used` paling lama. Satu untuk pagi, satu untuk malam. Catat pilihan.
2. Untuk TIAP slot (pagi, malam): buat direktori /home/hatch/workspace/dantechannel/work/YYYY-MM-DD-<slot>-<id-cerita>/ (tanggal hari ini WIB) dan tulis script.json berisi 24 scenes (±4 menit, 6 adegan/menit). Tiap scene: n, title, narration (Bahasa Indonesia, bahasa sederhana untuk anak, ±110-130 karakter agar pas ±8-10 detik), image_prompt (Bahasa Inggris + deskripsi karakter KONSISTEN di semua scene + ANATOMI BENAR: sebutkan jumlah kaki/tangan yang tepat untuk tiap hewan, mis. "turtle with exactly four legs", "rabbit with two arms and two legs" + style anchor: "Cheerful children's storybook 3D animation style, soft pastel colors, bright sunny lighting, Pixar-like render, cute and friendly, anatomically correct animals, no text"), anim_prompt (deskripsi gerakan). Kembangkan cerita dengan alur lengkap: pembuka → konflik → klimaks → penyelesaian → pesan moral (scene 24). Karakter dan gaya visual harus konsisten di semua 24 adegan. ATURAN ANATOMI (penonton anak kecil — karakter cacat DILARANG TAYANG): tiap hewan harus punya jumlah kaki/tangan yang benar dan natural (kura-kura = tepat 4 kaki, kelinci = 2 tangan + 2 kaki, dst.); tidak boleh ada kaki/tangan ekstra, tidak boleh anggota badan ganda atau menyatu secara aneh.
3. Delegasikan generasi ke subagent (spawn SATU subagent untuk kedua video, boleh paralel internal):
   - 24 gambar per video: `media-generation --media-subagent-output-type image --conversation-json '[{"text":"<image_prompt>"}]' --output-dir <scene dir> --timeout-secs 600`
   - Animasi: upload `media-generation --upload-only --image-file <gambar>` dapat id, lalu `media-generation --media-subagent-output-type video --conversation-json '[{"image":"<id>"},{"text":"<anim_prompt>"}]' --output-dir <scene dir> --timeout-secs 900`
   - Narasi: `tts speak --language id --voice avocado_v2:MAI_01 --text "<narration>" --output <scene dir>/nar.mp3` (pakai --text-stdin bila ada tanda kutip)
   - Gabung per scene: ffmpeg klip + narasi → done.mp4 (tpad clone frame terakhir bila narasi lebih panjang; -shortest; re-encode 1280x720 libx264 yuv420p + aac)
   - Concat 24 done.mp4 → /home/hatch/workspace/dantechannel/outbox/YYYY-MM-DD-pagi-<id>.mp4 dan YYYY-MM-DD-malam-<id>.mp4
   - Verifikasi tiap video: ffprobe (1280x720, ada audio, durasi ±3,5-4,5 menit), extract 3 frame (awal/tengah/akhir) dan LIHAT — gaya ceria, karakter konsisten, tanpa cacat parah; 1x generate ulang untuk scene cacat berat. QC ANATOMI WAJIB: di tiap frame yang dilihat, hitung kaki/tangan tiap karakter (kura-kura harus tepat 4 kaki, kelinci 2 tangan + 2 kaki, dst.); scene dengan anggota badan ekstra/ganda/menyatu aneh = cacat berat → generate ulang gambarnya.
4. Tulis metadata tiap slot: outbox/YYYY-MM-DD-<slot>-<id>.json = {"title": "<Judul> | Dongeng Anak Bergambar" (maks 100 char), "description": "sinopsis 2-3 kalimat + pesan moral + \n\n#dongenganak #<tag> #dongengbergambar #ceritaanak", "tags": [...], "made_for_kids": true, "visibility": "public"}.
5. Update stories.json: set `used` kedua cerita = tanggal hari ini (YYYY-MM-DD).
6. Daftarkan ke dashboard: `python3 /home/hatch/workspace/dantechannel/video_log.py register YYYY-MM-DD pagi <id-cerita-pagi> "<judul pagi>"` dan `python3 /home/hatch/workspace/dantechannel/video_log.py register YYYY-MM-DD malam <id-cerita-malam> "<judul malam>"` (otomatis sinkron dante.json ke dashboard yt-studio).
7. Selesai. TIDAK PERLU lapor ke chat — user tidak mau preview; video langsung dipakai job upload.

JANGAN upload di job ini. Jika generate gagal total, laporkan ke chat (satu-satunya kondisi yang perlu dilaporkan).
