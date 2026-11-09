# Dokumentasi Tugas: Form Registrasi Event (Pertemuan 8)

Proyek ini dibuat untuk memenuhi tugas mata kuliah **Desain Web** (Pertemuan 8: *HTML Form Lanjutan: Pilihan, Upload, Textarea, dan Validasi HTML5*). Form dibangun menggunakan HTML murni yang semantik dan ditata dengan eksternal CSS minimalis.

---

## 🛠️ Komponen Form & Aturan Validasi

Sesuai instruksi slide 14, formulir mengimplementasikan elemen-elemen berikut:
- **Radio Button**: Pemilihan kategori peserta tunggal (`Mahasiswa` / `Umum`).
- **Select / Dropdown**: Pemilihan sesi workshop dengan opsi kosong sebagai penjaga (*placeholder*).
- **Checkbox**: Pemilihan fasilitas tambahan opsional (multi-pilihan).
- **File Upload**: Unggah berkas KTM/KTP dengan batasan ekstensi berkas (`accept=".pdf, .jpg, .png"`).
- **Text Area**: Pengisian alasan mengikuti event dengan batasan panjang karakter.

### Aturan Validasi HTML5 yang Digunakan:
1. `required`: Memastikan field tidak boleh dikosongkan.
2. `minlength`: Membatasi jumlah karakter minimum pada input nama (min. 3) dan textarea (min. 20).
3. `maxlength`: Membatasi batas atas karakter (nama maks. 50, textarea maks. 250).
4. `pattern`: Memeriksa format angka 10–12 digit menggunakan regex `[0-9]{10,12}` pada field nomor identitas/WhatsApp.

---

## 🧪 Tabel Test Case Validasi

Pengujian dilakukan pada peramban (*client-side browser validation*) dengan 5 skenario masukan yang salah:

| No | Field / Komponen | Input Uji | Aturan Validasi | Hasil yang Diharapkan | Hasil Aktual | Kesimpulan |
|:--:|:---|:---|:---|:---|:---|:--:|
| 1 | **Nama Lengkap** | `"Al"` (2 karakter) | `minlength="3"` | Browser menolak submit dan meminta penambahan karakter. | Muncul peringatan browser: *"Please lengthen this text to 3 characters or more"*. | **Lolos** |
| 2 | **NIM / No. WA** | `"0812ABCDEF"` / `"12345"` | `pattern="[0-9]{10,12}"` | Browser memblokir pengiriman karena ada huruf atau panjang digit salah. | Muncul tooltip: *"Masukkan 10 hingga 12 digit angka saja"*. Form tidak terkirim. | **Lolos** |
| 3 | **Kategori Peserta** | Tidak ada radio yang dicentang | `required` | Form tertahan dan memfokuskan kursor ke opsi radio. | Muncul notifikasi: *"Please select one of these options"*. | **Lolos** |
| 4 | **Sesi Workshop** | Memilih opsi default (`value=""`) | `required` | Form menolak pengiriman karena nilai opsi kosong. | Muncul notifikasi: *"Please select an item in the list"*. | **Lolos** |
| 5 | **Alasan Event** | `"Ikut saja"` (9 karakter) | `minlength="20"` | Pengiriman gagal karena teks kurang dari 20 karakter. | Muncul tooltip peringatan batas minimal 20 karakter. | **Lolos** |

---

## 🛡️ Analisis Validasi Sisi Server (Server-Side)

Validasi bawaan HTML5 di peramban hanya ditujukan untuk kenyamanan pengguna (*user experience*), bukan lapisan keamanan. Pengguna dapat dengan mudah memodifikasi atau menghapus atribut HTML melalui *Developer Tools* atau mengirim permintaan langsung lewat cURL/Postman.

Oleh karena itu, validasi berikut **wajib diimplementasikan ulang di sisi backend**:
1. **Verifikasi & Keamanan File Upload**:
   - Memeriksa *MIME type* asli dan *magic bytes* file di server (bukan sekadar membaca ekstensi file).
   - Membatasi ukuran fisik file (misalnya maksimum 2 MB).
   - Mengubah nama file (*sanitize & hash filename*) sebelum disimpan ke direktori server untuk mencegah eksploitasi eksekusi skrip berbahaya.
2. **Pengecekan Keunikan Data (*Uniqueness Check*)**:
   - Memeriksa ke database apakah NIM atau nomor WhatsApp sudah terdaftar sebelumnya agar tidak terjadi duplikasi pendaftar.
3. **Whitelist Input Pilihan (*Radio & Select*)**:
   - Memastikan nilai yang dikirim berada dalam daftar nilai valid yang diizinkan sistem (mencegah manipulasi *value* liar).
4. **Sanitasi Data String (XSS & Injection)**:
   - Membersihkan karakter berbahaya pada string nama dan textarea sebelum disimpan ke basis data.

---

## 📝 Ringkasan Perubahan, Masalah, dan Solusi (178 Kata)

Dalam pembuatan formulir registrasi event ini, struktur formulir dibangun menggunakan elemen HTML5 lengkap meliputi radio button, checkbox, select dropdown, textarea, dan file upload. Desain visual dikemas sesederhana mungkin dengan CSS eksternal tanpa framework tambahan untuk menjaga kecepatan muat dan keterbacaan kode.

Masalah utama yang muncul saat perancangan adalah perilaku elemen form bawaan browser yang memiliki margin serta ukuran bervariasi antar perangkat. Kendala lainnya adalah bagaimana memvalidasi nomor identitas yang fleksibel namun tetap membatasi format hanya berupa angka tanpa membuat skrip JavaScript tambahan.

Solusi dari kendala tata letak diatasi dengan menerapkan selektor `box-sizing: border-box` secara global dan menetapkan tata letak vertikal berbasis blok standar. Sementara itu, validasi input angka dipecahkan secara murni melalui atribut HTML5 `pattern="[0-9]{10,12}"` yang dipadukan dengan atribut `title` sebagai panduan pesan kesalahan bagi pengguna. Pendekatan ini menghasilkan formulir yang ringan, mudah dikelola, dan telah lolos pengujian lima skenario validasi masukan yang salah secara konsisten pada peramban modern.

---

## 👥 Catatan Pengembang
- **Nama Praktikan**: [Nama Anda]
- **NIM**: [NIM Anda]
- **Kelas / Kelompok**: [Kelas / Kelompok Anda]
- **Peran**: Perancangan struktur form HTML5, integrasi stylesheet eksternal, pengujian skenario test case, dan penyusunan dokumentasi proyek.