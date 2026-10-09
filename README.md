# Handoff YouTube Jobs → muse1

17 cron job pipeline YouTube (generate + upload + eval) dari Dante dialihkan ke muse1.
Tanggal handoff: 2026-10-09.

## Daftar job

### JuanChoxters (Shorts CCTV)
| File | Jadwal WIB |
|---|---|
| yt-studio-generate.md | 09:16 |
| yt-studio-upload-noon.md | 12:00 |
| yt-studio-upload.md | 19:00 |
| yt-studio-upload-underwater.md | 21:00 |
| yt-studio-stats-sync.md | 08:16 |

### DanteChannel (dongeng anak)
| File | Jadwal WIB |
|---|---|
| dantechannel-generate.md | 05:30 |
| dantechannel-upload.md | 07:00 |
| dantechannel-shorts-siang.md | 12:00 |
| dantechannel-upload-malam.md | 19:00 |
| dantechannel-shorts-malam.md | 20:00 |
| dantechannel-eval.md | 22:00 |

### DanteStory / DanteJr / DanteKids
| File | Jadwal WIB |
|---|---|
| dantestory-generate.md | 04:00 |
| dantestory-upload.md | 10:00 |
| dantejr-generate.md | 04:45 |
| dantejr-upload.md | 13:00 |
| dantekids-lagu-generate.md | 05:30 |
| dantekids-lagu-upload.md | 16:00 |

## Yang HARUS dilakukan manual di muse1

1. **Bikin ulang semua cron** dari isi file .md (body = instruksi job).
2. **YouTube OAuth** (JuanChoxters): token di `~/.hermes/` TIDAK bisa ditransfer — hubungkan ulang via OAuth.
3. **Login browser YouTube Studio** (DanteChannel, DanteStory, DanteJr, DanteKids): sesi browser TIDAK bisa ditransfer — login ulang manual.
4. **Workspace files**: direktori `~/workspace/yt-studio/`, `~/workspace/dantechannel/`, `~/workspace/dantestory/`, `~/workspace/dantejr/`, `~/workspace/dantekids_lagu/` perlu disalin ke VM muse1 (via git atau copy manual).

## Aturan tetap dari detta
- Sukses = diam, lapor hanya yang tayang/gagal
- Hashtag wajib di setiap upload
- Stop + lapor jika Google minta verifikasi ulang

## Status (2026-10-09)
Semua 17 cron di akun Dante sudah di-DISABLE. Tidak akan dobel jalan.

## Yang BISA ditransfer ✅
- File definisi 17 job (folder ini)
- File workspace: `~/workspace/yt-studio/`, `~/workspace/dantechannel/`, `~/workspace/dantestory/`, `~/workspace/dantejr/`, `~/workspace/dantekids_lagu/` — clone dari VM Dante atau minta detta copy

## Yang TIDAK BISA ditransfer ❌ (wajib setup ulang di muse1)
- Token OAuth YouTube (JuanChoxters) — hubungkan ulang
- Sesi login browser YouTube Studio — login ulang manual
- Koneksi Threads/Gmail milik Dante — bukan bagian job ini
