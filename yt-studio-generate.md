---
id: yt-studio-generate
title: Generate video CCTV harian
enabled: true
owner: goal:youtube-shorts-pov-cctv-channel
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 09:16:00
delivery:
  - chat_id: d8481642-b644-44ae-8756-1825407cb31a
metadata:
  tags: [cron:flexible-time]
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"ACCBE4E7B807E8724E77CB3A3936B40C","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Generate 3 video harian untuk channel YouTube JuanChoxters (niche: POV CCTV / camera trap Shorts) — 1 CCTV + 1 camera trap + 1 kamera bawah laut.

LANGKAH:
1. Baca /home/hatch/workspace/yt-studio/next-video.json
   - Kalau `idea` terisi: pakai untuk video CCTV. Setelah dipakai, reset file jadi {"idea":"","set_by":"","theme":"acak","updated_at":""}, lalu git commit + push di /home/hatch/workspace/yt-studio.
   - Kalau kosong: tentukan sendiri ketiga ide di bawah.
2. VARIASI WAJIB: baca SEMUA .json di outbox/ dan uploaded/ (7 hari terakhir), catat hewan + aksi yang sudah dipakai. Ketiga video hari ini HARUS beda kombinasi hewan+aksinya satu sama lain DAN beda dari 7 hari terakhir. INTI KONTEN: kelakuan hewan natural dari POV kamera.
   - VIDEO 1 (CCTV): hewan BIASA → 🎥 CCTV rumah KHAS INDONESIA (teras, genteng tanah liat; pagar HANYA jika skenario butuh — kalau ada pagar WAJIB bercelah bukaan LEBAR, hewan HANYA lewat celah itu). Contoh: kucing lari-larian di halaman, musang kejar ayam, burung hinggap lalu terbang kaget, anjing gonggong ke kamera, kucing berantem rebutan makan, ayam dikejar kucing.
   - VIDEO 2 (CAMTRAP): hewan hutan NON-BERBAHAYA → 📷 CAMERA TRAP di hutan (infrared night vision, timestamp + label CAM, motion-triggered). Contoh: rusa kena camera trap, monyet bergerombol di tepi hutan, babi hutan melintas. ATAU burung gagak di rumah luar negeri tanpa pagar.
   - VIDEO 3 (UNDERWATER HORROR): untuk penderita THALASSOPHOBIA → 🌊 KAMERA BAWAH LAUT HOROR. Target: bikin merinding. Trigger: kegelapan total di bawah, bayangan raksasa melintas, sesuatu mendekat ke kamera lalu menghilang, mata menyala di kejauhan, siluet raksasa di air keruh, perahu/kapal dari bawah air terlihat kecil dan rentan. Makhluk: cumi raksasa, anglerfish, hiu besar, paus dari bawah, ubur-ubur raksasa. GAYA WAJIB REALISTIS: seperti rekaman ROV/ekspedisi laut dalam BETULAN — grainy, low-light noise, marine snow, spotlight redup. JANGAN gaya 3D/CGI kartun. DREAD dulu, punch moment = penampakan paling bikin merinding di detik akhir. TANPA gore/darah, TANPA jumpscare murahan.
   - DILARANG: ular, dan hewan buas/berbahaya realistis di darat (harimau, beruang madu, buaya) — rawan bikin panik publik.
   - Misteri (pocong, kuntilanak, bayangan hitam, kursi bergeser) boleh untuk slot CCTV, tetap rotasi.
3. Generate 3x via media.generate_video. Tiap prompt WAJIB: vertikal 9:16, timestamp overlay TAHUN 2026 + TANGGAL ACAK (JANGAN tanggal hari ini, JANGAN 2024!), durasi ~10 detik, SUDUT KAMERA AGAK JAUH (wide shot), objek natural TANPA clipping, MUKA/HEWAN jangan close-up, WAJIB ADA MOMEN PUNCH YANG NATURAL (terlihat tidak disengaja seperti tersenggol — JANGAN seperti dipukul sengaja), LOGIKA BENDA KONSISTEN (barang UTUH sebelum jatuh; retak HANYA setelah jatuh). Untuk VIDEO 3 prompt WAJIB diawali "Real deep sea ROV expedition footage, actual underwater camera:" + "heavy grain, low-light noise, floating marine snow particles, dim ROV spotlight, found footage, not CGI, not 3D render, authentic deep ocean, thalassophobia dread".
   - output_dir="/home/hatch/workspace/yt-studio/backend/outbox", name="YYYY-MM-DD-cctv-<subjek>", "YYYY-MM-DD-camtrap-<subjek>", dan "YYYY-MM-DD-underwater-<subjek>" (tanggal hari ini WIB).
4. Verifikasi tiap video: ffprobe 720x1280 & ~10 dtk. Extract 2 frame (ffmpeg -ss 3 dan -ss 7), LIHAT: tahun 2026 tanggal acak, wide, setting sesuai hewan, pagar (kalau ada) bercelah lebar, logika benda konsisten, tidak ada cacat. Untuk VIDEO 3: pastikan terlihat seperti rekaman asli (grainy, gelap) BUKAN 3D/CGI; kalau kelihatan kartun → generate ulang SEKALI dengan penekanan "real footage". Cacat → generate ulang SEKALI. Masih cacat → laporkan, jangan upload.
5. Tulis metadata: /home/hatch/workspace/yt-studio/backend/outbox/YYYY-MM-DD-cctv.json, YYYY-MM-DD-camtrap.json, dan YYYY-MM-DD-underwater.json — {"title": ("KETANGKEP CCTV! ..." atau "KETANGKEP CAMERA TRAP! ..." atau "KETANGKEP KAMERA BAWAH LAUT! ...") + " 😱 #shorts", maks 100 char), "description": 1-2 kalimat + "\n\n#shorts #cctv #<subjek>", "tags": ["shorts","cctv","<subjek>"]}.
6. Selesai — TIDAK PERLU lapor ke chat (operasi silent sesuai perintah detta: generate & upload diam-diam, lapor hanya yang tayang/gagal). Video menunggu upload otomatis jam 12:00 (camtrap), 21:00 (underwater) & 19:00 (CCTV).

JANGAN upload ke YouTube di job ini. Jika generate gagal total, laporkan ke chat (satu-satunya kondisi yang perlu dilaporkan).
