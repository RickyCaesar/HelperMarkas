# Pencarian NPSN BOSDA

![Status](https://img.shields.io/badge/Status-Active-success)
![Made with](https://img.shields.io/badge/Made%20with-HTML%2FCSS%2FJS-informational)
![License](https://img.shields.io/badge/License-Private-lightgrey)

> Aplikasi web lokal untuk membantu verifikator BOS Daerah mencari sekolah berdasarkan NPSN, menghasilkan ringkasan, menghitung persentase terhadap “Jumlah Dana”, serta mengirim notifikasi ke Telegram.

---

## Fitur
- Pencarian cepat berdasarkan NPSN (tampil Nama Sekolah, status, program, tanggal).
- Ringkasan terformat siap salin via tombol “Salin Hasil”.
- Input koreksi manual (satu baris = satu poin) dengan tombol “Bersihkan Koreksi”.
- Kalkulator persentase: menghitung (Nilai yang diisi / Jumlah Dana × 100) dengan presisi detail (tanpa pembulatan).
- Notifikasi Telegram via tombol “Sah!!” dan “Ditolak!!”, termasuk dukungan pengiriman ke Topik Grup (message_thread_id).
- Input Nama Verifikator dan No. HP/WA yang tersimpan di browser.

---

## Cara Menjalankan (Lokal)
- Buka: [index.html](./index.html)
- Masukkan NPSN lalu Enter atau klik “Cari”.
- Pilih “Jenis Kertas Kerja” sesuai kebutuhan:
  - BOS Daerah, BOS Reguler, BOS Kinerja, SiLPA BOS Kinerja, atau BOS Lainnya.
- Isi “Nama Verifikator” dan “No. HP/WA” (tersimpan otomatis di browser).
- Isi koreksi pada area “Koreksi (isi manual, satu baris = satu poin)”:
  - Gunakan tombol “Bersihkan Koreksi” untuk menghapus semua catatan dengan cepat.
- Isi “Nilai yang diisi” di bagian kalkulator untuk melihat persentase terhadap “Jumlah Dana”.
- Klik “Salin Hasil” untuk menyalin ringkasan.

---

## Sumber Data
- Data sekolah ditanam dalam halaman pada blok CSV.
- Kolom yang dibaca:
  - NPSN
  - Nama Sekolah
  - Jumlah Dana
- “Jumlah Dana” digunakan untuk perhitungan persentase.

---

## Format Ringkasan
Contoh hasil yang disalin:

```
30409868 - SMK INNE DONGHWA
Ditolak pada 25/03/2026

BOS Daerah

Perlu dijelaskan/diperbaiki :

1. koreksi 1

2. Koreksi 2

3. dst

Verifikator : Ricky - 085751664898
```

- Koreksi diisi manual (satu baris = satu poin).
- Nama verifikator dan No. HP/WA dapat diisi oleh pengguna.

---

## Kalkulator Persentase
- Di bawah hasil, terdapat:
  - “Jumlah Dana”: otomatis tampil dari data sekolah.
  - “Nilai yang diisi”: masukkan angka (misal 12500000).
  - “Persentase”: dihitung otomatis dan ditampilkan detail tanpa pembulatan (12 digit desimal).

Formula:
```
Persentase = (Nilai yang diisi / Jumlah Dana) × 100
```

---

## Kirim Notifikasi ke Telegram
Aplikasi menyediakan dua tombol:
- “Sah!!” untuk mengirim pesan bahwa kertas kerja disahkan.
- “Ditolak!!” untuk mengirim pesan bahwa kertas kerja ditolak.

Format pesan ringkas:
- Ditolak: `Pengajuan BOS Daerah {NPSN} - {Nama Sekolah} Ditolak, silahkan cek hasil verifikasi kami`
- Disahkan: `Pengajuan BOS Daerah {NPSN} - {Nama Sekolah} disahkan, silahkan cek status`

Pengaturan kirim:
- Bot Token, Chat ID, dan Topic ID (opsional) diisi pada form “Kirim ke Telegram”.
- Topic ID diisi bila ingin mengirim ke Topik Grup (message_thread_id).
- Nilai tersimpan di browser (localStorage).

### Menyiapkan Bot Telegram (Detail Lengkap)
1. Buat Bot via BotFather:
   - Buka Telegram, cari akun resmi “@BotFather”.
   - Ketik `/start`.
   - Ketik `/newbot` lalu ikuti instruksi (beri “Bot name” dan “username” yang diakhiri `bot`, mis. `npsn_helper_bot`).
   - Setelah selesai, BotFather memberikan “HTTP API token” (format seperti `1234567890:ABCDEF...`). Simpan token ini.

2. Dapatkan Chat ID:
   - Opsi A: Chat pribadi
     - Cari bot Anda, klik “Start” (atau kirim pesan apa pun ke bot).
     - Buka tautan di browser (ganti TOKEN):
        - `https://api.telegram.org/botTOKEN/getUpdates`
     - Cari objek `chat` pada respon JSON; ambil `chat.id` (angka positif).
   - Opsi B: Group/Channel
     - Tambahkan bot ke grup/ch
     - Nonaktifkan “privacy mode” agar bot dapat membaca pesan:
       - Ke @BotFather: `/setprivacy` → pilih bot → pilih “Disable”
     - Kirim pesan apa pun di grup.
     - Buka:
        - `https://api.telegram.org/botTOKEN/getUpdates`
     - Ambil `chat.id` (untuk grup biasanya angka negatif, mis. `-1001234567890`).

3. Uji Kirim Pesan (opsional):
   - Buka di browser (ganti TOKEN dan CHAT_ID; encode spasi dengan `%20`):
     - `https://api.telegram.org/botTOKEN/sendMessage?chat_id=CHAT_ID&text=Halo%20dari%20NPSN%20BOSDA`
   - Jika “ok: true”, bot siap digunakan di aplikasi.

4. Isi di Aplikasi:
   - Pada bagian “Kirim ke Telegram”:
     - Bot Token: isi token dari BotFather.
     - Chat ID: isi `chat.id` yang sudah diperoleh.
      - Topic ID (opsional): isi `message_thread_id` bila ingin mengirim ke Topik Grup.
   - Nilai akan tersimpan di browser (localStorage) agar tidak perlu diisi ulang.
   - Klik “Sah!!” atau “Ditolak!!” untuk mengirim pesan.

### Catatan Penting
- Token bersifat rahasia. Jangan membagikan ke publik atau commit ke repository.
- Jika mengirim ke grup:
  - Pastikan bot sudah ditambahkan ke grup.
  - Pertimbangkan menjadikan bot sebagai admin jika diperlukan (tergantung pengaturan grup).
  - Jika pesan tidak masuk, coba nonaktifkan “privacy mode” di @BotFather seperti langkah di atas.
- Jika API menolak:
  - Periksa kembali TOKEN dan CHAT_ID.
  - Pastikan Anda sudah mengirim pesan terlebih dahulu di chat/grup agar `getUpdates` menampilkan `chat.id`.
  - Coba uji dengan URL `sendMessage` seperti di atas untuk memastikan token & chat id valid.

---

## Kustomisasi (Opsional)
Ubah nilai default di [index.html](./index.html):
- Nama verifikator dan nomor:
  - Cari konstanta: `DEFAULT_VER_NAME`, `DEFAULT_VER_PHONE`.
- Program dan tanggal default:
  - `DEFAULT_PROGRAM`, `DEFAULT_DATE`.
- Presisi persentase:
  - Fungsi `formatPercentTrunc(pct, 12)` → ubah parameter `12` untuk mengatur digit desimal.
- Format pesan Telegram:
  - Fungsi `sendTelegram(decision)` menyusun teks dari hasil + persentase + keputusan.

---

## Batasan
- Data CSV ditanam langsung dalam halaman; untuk memperbarui data, edit blok CSV di dalam `index.html`.
- Aplikasi berjalan lokal di browser; akses ke API Telegram dilakukan langsung dari browser menggunakan HTTPS.
- Token Telegram disimpan di localStorage pada browser yang Anda gunakan (bersifat per-perangkat/per-browser).

---

## Troubleshooting
- Tidak ada hasil saat cari NPSN:
  - Pastikan NPSN ada di data CSV yang tertanam.
  - Coba ketik hanya angka (tanpa spasi/tanda baca).
- Persentase tidak muncul:
  - Pastikan sudah memilih NPSN agar “Jumlah Dana” terisi.
  - Pastikan mengisi “Nilai yang diisi” dengan angka.
- Telegram gagal mengirim:
  - Cek kembali Token dan Chat ID.
  - Tes kirim manual via URL `sendMessage`.
  - Untuk grup, nonaktifkan “privacy mode” di @BotFather (`/setprivacy → Disable`).
  - Pastikan koneksi internet aktif (Telegram API butuh internet).

---

## Ringkas
- File utama: [index.html](./index.html)
- Fitur: Cari NPSN, hasil ringkasan, salin hasil, koreksi manual, kalkulator persentase, kirim Telegram (Sah!!/Ditolak!!).
- Konfigurasi Telegram: isi Bot Token & Chat ID pada form yang disediakan, tombol akan mengirim pesan ringkasan ke Telegram.

---

## Repo Stats (GitHub)
> Ganti `USERNAME` dan `REPO` dengan nama akun dan repo GitHub Anda agar badge/ kartu tampil benar.

![Stars](https://img.shields.io/github/stars/USERNAME/REPO?style=social)
![Forks](https://img.shields.io/github/forks/USERNAME/REPO?style=social)
![Issues](https://img.shields.io/github/issues/USERNAME/REPO)

[![Repo card](https://github-readme-stats.vercel.app/api/pin/?username=USERNAME&repo=REPO&show_owner=true)](https://github.com/USERNAME/REPO)

[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=USERNAME&layout=compact)](https://github.com/USERNAME)
