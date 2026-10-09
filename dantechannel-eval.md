---
id: dantechannel-eval
title: Evaluasi harian DanteKids jam 10 malam
enabled: true
owner: goal:video-dongeng-harian-dantechannel
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 22:00:00
delivery:
  - chat_id: d8481642-b644-44ae-8756-1825407cb31a
metadata:
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC6FC8131D6CED8989571FE0FE425857","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Evaluasi harian performa channel YouTube DanteChannel + cari solusi bila ada peningkatan/penurunan.

KONTEKS:
- Channel: DanteChannel (@dantekidschannel) — baca /home/hatch/workspace/dantechannel/CHANNEL.md
- Arsip statistik: /home/hatch/workspace/dantechannel/stats.json (format: {"days": [{"date":"YYYY-MM-DD","subs":N,"views_7d":N,"views_24h":N,"watch_hours":F,"videos":[{"slot":"pagi","title":"...","views":N}]}]})
- API YouTube TIDAK bisa dipakai (token milik channel lain) — ambil data lewat browser.

LANGKAH:
1. Spawn browser task READ-ONLY (jangan klik upload/ubah/publish apa pun):
   - initial_url: https://studio.youtube.com/
   - task: "Ambil data analitik channel YouTube 'DanteChannel' (READ-ONLY, jangan mengubah apa pun). LANGKAH: (1) Pastikan channel aktif DanteChannel via channel switcher. (2) Buka Analytics: catat Subscribers total, Views 7 hari terakhir, Views 24 jam terakhir, Watch time (jam) 7 hari terakhir. (3) Buka Content → Videos: catat views tiap video (judul + views). Laporkan semua angka."
2. Hitung tanggal hari ini WIB: TZ=Asia/Jakarta date +%F. Tambahkan/update entri hari ini di stats.json (buat file bila belum ada).
3. Bandingkan dengan entri kemarin (bila ada): delta subscriber, delta views_7d, video mana yang naik/turun.
4. Tulis evaluasi ke /home/hatch/workspace/dantechannel/eval/YYYY-MM-DD.md: angka hari ini, perbandingan vs kemarin, video terbaik/terburuk, hipotesis penyebab (jam upload, judul, pilihan cerita, shorts vs long), dan SOLUSI KONKRET untuk besok (mis. geser jam, ubah gaya judul, prioritaskan cerita tertentu).
5. Sinkron dashboard: python3 /home/hatch/workspace/dantechannel/video_log.py sync
6. Laporkan ke chat (ringkas, Bahasa Indonesia): subscriber & views hari ini, naik/turun vs kemarin, 1-2 solusi yang akan diterapkan. Kalau data analitik tidak bisa diambil (mis. butuh verifikasi), laporkan apa adanya — jangan mengarang angka.

JANGAN mengubah apa pun di channel. JANGAN mengarang angka statistik.
