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
  
Perhatikan juga untuk field date, perbaiki formatnya ke dalam bentuk shortdate di Excel.
  
<img width=720 src=https://user-images.githubusercontent.com/74480780/134905571-a97817d5-106f-450c-91ec-806e2e83053d.png>
