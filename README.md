# Excel Solver Optimization: Perencanaan Gizi & Vehicle Routing
 
*Hackathon Excel DQLab | Operations Research*
 
Proyek ini memakai Microsoft Excel dan Solver untuk memodelkan dua masalah optimasi dengan batasan (constraint):
 
- **Perencanaan Gizi (Integer Linear Programming):** menyusun rencana belanja bahan makanan yang memenuhi target protein dan serat dalam batas anggaran dan batas jumlah pembelian tiap bahan.
- **Vehicle Routing Problem (VRP):** membagi lokasi pelanggan ke rute truk dengan tetap memperhatikan kapasitas kendaraan, sambil meminimalkan total jarak tempuh.
---
 
## Overview
 
Proyek ini menunjukkan bagaimana formula spreadsheet, variabel keputusan, fungsi tujuan (objective), dan constraint dapat mengubah data mentah menjadi model perencanaan yang praktis.
 
| | Kasus 1: Perencanaan Gizi | Kasus 2: Vehicle Routing Problem |
|---|---|---|
| **Masalah** | Memenuhi kebutuhan protein dan serat keluarga dengan biaya belanja serendah mungkin | Menentukan rute 10 truk untuk mengirim barang ke 50 pelanggan dengan jarak total sesingkat mungkin |
| **Tipe optimasi** | Integer Linear Programming | Combinatorial optimization (routing) |
| **Keputusan** | Jumlah unit tiap bahan makanan dan menu harian | Urutan kunjungan pelanggan tiap truk |
| **Tujuan** | Minimalkan biaya | Minimalkan total jarak tempuh |
| **Hasil** | Biaya **Rp1.910.000** (hemat Rp90.000 dari anggaran) | Total jarak **1.153,24 km**, 259 unit terkirim |
 
---
 
## Dataset
 
### Kasus 1: Perencanaan Gizi (`excelhack03-perencanaan-gizi-dqlab.xlsx`)
 
| Parameter | Nilai |
|---|---|
| Anggaran belanja maksimum | Rp2.000.000 |
| Target protein / serat | ≥ 2.000 g / ≥ 500 g |
| Bahan | Telur, Tempe, Ayam, Bayam, Oat (harga, protein, serat, batas beli minimum–maksimum per bahan) |
| Kebutuhan harian (berat badan 60 kg) | Protein 0,8–1 g/kg (48–60 g), serat 25–30 g |
 
### Kasus 2: VRP (`excelhack03-perencanaan-routing-dqlab.xlsx`)
 
| Parameter | Nilai |
|---|---|
| Depot | 1 gudang pusat (ID 0) |
| Pelanggan | 50 lokasi, total permintaan (demand) **259 unit** |
| Armada | 10 truk identik, kapasitas **28 unit** |
| Jarak | Matriks jarak 51 × 51 (km) |
 
*Sumber data: workbook hackathon DQLab yang disediakan. Tidak ada dataset eksternal yang ditambahkan.*
 
---
 
## Tools & Metode
 
| Tool / Metode | Kegunaan |
|---|---|
| **Microsoft Excel** | Pengorganisasian data, perhitungan berbasis formula, dan ringkasan hasil |
| **Excel Solver Add-in** | Optimasi objective dengan constraint |
| **Simplex LP** | Optimasi linear (dengan constraint integer) untuk model gizi |
| **Evolutionary Algorithm** | Metode pencarian untuk masalah kombinatorial VRP |
 
### Formula yang digunakan
 
| Formula | Peran dalam proyek |
|---|---|
| `INDEX` | Mengambil demand pelanggan dan jarak dari tabel referensi / matriks jarak |
| `IF` | Menangani pemisah rute, pemetaan depot, dan posisi rute yang kosong |
| `COUNTIF` | Mengidentifikasi pemisah rute dan menghitung nomor rute |
| `COUNTIFS` | Menghitung posisi dan jumlah pelanggan dalam rute dengan beberapa kriteria |
| `SUMPRODUCT` | Menjumlahkan total gizi dan muatan truk |
| `SUM` / `SUMIF` | Menghitung total dan ringkasan per rute |
 
---
 
## Alur Pengerjaan
 
