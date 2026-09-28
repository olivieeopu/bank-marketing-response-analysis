# bank-marketing-response-analysis
Analisis respons kampanye pemasaran bank menggunakan R, mencakup data cleaning, EDA, feature engineering, visualisasi interaktif, dan data storytelling.

# Analisis Respons Kampanye Pemasaran Bank

Exploratory Data Analysis (EDA) menggunakan R untuk mengeksplorasi profil nasabah, riwayat kontak, dan respons terhadap kampanye pemasaran bank.

Proyek ini mencakup pemeriksaan kualitas data, preprocessing, feature engineering, analisis hubungan antarvariabel, serta visualisasi interaktif. Fokus utamanya adalah memahami pola respons dalam dataset dan menyusun arah evaluasi kampanye.

## Pertanyaan Analisis

- Bagaimana karakteristik nasabah dan riwayat kontak dalam dataset?
- Bagaimana tingkat respons positif berbeda menurut metode kontak?
- Bagaimana distribusi respons menurut pendidikan dan metode kontak?
- Apakah durasi percakapan berkaitan dengan riwayat kontak dan hasil kampanye sebelumnya?
- Bagaimana keputusan preprocessing memengaruhi data yang dianalisis?

## Dataset

Dataset berisi informasi profil nasabah, aktivitas kontak, dan hasil kampanye pemasaran bank.

| Komponen | Keterangan |
|---|---|
| Observasi awal | 8.238 |
| Observasi setelah preprocessing | 7.130 |
| Target analisis | `y` |
| Respons positif | `y = yes` |
| Respons negatif | `y = no` |

Sumber asli dan lisensi dataset belum dicantumkan dalam laporan HTML. Informasi tersebut perlu dilengkapi sebelum dataset dibagikan melalui repository.

### Variabel Utama

| Kelompok | Variabel |
|---|---|
| Profil nasabah | `age`, `job`, `marital`, `education` |
| Informasi kredit dan pinjaman | `default`, `housing`, `loan` |
| Kontak kampanye | `contact`, `month`, `day_of_week`, `duration`, `campaign` |
| Riwayat kampanye | `pdays`, `previous`, `poutcome` |
| Hasil kampanye saat ini | `y` |

`poutcome` menunjukkan hasil kampanye sebelumnya, sedangkan `y` menunjukkan respons pada kampanye yang dianalisis.

## Tools

- **R** — pengolahan data dan analisis statistik.
- **dplyr** — transformasi, pengelompokan, dan agregasi data.
- **ggplot2** — visualisasi data.
- **Plotly** — visualisasi interaktif.

## Alur Analisis

### 1. Data Quality Assessment

Pemeriksaan awal mencakup:

- Missing values.
- Duplikasi.
- Kategori kosong dan `unknown`.
- Distribusi variabel numerik.
- Nilai ekstrem.
- Nilai khusus seperti `pdays = 999`, yang menunjukkan belum pernah dihubungi sebelumnya.

Beberapa temuan awal:

- Terdapat 9 missing values pada `duration`.
- Kolom `job` memiliki kategori kosong dan `unknown`.
- Sejumlah variabel kategorikal mengandung nilai `unknown`.
- Distribusi `duration` dan `campaign` memiliki nilai ekstrem.

Nilai ekstrem tidak otomatis merupakan kesalahan data. Karena itu, pengaruh penyaringan terhadap hasil analisis perlu diperiksa.

### 2. Preprocessing

Langkah yang tercatat dalam laporan:

- Menyeragamkan kategori kosong pada `job` menjadi `unknown`.
- Menghapus observasi dengan missing values pada `duration`.
- Menghapus observasi dengan kategori `unknown` pada `job` dan `marital`.
- Menyaring nilai tinggi pada `duration` dan `campaign`; data akhir memiliki maksimum 635 detik dan 6 kontak.
- Membentuk indikator riwayat kontak untuk mempermudah interpretasi.

Laporan juga mengubah label `unknown` pada `default` menjadi `not_disclosed`. Label tersebut perlu ditinjau kembali karena informasi yang tidak diketahui belum tentu berarti nasabah menolak mengungkapkannya.

Setelah preprocessing, jumlah observasi berkurang dari **8.238 menjadi 7.130**, atau sekitar **13,45%**.

### 3. Exploratory Data Analysis

Eksplorasi dilakukan melalui:

