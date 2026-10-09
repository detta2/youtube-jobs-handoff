---
id: dantejr-generate
title: Generate video harian DanteJr
enabled: true
owner: goal:new-youtube-channels-dantestory-dantejr-dantekids
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 04:45:00
timeout_secs: 7200
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC66C13E140B2090F2CD441B63BA5C9E","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Generate 1 video thalassophobia baru untuk channel YouTube DanteJr (16:9, 12 adegan, ±2 menit) — POV horor laut dalam yang memicu takut laut: jurang tanpa dasar, bayangan raksasa di bawah perahu, kegelapan total, sendirian di tengah samudra. AUDIENCE UMUM/DEWASA (bukan untuk anak). Upload dilakukan job dantejr-upload jam 13:00.

KONSEP: found-footage horor laut — seolah rekaman nyata penyelam/nelayan/kapal. Setiap video: ketenangan awal → hal janggal → eskalasi dread → punch moment yang bikin merinding → akhir menggantung. Narator (Bahasa Indonesia, gaya horor-thriller pelan dan mencekam).

ATURAN KERAS:
- Bangun DREAD, bukan jumpscare murahan. Ketakutan datang dari skala, kegelapan, dan ketidak-tahuan.
- Tanpa gore/darah. Tanpa hantu (ini soal laut, bukan setan).
- Visual POV: kamera penyelam, drone bawah air, dek kapal, sonar. Wide shot; kegelapan dominan tapi subjek tetap terbaca.
- Variasi: bedakan kategori & skenario tiap hari; tanpa pengulangan topik dalam 14 hari.

LANGKAH:
1. Baca /home/hatch/workspace/dantejr/topics.json. Pilih 1 topik dengan `used` null. Kalau semua sudah dipakai, ambil yang `used` paling lama. ATURAN: tanpa pengulangan topik dalam 14 hari.
2. Buat direktori /home/hatch/workspace/dantejr/work/YYYY-MM-DD-<id-topik>/ (tanggal hari ini WIB), tulis script.json berisi 12 scenes (±2 menit, 6 adegan/menit). Tiap scene: title, narration (Bahasa Indonesia, ±90-110 karakter ≈ ±8-10 detik, gaya horor-thriller), img_prompt (Bahasa Inggris, diawali "Underwater horror found-footage, horizontal 16:9:" + deskripsi + style anchor konsisten di semua scene: "deep dark ocean, murky water, diver POV, ominous vastness, photorealistic, subtle dread, no text"). Alur 12 adegan: 1-3 = ketenangan & lokasi; 4-7 = hal janggal mulai terasa; 8-10 = eskalasi, ancaman makin dekat; 11 = punch moment; 12 = akhir menggantung + ajakan komentar.
3. Delegasikan ke SATU subagent: (a) 12 gambar via media-generation (timeout 600); (b) narasi via /opt/hatch/bin/tts --language id --voice avocado_v2:MAI_01; (c) rakit: `bash /home/hatch/workspace/dantejr/assemble_16x9.sh <workdir> /home/hatch/workspace/dantejr/outbox/YYYY-MM-DD-dantejr-<id>.mp4 12`; (d) verifikasi ffprobe (1920x1080, audio, 1,5-2,5 menit) + LIHAT 3 frame (awal/tengah/akhir); QC: nuansa horor laut konsisten, tidak kartun konyol, tidak ada cacat parah; 1x generate ulang untuk scene cacat berat.
4. Tulis metadata outbox/YYYY-MM-DD-dantejr-<id>.json: {"title": "<Judul> 😱" (maks 100 char), "description": "sinopsis 2-3 kalimat + pertanyaan mencekam + ajakan komentar + \n\n#thalassophobia #lautdalam #hororlaut #dantejr", "tags": [...], "made_for_kids": false, "visibility": "public"}.
5. Update topics.json: `used` topik terpilih = tanggal hari ini (YYYY-MM-DD).
6. Daftarkan: `python3 /home/hatch/workspace/dantejr/video_log.py register YYYY-MM-DD harian <id-topik> "<judul>"`.
7. Selesai. TIDAK PERLU lapor ke chat. JANGAN upload di job ini. Jika generate gagal total, laporkan ke chat.