1. **Siapkan input:** tinjau harga bahan, nilai gizi, demand pelanggan, matriks jarak, jumlah truk, dan kapasitas.
2. **Tentukan variabel keputusan:** jumlah unit bahan makanan untuk model gizi; urutan pelanggan yang dikodekan beserta pemisah rute untuk VRP.
3. **Bangun logika formula:** hitung biaya belanja, protein, serat, jarak rute, dan demand per truk. Pada sheet `Helper`, fungsi objective `K23` = total jarak (`K16`) + bobot penalti (`K22`) × penalti pelanggaran. Sel penalti (`K19:K21`) saat ini bernilai 0 karena batasan dikendalikan langsung lewat constraint Solver.
4. **Atur Solver:** pilih objective, variabel keputusan, constraint, dan metode penyelesaian yang sesuai.
5. **Cek kelayakan:** pastikan anggaran dan target gizi terpenuhi, setiap pelanggan dilayani satu kali, dan tidak ada truk yang melebihi kapasitas.
6. **Tinjau hasil:** periksa rencana belanja dan ringkasan rute setelah Solver selesai.
---
 
## Hasil
 
### Kasus 1: Perencanaan Gizi
 
#### Tugas 1: Pembelian Optimal
 
| Bahan | Jumlah (unit) | Biaya |
|---|---|---|
| Telur | 26 | Rp780.000 |
| Tempe | 40 | Rp600.000 |
| Ayam | 2 | Rp80.000 |
| Bayam | 5 | Rp50.000 |
| Oat | 16 | Rp400.000 |
| **Total** | | **Rp1.910.000** |
 
- Protein **2.021 g** (target ≥ 2.000), serat **500 g** (target ≥ 500), rasio Tempe : Ayam = **20** (batas ≤ 20).
- Biaya Rp1.910.000 berada di bawah anggaran Rp2.000.000 (selisih Rp90.000).
- Hasil ini **sama dengan solusi optimal global** yang dihitung ulang secara independen dengan Integer Programming di Python.
#### Tugas 2: Menu Harian
 
| Bahan | Per hari (unit) |
|---|---|
| Tempe | 2,946 |
| Bayam | 0,357 |
| Telur, Ayam, Oat | 0 |
 
- Protein **60 g/hari** (batas maksimum), serat **25 g/hari** (batas minimum).
- Biaya **Rp47.768/hari**, atau **Rp286.607** untuk 6 hari. Nilai ini sama dengan solusi Linear Programming.
### Kasus 2: Vehicle Routing Problem
 
| Truk | Rute | Jarak (km) | Muatan |
|---|---|---|---|
| 1 | 0 → 36 → 23 → 9 → 10 → 37 → 38 → 0 | 146,86 | 28 |
| 2 | 0 → 20 → 13 → 33 → 5 → 34 → 45 → 1 → 0 | 125,46 | 28 |
| 3 | 0 → 49 → 47 → 11 → 39 → 0 | 72,97 | 18 |
| 4 | 0 → 28 → 40 → 16 → 25 → 0 | 110,37 | 26 |
| 5 | 0 → 15 → 14 → 44 → 6 → 46 → 0 | 132,04 | 26 |
| 6 | 0 → 26 → 12 → 7 → 42 → 50 → 0 | 124,80 | 28 |
| 7 | 0 → 22 → 29 → 41 → 0 | 67,71 | 27 |
| 8 | 0 → 3 → 21 → 4 → 18 → 24 → 0 | 112,75 | 28 |
| 9 | 0 → 43 → 48 → 32 → 8 → 0 | 105,22 | 23 |
| 10 | 0 → 30 → 35 → 2 → 27 → 19 → 31 → 17 → 0 | 155,06 | 27 |
| **Total** | | **1.153,24** | **259** |
 
**Validasi constraint:**
 
- Semua 50 pelanggan dikunjungi tepat satu kali oleh satu truk.
- Setiap truk berangkat dari depot dan kembali ke depot.
- Muatan maksimum per truk 28 unit (tidak ada yang melebihi).
- Total demand 259 unit, sesuai total permintaan.
> **Catatan pengembangan:** sebagai pembanding, solver routing khusus (Google OR-Tools, 120 detik) menemukan total jarak sekitar **1.068,8 km**, sekitar 7% lebih pendek. Solver Evolutionary di Excel menghasilkan solusi valid yang baik, tetapi belum tentu mendekati optimum. Pengembangan berikutnya: jalankan Solver beberapa kali dengan *random seed* berbeda, tambah waktu iterasi, atau mulai dari solusi awal berbasis heuristik (mis. *nearest neighbor*).
 