- Statistik deskriptif, termasuk mean, median, standar deviasi, dan IQR.
- Distribusi usia menurut pekerjaan dan status pernikahan.
- Distribusi durasi kontak menurut hasil kampanye.
- Perbandingan respons berdasarkan metode kontak.
- Visualisasi respons menurut pendidikan dan metode kontak.

### 4. Feature Engineering

| Fitur | Tujuan |
|---|---|
| `pdays_contacted` | Membedakan nilai penanda belum pernah dihubungi dari nilai hari kontak sebelumnya. |
| `previous_contacted` | Menunjukkan apakah nasabah memiliki riwayat kontak sebelumnya. |
| `age_group` | Mengelompokkan usia menjadi kurang dari 30, 30–50, dan lebih dari 50 tahun. |
| `recency_level` | Mengelompokkan riwayat kontak menjadi `never`, `recent`, dan `old`. |
| `total_contact_score` | Menjumlahkan `campaign` dan `previous` sebagai ringkasan jumlah kontak. |
| `education_grouped` | Menggabungkan kategori pendidikan untuk mempermudah visualisasi. |

### 5. Analisis Hubungan

Laporan mencakup:

- Uji Kendall antara durasi kontak dan pengodean hasil kampanye sebelumnya.
- Uji Kendall antara durasi kontak dan indikator pernah dihubungi.
- Uji chi-square antara `poutcome` dan `previous_contacted`.

Interpretasi hubungan dengan `poutcome` perlu berhati-hati karena kategorinya tidak memiliki urutan numerik alami yang jelas.

Hubungan antara `poutcome` dan `previous_contacted` juga sebagian berasal dari definisi variabel: nasabah yang belum pernah dihubungi tidak memiliki hasil kampanye sebelumnya. Hubungan tersebut tidak diperlakukan sebagai temuan bisnis yang independen.

## Hasil Utama

Angka berikut dihitung dari tabel frekuensi pada **data setelah preprocessing**.

Tingkat respons positif dihitung sebagai:

```text
Jumlah observasi dengan y = yes
──────────────────────────────── × 100%
Total observasi dalam kelompok
```

| Kelompok | Respons positif | Total observasi | Tingkat respons positif |
|---|---:|---:|---:|
| Keseluruhan | 579 | 7.130 | 8,12% |
| Cellular | 513 | 4.584 | 11,19% |
| Telephone | 66 | 2.546 | 2,59% |

### 1. Perbedaan Respons Menurut Metode Kontak

Kelompok cellular memiliki tingkat respons positif **11,19%**, sedangkan telephone **2,59%**, dengan selisih sekitar **8,60 poin persentase**.

Perbandingan menggunakan persentase dalam masing-masing kelompok karena jumlah observasi kedua metode kontak berbeda.

Temuan ini menunjukkan asosiasi dalam dataset setelah preprocessing. Hasilnya belum membuktikan bahwa penggunaan cellular menyebabkan peningkatan respons, karena profil nasabah, riwayat kampanye, dan faktor lainnya dapat berbeda antarkelompok.

![Jumlah respons kampanye menurut metode kontak](images/contact-method-outcome.png)

*Grafik dari laporan menunjukkan jumlah respons. Persentase pada tabel di atas dihitung secara terpisah menggunakan total observasi masing-masing metode kontak.*

### 2. Durasi Percakapan Berbeda Menurut Respons

Pada data setelah preprocessing:

| Respons | Rata-rata durasi | Median durasi |
|---|---:|---:|
| Positif (`yes`) | Sekitar 327 detik | 299 detik |
| Negatif (`no`) | Sekitar 193 detik | 158 detik |

Respons positif disertai durasi percakapan yang lebih panjang dalam data yang dianalisis. Namun, percakapan yang lebih panjang belum tentu menyebabkan respons positif; durasi juga dapat mencerminkan ketertarikan yang sudah muncul selama percakapan.

Durasi baru diketahui setelah kontak berlangsung, sehingga tidak dapat langsung digunakan untuk menentukan target nasabah sebelum panggilan.

### 3. Analisis Pendidikan dan Metode Kontak

Visualisasi berdasarkan pendidikan dan metode kontak membantu mengeksplorasi perbedaan distribusi respons antarsegmen.

<img width="670" height="496" alt="Screenshot 2026-09-28 at 17 53 36" src="https://github.com/user-attachments/assets/ab683dcb-0929-49f4-9130-d75edf49cefb" />

