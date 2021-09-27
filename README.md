# Tableau Visualization

Melakukan visualisasi data dan membangun Dashboard menggunakan Tableau

## 1. Mengumpulkan data dari dataset dengan SQL
Karena beberapa alasan tertentu, query yang kita simpan dengan file sql tidak dapat di-import secara langsung pada Tableau. Oleh karena itu, kita perlu melakukan pengumpulan data secara manual dengan menyalin data hasil query lalu menyimpannya ke dalam file Excel.

Berikut beberapa data hasil query yang dibutuhkan untuk melakukan visualisasi nantinya

- Total angka keseluruhan kasus COVID-19 di dunia (Total Angka Kasus, Total Angka Kematian, Presentase Kematian)

```
SELECT SUM(new_cases) AS TotalAngkaKasus, SUM(CAST(new_deaths AS INT)) AS TotalAngkaKematian, 
 SUM(CAST(new_deaths AS INT))/SUM(new_cases)*100 AS PresentaseKematian
FROM PortfolioProject.dbo.CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 1, 2
```

<img width=720 src=https://user-images.githubusercontent.com/74480780/134905019-17cd712f-f3f3-43eb-9533-e8af5773684c.png>

Salin hasil query tersebut ke dalam file Excel

- Total angka kematian di tiap benua

```
SELECT location, SUM(CAST(new_deaths AS INT)) AS TotalAngkaKematian
FROM PortfolioProject.dbo.CovidDeaths
WHERE continent IS NULL AND location NOT IN ('World', 'European Union', 'International')
GROUP BY location
ORDER BY TotalAngkaKematian DESC
```

<img width=720 src=https://user-images.githubusercontent.com/74480780/134904890-d5895351-3575-4f8a-9280-a1a2671c6bc6.png>

- Presentase terinfeksi dari angka terinfeksi tertinggi setiap negara

```
SELECT location, population, MAX(total_cases) AS AngkaInfeksiTertinggi, MAX((total_cases/population))*100
 AS PresentasePopulasiTerinfeksi
FROM PortfolioProject.dbo.CovidDeaths
GROUP BY location, population
ORDER BY PresentasePopulasiTerinfeksi DESC
```

<img width=720 src=https://user-images.githubusercontent.com/74480780/134904785-731cb5c7-7261-4830-a43e-b524e76217ae.png>

Untuk data point yang bernilai NULL perlu diubah ke dalam bentuk numerik, dalam hal ini kita ganti NULL menjadi nilai 0

<img width=720 src=https://user-images.githubusercontent.com/74480780/134902227-2078729a-0bd9-4f57-a25f-31590d8c90ea.png>
  
- Presentase terinfeksi berdasarkan tanggal di tiap negara
  
```
SELECT location, population, date, MAX(total_cases) AS AngkaInfeksiTertinggi, MAX((total_cases/population))*100
 AS PresentasePopulasiTerinfeksi
FROM PortfolioProject.dbo.CovidDeaths
GROUP BY location, population, date
ORDER BY PresentasePopulasiTerinfeksi DESC
```
  
<img width=720 src=https://user-images.githubusercontent.com/74480780/134904628-746b0588-0894-478e-be23-05e7a01a9662.png>

## 2. Import Data

Setelah menyimpan data ke dalam tabel Excel, maka saatnya kita import data tersebut ke dalam Tableau.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134907634-1bdf4ff3-f127-4ed4-a965-09b36b43bfa8.png>

Import semua file excel ke dalam satu lembar kerja Tableau

<img width=720 src=https://user-images.githubusercontent.com/74480780/134908075-6f721c72-b9f4-4d11-b64a-82a542d134bd.png>

## 3. Membuat Visualisasi

### 3.1 Visualisasi total angka keseluruhan kasus COVID-19 di dunia

Pertama olah terlebih dahulu data yang berada di Sheet 1 (data total angka keseluruhan kasus COVID-19 di dunia). Bentuk text table dengan data TotalAngkaKasus, TotalAngkaKematian, dan PresentaseKematian.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134913371-2ddb5492-2fd7-4881-a83d-a788b89323ba.png>

### 3.2 Visualisasi total angka kematian di tiap benua

Lalu untuk Sheet 2 (data total angka kematian di tiap benua) buat horizontal bar dengan menyimpan continent ke dalam `Columns` dan TotalAngkaKematian ke dalam 'Rows'. Personalisasikan agar lebih mudah dipandang. Ubah juga nama field continent menjadi 'Benua' agar lebih menyatu dengan tampilan.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134917123-2d8d4a70-e501-4e0a-b527-498e9bba2635.png>

### 3.3 Visualisasi presentase terinfeksi setiap negara

Untuk Sheet 3 (data presentase terinfeksi dari angka terinfeksi tertinggi setiap negara) karena data tersebut berisi angka tertinggi dari tiap negara yang berbeda, memungkinkan kita membuat visualisasi peta.

Ubah terlebih dahulu field location dengan menentukan tipe datanya menjadi Geographic Role untuk negara.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134918249-1be80c22-2801-4c23-9c80-76c0ded273af.png>

Secara otomatis akan muncul dua field baru yaitu Longitude dan Lotitude, masukkan kedua Longitude ke dalam `Columns` dan Latitude ke dalam `Rows`. Lalu taruh field location ke dalam tab Marks.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134919192-8c911122-dfa5-4e65-8055-1a409cf2b861.png>

Tambahkan pula field PresentasePopulasiTerinfeksi di tab Marks, tepat di bawah location. Lalu ubah opsinya menjadi color.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134919911-13393a52-5a81-44f6-a470-55dc71b09999.png>

Personalisasikan visualisasi peta tersebut agar lebih enak dilihat.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134922086-56753c61-e9bd-43b1-891c-c60f14a1acf6.png>

### 3.4 Visualisasi time-series presentase terinfeksi di tiap negara

Sheet 4 berisi data time-series, sehingga kita akan membuat line chart berdasarkan tanggal yang ada. Masukkan field date ke dalam `Columns` lalu tentukam berdasarkan bulan. Masukkan PresentasePopulasiTerinfeksi ke dalam `Rows` lalu measurment-nya menjadi Average. Masukkan location ke dalam tab Marks dan edit berdasarkan color.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134931463-be2cc4b6-65da-49cd-b744-e1eab8dd6936.png>

Karena terlalu banyak data negara yang dimasukkan ke dalam satu kanvas grafik, filter dan ambil beberapa negara yang akan ditampilkan.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134931884-1d4ba173-aabb-4299-ae50-5f289a163589.png>

Dengan begitu, time-series viasualiasai yang dihasilkan lebih rapi dan informatif. Personalisasikan dengan menambahkan berbagai aspek, semisal forecasting dan lain-lain.

<img width=720 src=https://user-images.githubusercontent.com/74480780/134936910-66e8bf1e-0e1a-415f-9b1c-e3b22f587eca.png>