---
 
## Struktur Repository
 
```
excel-solver-optimization/
├── excel-solver/
│   ├── excelhack03-perencanaan-gizi-dqlab.xlsx
│   └── excelhack03-perencanaan-routing-dqlab.xlsx
├── raw-data/
│   ├── soal-perencanaan-gizi.pdf
│   └── soal-vehicle-routing-problem.pdf
├── docs/
│   ├── soal-perencanaan-gizi.pdf
│   └── soal-vehicle-routing-problem.pdf
└── README.md
```
 
---
 
## How to Run
 
### Prasyarat
 
- Microsoft Excel versi desktop
- **Solver Add-in** Excel yang sudah aktif
- Kedua workbook `.xlsx` dari proyek ini
Untuk mengaktifkan Solver, buka **File → Options → Add-ins → Excel Add-ins → Go**, lalu centang **Solver Add-in**.
 
### A. Menjalankan Model Perencanaan Gizi
 
1. Buka `excelhack03-perencanaan-gizi-dqlab.xlsx`, lalu pilih sheet `Bahan dan Menu`.
2. Buka **Data → Solver**.
3. Atur model:
   - **Set Objective:** `H12` (total biaya), pilih **Min**.
   - **By Changing Variable Cells:** `G7:G11` (jumlah unit yang direncanakan).
   - **Constraints:**
     - `H12 <= B2`
     - `E2 >= B3`
     - `E3 >= B4`
     - `G7:G11 >= E7:E11` dan `G7:G11 <= F7:F11`
     - `G7:G11 = integer`
     - `E4 <= 20` (rasio Tempe : Ayam)
   - **Select a Solving Method:** **Simplex LP**.
4. Klik **Solve**, simpan solusi jika semua constraint terpenuhi, lalu tinjau biaya dan total gizi yang diperbarui.
5. Menu harian (`B21:G25`) dihitung dengan formula dan tidak memerlukan Solver.
### B. Menjalankan Model Vehicle Routing
 
1. Buka `excelhack03-perencanaan-routing-dqlab.xlsx`.
2. Buka **Data → Solver**.
3. Atur model:
   - **Set Objective:** `Helper!K23`, pilih **Min**.
   - **By Changing Variable Cells:** `Helper!B3:B61` (urutan rute yang dikodekan).
   - **Select a Solving Method:** **Evolutionary**.
   - **Constraints:**
     - `Helper!B3:B61` bertipe **integer** dengan batas `1` sampai `59`.
     - `Helper!B3:B61` memakai constraint **dif (alldifferent)** agar setiap ID dipakai tepat satu kali.
     - `Helper!L3:L12 <= Helper!K17` (kapasitas truk, 28 unit).
     - `Helper!M3:M12 <= Helper!K18` (maksimum 8 pelanggan per rute, sesuai layout sheet `Solution`).
   - Pertahankan pengodean rute pada workbook: ID pelanggan adalah `1–50`, sedangkan nilai `51–59` adalah pemisah rute (menghasilkan 10 rute).
4. Klik **Solve**. Salin rute dari `Helper` ke sheet `Solution` dengan **Paste as Value**, lalu cek kolom **Jarak** dan **Total Unit** (total harus 259). Pastikan setiap pelanggan muncul satu kali, setiap rute dimulai dan diakhiri di depot, dan demand tiap truk tidak lebih dari 28 unit.
> **Catatan reproduksibilitas:** hasil Solver dapat berubah bergantung pada solusi awal, constraint, pengaturan penghentian, dan versi Excel. Selalu validasi hasil akhir terhadap constraint model, jangan hanya mengandalkan nilai objective.
 
---
 
## Referensi
 
- [Microsoft Support: Define and solve a problem by using Solver](https://support.microsoft.com/en-us/excel/define-and-solve-a-problem-by-using-solver)
> Soal berasal dari Hackathon Excel Oktober 2026 (DQLab x UjiKompetensi.com) dan digunakan untuk tujuan pembelajaran dan portofolio.


> Soal berasal dari Hackathon Excel Oktober 2026 (DQLab x UjiKompetensi.com) dan digunakan untuk tujuan pembelajaran dan portofolio.