<img width="670" height="501" alt="Screenshot 2026-09-28 at 17 54 00" src="https://github.com/user-attachments/assets/c374645b-9cbf-4760-a65f-7dad6d823512" />


Perbandingan antarsegmen perlu mempertimbangkan ukuran kelompok dan tingkat respons dalam setiap kelompok. Jumlah respons positif yang besar tidak otomatis menunjukkan tingkat keberhasilan yang lebih tinggi.

## Arah Evaluasi Kampanye

Berdasarkan temuan deskriptif, analisis lanjutan dapat diarahkan untuk:

1. **Membandingkan metode kontak pada profil nasabah yang serupa.**  
   Periksa apakah perbedaan respons tetap terlihat setelah mempertimbangkan pendidikan, usia, waktu kontak, dan riwayat kampanye.

2. **Mengevaluasi riwayat kampanye sebagai dasar segmentasi.**  
   Bandingkan tingkat respons menurut `poutcome` dan recency sebelum menetapkan prioritas follow-up.

3. **Menggunakan tingkat respons bersama ukuran kelompok.**  
   Sajikan jumlah observasi agar segmen kecil tidak dianggap lebih menjanjikan hanya karena memiliki persentase tinggi.

4. **Memvalidasi perubahan strategi sebelum diterapkan secara luas.**  
   Temuan observasional dapat digunakan untuk menyusun hipotesis yang kemudian diuji melalui eksperimen kampanye.

Rekomendasi tersebut belum diterapkan atau diuji dalam kegiatan operasional. Proyek ini tidak mengukur peningkatan pendapatan, ROI, atau efisiensi kampanye.

## Keterbatasan dan Catatan Interpretasi

### Dampak Penyaringan Data

Proporsi respons positif berubah setelah preprocessing:

| Tahap | Respons positif | Total observasi | Proporsi |
|---|---:|---:|---:|
| Data awal | 890 | 8.238 | 10,80% |
| Setelah preprocessing | 579 | 7.130 | 8,12% |

Perubahan ini menunjukkan bahwa penyaringan memengaruhi komposisi hasil kampanye. Temuan pada data bersih perlu dibandingkan dengan data sebelum penyaringan, terutama karena panggilan panjang dan frekuensi kontak tinggi belum tentu merupakan kesalahan pencatatan.

### Konsistensi Laporan

HTML merupakan laporan akademik yang masih memiliki beberapa ketidaksesuaian narasi:

- Bagian hubungan `duration` dan `previous_contacted` menyebut Spearman, tetapi output yang ditampilkan adalah Kendall.
- Beberapa p-value dalam narasi berbeda dari output uji.
- Angka sekitar 254 detik merupakan rata-rata durasi seluruh data awal, bukan rata-rata durasi respons positif pada data bersih.
- Rekomendasi batas durasi panggilan dan peningkatan ROI belum memiliki bukti evaluasi yang memadai.

README ini menggunakan angka pada tabel output dan membatasi kesimpulan pada hasil deskriptif yang didukung laporan. Dokumen HTML masih perlu diselaraskan dengan interpretasi tersebut.

### Batas Analisis

- Analisis bersifat observasional dan tidak membuktikan hubungan sebab-akibat.
- Proyek tidak mencakup model prediksi atau evaluasi dampak kampanye melalui eksperimen.
- Kategori `unknown` tidak otomatis merupakan data yang salah.
- Keterwakilan dataset terhadap populasi nasabah belum diketahui.

## Kesimpulan

Pada data setelah preprocessing, tingkat respons positif berbeda menurut metode kontak, dengan cellular sebesar 11,19% dan telephone sebesar 2,59%. Respons positif juga disertai rata-rata durasi percakapan yang lebih panjang.

Temuan tersebut dapat menjadi dasar eksplorasi segmentasi dan evaluasi kampanye lebih lanjut. Namun, perubahan komposisi data akibat penyaringan serta potensi perbedaan karakteristik nasabah perlu diperhatikan sebelum membuat keputusan operasional.


## Pengembangan Selanjutnya

- Melengkapi sumber dan informasi lisensi dataset.
- Menambahkan source R/R Markdown untuk reproduksibilitas.
- Membandingkan hasil sebelum dan sesudah penyaringan nilai ekstrem.
- Menyelaraskan metode statistik, p-value, dan narasi pada laporan.
- Menampilkan response rate beserta jumlah observasi pada setiap segmen.
- Menguji perbedaan metode kontak dengan mempertimbangkan faktor lain.
