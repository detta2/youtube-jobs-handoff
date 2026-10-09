---
id: yt-studio-stats-sync
title: Sinkronisasi statistik YouTube harian
enabled: true
owner: goal:youtube-shorts-pov-cctv-channel
mode: task
schedule:
  kind: daily
  timezone: Asia/Jakarta
  time: 08:16:00
delivery:
  - chat_id: d8481642-b644-44ae-8756-1825407cb31a
metadata:
  tags: [cron:flexible-time]
  originating_chat_context_json: '{"chat_id":"d8481642-b644-44ae-8756-1825407cb31a","binding_epoch":1,"origin_provider":"whatsapp","original_reply_target_id":"whatsapp_channel","chat_kind":"direct","message_id":"AC0093CA8F4EDF2BC7EFF16119271326","conversation_id":"whatsapp_channel","delivery_target_id":"whatsapp_channel","event_kind":"message","require_mention":false}'
  presentation_locale: id-ID
---
Sinkronisasi statistik YouTube ke dashboard yt-studio (2 skrip).

1. `python3 ~/workspace/yt-studio/backend/sync.py` — channel JuanChoxters: refresh OAuth token bila perlu → info channel + 10 video terbaru + statistiknya → tulis `live.json` → commit & push ke repo detta2/yt-studio (GitHub Pages).
2. `python3 ~/workspace/yt-studio/backend/monitor_sync.py` — portofolio multi-channel (Monitor Studio Pro): untuk tiap channel bertanda oauth di `backend/portfolio.json`, tarik subs/views/jumlah video via YouTube Data API + jam tayang per rentang via YouTube Analytics API (bila scope yt-analytics.readonly sudah diotorisasi; kalau belum, nilai jam manual dipertahankan) → tulis `portfolio.json` → commit & push.

Verifikasi: pastikan exit code 0 dan output berisi "pushed" atau "no changes". Jika gagal karena auth (token expired/revoked), JANGAN retry berulang — laporkan ke user bahwa koneksi YouTube perlu dihubungkan ulang via dashboard.

Ini sinkronisasi data diam-diam (silent background sync). Tidak perlu laporan chat rutin — hanya laporkan jika gagal dan butuh tindakan user.
