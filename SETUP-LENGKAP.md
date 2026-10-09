# Panduan Full Setup YouTube Jobs di muse1

> Untuk dibaca muse1 dari awal sampai akhir sebelum mulai kerja.

## 0. Gambaran

Kamu mengambil alih 17 cron job pipeline YouTube dari Dante (akun Muse utama detta).
5 channel: JuanChoxters, DanteChannel, DanteStory, DanteJr, DanteKids.

**Aturan tetap detta (JANGAN dilanggar):**
1. Sukses = DIAM. Jangan lapor ke chat kalau generate/upload berhasil.
2. Lapor HANYA yang tayang atau yang gagal.
3. Setiap upload WAJIB ada hashtag.
4. Jika Google/YouTube minta verifikasi ulang saat upload → STOP job, lapor ke chat, jangan coba cara lain.
5. JANGAN upload via API untuk channel selain JuanChoxters (token API cuma milik JuanChoxters).
6. JANGAN upload ke channel yang salah — selalu verifikasi channel aktif di YouTube Studio sebelum upload.

## 1. Siapkan workspace files

Clone/copy direktori berikut dari VM Dante ke `~/workspace/` di VM kamu:
- `yt-studio/` — pipeline JuanChoxters (backend/, outbox/, uploaded/)
- `dantechannel/` — pipeline DanteChannel (work/, outbox/, stories.json, video_log.py, CHANNEL.md)
- `dantestory/` — pipeline DanteStory (work/, outbox/, topics.json, CHANNEL.md)
- `dantejr/` — pipeline DanteJr (work/, outbox/, topics.json, CHANNEL.md)
- `dantekids_lagu/` — pipeline DanteKids (work/, outbox/, topics.json, CHANNEL.md)

Script penting di tiap direktori: `video_log.py` (dashboard), `assemble_*.sh` (rakit video).

## 2. Setup YouTube OAuth (untuk JuanChoxters API upload)

1. Buat kredensial OAuth Desktop App di Google Cloud Console (project milik detta).
2. Jalankan flow OAuth untuk channel JuanChoxters, simpan token.
3. Script upload: `~/workspace/yt-studio/backend/upload.py`

## 3. Login browser YouTube Studio (untuk DanteChannel, DanteStory, DanteJr, DanteKids)

1. Buka `https://studio.youtube.com/` di browser.
2. Login dengan akun Google detta.
3. Ganti channel ke masing-masing channel via channel switcher, pastikan sesi tersimpan.
4. Upload untuk 4 channel ini SELALU via browser, JANGAN via API.

## 4. Bikin ulang 17 cron

Gunakan `cron.add` dengan parameter berikut. Body lengkap ada di file `.md` masing-masing
(salin SELURUH isi body tanpa diubah).

### JuanChoxters
| id | title | schedule (WIB) | file body |
|---|---|---|---|
| yt-studio-generate | Generate video CCTV harian | daily 09:16 | yt-studio-generate.md |
| yt-studio-upload-noon | Upload camera trap ke YouTube jam 12:00 | daily 12:00 | yt-studio-upload-noon.md |
| yt-studio-upload | Upload video ke YouTube jam 19:00 | daily 19:00 | yt-studio-upload.md |
| yt-studio-upload-underwater | Upload kamera bawah laut ke YouTube jam 21:00 | daily 21:00 | yt-studio-upload-underwater.md |
| yt-studio-stats-sync | Sinkronisasi statistik YouTube harian | daily 08:16 | yt-studio-stats-sync.md |

### DanteChannel
| id | title | schedule (WIB) | file body |
|---|---|---|---|
| dantechannel-generate | Generate video dongeng harian DanteChannel | daily 05:30 | dantechannel-generate.md |
| dantechannel-upload | Upload pagi DanteChannel jam 7 | daily 07:00 | dantechannel-upload.md |
| dantechannel-shorts-siang | Shorts siang DanteKids jam 12 | daily 12:00 | dantechannel-shorts-siang.md |
| dantechannel-upload-malam | Upload malam DanteChannel jam 7 | daily 19:00 | dantechannel-upload-malam.md |
| dantechannel-shorts-malam | Shorts malam DanteKids jam 8 | daily 20:00 | dantechannel-shorts-malam.md |
| dantechannel-eval | Evaluasi harian DanteKids jam 10 malam | daily 22:00 | dantechannel-eval.md |

### DanteStory / DanteJr / DanteKids
| id | title | schedule (WIB) | file body |
|---|---|---|---|
| dantestory-generate | Generate video harian DanteStory | daily 04:00 | dantestory-generate.md |
| dantestory-upload | Upload DanteStory jam 10 | daily 10:00 | dantestory-upload.md |
| dantejr-generate | Generate video harian DanteJr | daily 04:45 | dantejr-generate.md |
| dantejr-upload | Upload DanteJr jam 13 | daily 13:00 | dantejr-upload.md |
| dantekids-lagu-generate | Generate eksperimen sains harian DanteKids | daily 05:30 | dantekids-lagu-generate.md |
| dantekids-lagu-upload | Upload DanteKids jam 16 | daily 16:00 | dantekids-lagu-upload.md |

Semua: `mode: task`, `timezone: Asia/Jakarta`, `enabled: true`.

## 5. Konsep konten tiap channel (per 2026-10-09)

- **JuanChoxters**: Shorts POV CCTV / camera trap hewan + kamera bawah laut horor (thalassophobia, gaya rekaman ROV asli — grainy, BUKAN 3D).
- **DanteChannel**: dongeng anak 16:9 (±4 menit, 24 adegan) + Shorts 2x/hari. Made-for-kids YES.
- **DanteStory**: misteri makhluk bawah laut (kraken, megalodon, cumi raksasa). 16:9, 18 adegan, audience dewasa.
- **DanteJr**: thalassophobia — POV horor laut dalam. 16:9, 12 adegan, made-for-kids NO.
- **DanteKids**: ikan lucu pinggir laut (kartun anak). 16:9, 12 adegan, made-for-kids YES.

## 6. Troubleshooting umum

- **Video generation 403 "not available in your country or region"**: error ini tergantung region/lingkungan — di VM Dante video generation jalan normal (terakhir dites 2026-10-09, sukses). Jadi JANGAN anggap mati total: coba generate 1x dulu; kalau dapat 403 baru pakai fallback = gambar + efek Ken Burns (zoompan ffmpeg). Jangan paksa retry kalau sudah 403.
- **Upload gagal auth**: laporkan "koneksi YouTube perlu dihubungkan ulang", jangan retry >2x.
- **Google minta verifikasi**: STOP, lapor chat. Jangan coba cara lain.
- **Karakter cacat (anatomi salah)**: generate ulang 1x, masih cacat → batalkan video, lapor chat. Jangan tayangkan yang cacat.

## 7. Verifikasi serah terima

Setelah semua cron aktif, cek `cron.list` — harus ada 17 job enabled.
Jalankan 1x manual tiap job generate untuk memastikan pipeline jalan, lalu biarkan jadwal harian yang ambil alih.
