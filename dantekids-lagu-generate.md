---
id: dantekids-lagu-generate
title: Generate eksperimen sains harian DanteKids
enabled: true
owner: goal:new-youtube-channels-dantestory-dantejr-dantekids
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 05:30:00
timeout_secs: 7200
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC66C13E140B2090F2CD441B63BA5C9E","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Generate 1 video ikan lucu pinggir laut baru untuk channel YouTube DanteKids (16:9, 12 adegan, ±2 menit). Setiap video menampilkan 1 ikan/hewan pantai yang kocak: ikan badut, buntal, kuda laut, kepiting, tukik, dll. Untuk ANAK (made-for-kids = YES). Upload dilakukan job dantekids-lagu-upload jam 16:00.

KONSEP: kenalan sama sahabat laut — narator (Bahasa Indonesia, ceria & sederhana untuk anak) memperkenalkan hewan, menunjukkan tingkah lucunya, menyelipkan 1 fakta seru yang mudah diingat anak. Visual: bright cheerful cartoon, warna-warni, ramah anak. Setiap video diakhiri ajakan "yuk sayangi laut!".

ATURAN KERAS (konten anak):
- Selalu ceria & aman; TIDAK BOLEH seram.
- Anatomi hewan benar; tidak ada hewan cacat.
- made-for-kids = YES selalu.

LANGKAH:
1. Baca /home/hatch/workspace/dantekids_lagu/topics.json. Pilih 1 topik dengan `used` null; hindari kategori yang dipakai 2 hari terakhir (lihat `used` terbaru). Kalau semua sudah dipakai, ambil yang `used` paling lama. ATURAN: tanpa pengulangan topik dalam 14 hari.
2. Buat direktori /home/hatch/workspace/dantekids_lagu/work/YYYY-MM-DD-<id-topik>/ (tanggal hari ini WIB), tulis script.json berisi 12 scenes (±2 menit, 6 adegan/menit). Tiap scene: title, narration (Bahasa Indonesia, ±90-110 karakter agar ±8-10 detik per adegan, ceria & sederhana untuk anak), img_prompt (Bahasa Inggris, diawali "Bright cheerful children's cartoon, horizontal 16:9:" + deskripsi adegan + style anchor KONSISTEN untuk seluruh video: "cute cartoon sea animals, vibrant coral reef, sunny shallow water, kid-friendly, anatomically correct animals, no text"). Alur 12 adegan: 1-2 = kenalan sama hewannya; 3-8 = tingkah lucu; 9-10 = momen paling kocak; 11 = fakta seru 1 kalimat; 12 = ajakan sayangi laut.
3. Delegasikan ke SATU subagent: (a) 12 gambar: `media-generation --media-subagent-output-type image --conversation-json '[{"text":"<img_prompt>"}]' --output-dir <workdir>/img --timeout-secs 600`; (b) narasi: `/opt/hatch/bin/tts speak --language id --voice avocado_v2:MAI_01 --text "<narration>" --output <workdir>/audio/nar-NN.mp3` (pakai --text-stdin bila ada tanda kutip); (c) rakit video 16:9 dari gambar + audio dengan ffmpeg (zoompan halus per gambar, concat, 1920x1080) → /home/hatch/workspace/dantekids_lagu/outbox/YYYY-MM-DD-dantekids-<id-topik>.mp4; (d) verifikasi: ffprobe (1920x1080, ada audio, durasi 1,5-2,5 menit), extract 3 frame (awal/tengah/akhir) dan LIHAT — gaya konsisten, tidak ada cacat parah; 1x generate ulang untuk scene cacat berat.
4. Tulis metadata outbox/YYYY-MM-DD-dantekids-<id-topik>.json: {"title": "<Judul> 🐠" (maks 100 char), "description": "Kenalan sama <hewan>! <sinopsis 2 kalimat>.\n\n#ikanlucu #hewanlaut #anaksenang #dantekids", "tags": [...], "made_for_kids": true, "visibility": "public"}.
5. Update topics.json: set `used` topik = tanggal hari ini.
6. Daftarkan: `python3 /home/hatch/workspace/dantekids_lagu/video_log.py register YYYY-MM-DD harian <id-topik> "<judul>"` (otomatis sinkron dashboard).
7. Selesai. TIDAK PERLU lapor ke chat. JANGAN upload di job ini. Jika generate gagal total, laporkan ke chat (satu-satunya kondisi yang perlu dilaporkan).
