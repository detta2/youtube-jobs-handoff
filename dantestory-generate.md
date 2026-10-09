---
id: dantestory-generate
title: Generate video harian DanteStory
enabled: true
owner: goal:new-youtube-channels-dantestory-dantejr-dantekids
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 04:00:00
timeout_secs: 7200
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC567AF3B2594F43417353D703139CE9","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Generate 1 video misteri makhluk bawah laut baru untuk channel YouTube DanteStory (16:9, 18 adegan, ±3 menit) — dokumenter misteri tentang kraken, megalodon, ikan laut dalam yang aneh, kriptid & legenda naga laut. AUDIENCE UMUM/DEWASA (bukan untuk anak). Upload dilakukan job dantestory-upload jam 10:00.

KONSEP: gaya dokumenter misteri ala Discovery/Netflix — fakta sains + legenda + pertanyaan yang bikin penasaran. Setiap video: fakta pembuka yang mengejutkan → bukti & penemuan → sisi misterius/legenda → punch moment (fakta paling mind-blowing) → penutup ("masih banyak misteri di kedalaman"). Narator (Bahasa Indonesia, gaya dokumenter misteri yang membangun rasa takjub & penasaran).

ATURAN KERAS:
- Fakta harus akurat untuk makhluk nyata (ukuran, habitat, kedalaman); legenda/kriptid disajikan sebagai legenda — jangan klaim sebagai fakta.
- Visual sinematik laut dalam (deep sea, bioluminescence, kapal selam, sonar) — BUKAN gaya CCTV.
- Tanpa gore/darah berlebihan; tanpa jumpscare.
- Variasi: bedakan kategori & makhluk tiap hari; tanpa pengulangan topik dalam 14 hari.

LANGKAH:
1. Baca /home/hatch/workspace/dantestory/topics.json. Pilih 1 topik dengan `used` null; hindari kategori yang dipakai 2 hari terakhir (lihat `used` terbaru). Kalau semua sudah dipakai, ambil yang `used` paling lama. ATURAN: tanpa pengulangan topik dalam 14 hari.
2. Buat direktori /home/hatch/workspace/dantestory/work/YYYY-MM-DD-<id-topik>/ (tanggal hari ini WIB), tulis script.json berisi 18 scenes (±3 menit, 6 adegan/menit). Tiap scene: title, narration (Bahasa Indonesia, ±90-110 karakter agar ±8-10 detik per adegan, gaya dokumenter misteri), img_prompt (Bahasa Inggris, diawali "Cinematic deep sea documentary footage, horizontal 16:9:" + deskripsi adegan + style anchor KONSISTEN untuk seluruh video: "deep ocean, bioluminescent glow, dark abyss, photorealistic marine life, documentary cinematography, mysterious atmosphere, no text"). Alur 18 adegan: 1-4 = fakta pembuka & habitat; 5-10 = bukti, penemuan, perilaku aneh; 11-15 = sisi misterius/legenda; 16-17 = punch moment (fakta paling mengejutkan); 18 = penutup + ajakan komentar.
3. Delegasikan ke SATU subagent: (a) 18 gambar: `media-generation --media-subagent-output-type image --conversation-json '[{"text":"<img_prompt>"}]' --output-dir <workdir>/img --timeout-secs 600`; (b) narasi: `/opt/hatch/bin/tts speak --language id --voice avocado_v2:MAI_01 --text "<narration>" --output <workdir>/audio/nar-NN.mp3` (pakai --text-stdin bila ada tanda kutip); (c) rakit: `bash /home/hatch/workspace/dantestory/assemble_16x9.sh <workdir> /home/hatch/workspace/dantestory/outbox/YYYY-MM-DD-dantestory-<id>.mp4 18`; (d) verifikasi: ffprobe (1920x1080, ada audio, durasi 2,5-3,5 menit), extract 3 frame (awal/tengah/akhir) dan LIHAT — gaya dokumenter laut dalam konsisten, makhluk terlihat meyakinkan (bukan kartun konyol), tidak ada cacat parah; 1x generate ulang untuk scene cacat berat.
4. Tulis metadata outbox/YYYY-MM-DD-dantestory-<id>.json: {"title": "<Fakta mengejutkan>! 😱" (maks 100 char), "description": "sinopsis 2-3 kalimat + fakta mind-blowing + ajakan komentar + \n\n#misterilaut #makhluklaut #deepsea #dantestory", "tags": [...], "made_for_kids": false, "visibility": "public"}.
5. Update topics.json: set `used` topik = tanggal hari ini.
6. Daftarkan: `python3 /home/hatch/workspace/dantestory/video_log.py register YYYY-MM-DD harian <id-topik> "<judul>"` (otomatis sinkron dashboard).
7. Selesai. TIDAK PERLU lapor ke chat. JANGAN upload di job ini. Jika generate gagal total, laporkan ke chat (satu-satunya kondisi yang perlu dilaporkan).
