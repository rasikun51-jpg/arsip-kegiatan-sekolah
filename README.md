# 📂 Dashboard Kustom Arsip & Dokumen Kepanitiaan Kegiatan Sekolah

Pusat dokumentasi dan arsip kegiatan sekolah digital yang modern, ringan, dan terintegrasi langsung dengan ekosistem **Google Drive** serta **Google Sheets**. Dibangun menggunakan *Single-File HTML* dan **Tailwind CSS**.

---

## 🚀 Fitur Utama
* **Pencarian Instan (*Live Search*):** Cari nama kegiatan (misal: *TKA*, *HUT RI*, *Hari Santri*) secara cepat tanpa *reload* halaman.
* **Akses Dokumen Terpusat:** Tombol cepat untuk langsung melihat **SK Kepanitiaan (PDF)**, **Berkas SPJ Keuangan**, dan **Galeri Foto/Video** dari Google Drive.
* **Manajemen Data Mudah:** Tidak perlu edit kode web jika ada kegiatan baru; cukup tambahkan data melalui Google Sheets.
* **Responsif & Estetik:** Tampilan bersih ala aplikasi web modern (*SaaS Dashboard*) yang nyaman diakses lewat laptop maupun *smartphone*.

---

## 🛠️ Panduan Instalasi & Penggunaan

### Langkah 1: Siapkan Google Drive & Google Sheets
1. Buat folder utama di Google Drive Anda (misal: `PUSAT ARSIP SEKOLAH`).
2. Buat file **Google Sheets** baru di dalam folder tersebut dengan nama `Database_Kegiatan`.
3. Buat baris header (*kolom*) pada Sheet pertama dengan urutan:
   * `ID` | `NamaKegiatan` | `Kategori` | `Tahun` | `Deskripsi` | `LinkSK` | `LinkSPJ` | `LinkFoto`

### Langkah 2: Pasang Backend Google Apps Script (GAS)
1. Di dalam Google Sheets Anda, klik menu **Extensions > Apps Script**.
2. Hapus semua kode default, lalu masukkan kode berikut:

```javascript
function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Sheet1"); // Sesuaikan nama sheet jika beda
  var data = sheet.getDataRange().getValues();
  var headers = data[0];
  var result = [];
  
  for (var i = 1; i < data.length; i++) {
    var obj = {};
    for (var j = 0; j < headers.length; j++) {
      obj[headers[j]] = data[i][j];
    }
    result.push(obj);
  }
  
  return ContentService.createTextOutput(JSON.stringify(result))
    .setMimeType(ContentService.MimeType.JSON);
}
