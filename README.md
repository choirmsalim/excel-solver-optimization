# Excel Optimization Hackathon — Nutrition Planning & Vehicle Routing

**Excel DQLab Hackathon | Operations Research**

This project uses Microsoft Excel and Solver to model two constrained optimization problems:

- **Nutrition Budget (Linear Programming):** build a food plan that meets protein and fiber targets within a budget and ingredient quantity limits.
- **Vehicle Routing Problem (VRP):** assign customer locations to truck routes while respecting vehicle capacity and minimizing total travel distance.

## Overview

The project demonstrates how spreadsheet formulas, decision variables, objective functions, and constraints can turn raw inputs into practical planning models.
| | Kasus 1: Perencanaan Gizi | Kasus 2: Vehicle Routing Problem |
|---|---|---|
| **Masalah** | Memenuhi kebutuhan protein dan serat keluarga dengan biaya belanja serendah mungkin | Menentukan rute 10 truk untuk mengirim barang ke 50 pelanggan dengan jarak total sesingkat mungkin |
| **Tipe optimasi** | Integer Linear Programming | Combinatorial optimization (routing) |
| **Keputusan** | Jumlah unit tiap bahan makanan, dan menu harian | Urutan kunjungan pelanggan tiap truk |
| **Tujuan** | Minimalkan biaya | Minimalkan total jarak tempuh |
| **Hasil** | Biaya **Rp1.910.000** (hemat Rp90.000 dari anggaran) | Total jarak **1,153.24 km**, 259 unit terkirim |
 
---


## Dataset

### Kasus 1: Perencanaan Gizi (`excelhack03-perencanaan-gizi-dqlab.xlsx`)
 
| Parameter | Nilai |
|---|---|
| Anggaran belanja maksimum | Rp2.000.000 |
| Target protein / serat | ≥ 2.000 g / ≥ 500 g |
| Bahan | Telur, Tempe, Ayam, Bayam, Oat (harga, protein, serat, batas beli min–maks per bahan) |
| Kebutuhan harian (berat badan 60 kg) | Protein 0.8–1 g/kg (48–60 g), serat 25–30 g |
 
### Kasus 2: VRP (`excelhack03-perencanaan-routing-dqlab.xlsx`)
 
| Parameter | Nilai |
|---|---|
| Depot | 1 gudang pusat (ID 0) |
| Pelanggan | 50 lokasi, total demand **259 unit** |
| Armada | 10 truk identik, kapasitas **28 unit** |
| Jarak | Matriks jarak 51 × 51 (km) |

*Dataset source: the supplied DQLab hackathon workbooks. No external dataset is added.*
---


## Tools & Methods

- **Microsoft Excel** — data organization, formula-based calculations, and result summaries.
- **Excel Solver Add-in** — objective and constraint optimization.
- **Simplex LP** — linear optimization for the nutrition model.
- **Evolutionary Algorithm** — search method for the combinatorial VRP.

### Spreadsheet formulas

| Formula | Role in the project |
|---|---|
| `INDEX` | Retrieve customer demand and distances from lookup tables / the distance matrix. |
| `IF/IFS` | Handle route separators, depot mapping, and empty route positions. |
| `COUNTIF` | Identify route separators and calculate route numbering. |
| `COUNTIFS` | Count route positions while applying multiple criteria. |
| `SUMPRODUCT` | Aggregate nutrition totals and truck demand. |
| `SUM` / `SUMIF` | Calculate totals and route-level summaries. |


## Workflow

1. **Prepare inputs** — review ingredient costs, nutrition values, customer demand, distance matrix, truck count, and capacity.
2. **Define decision variables** — food quantities for the nutrition model; encoded customer order and route separators for the VRP.
3. **Build formula logic** — calculate food cost, protein, fiber, route distance, and demand per truck.
4. **Configure Solver** — select the objective, variable cells, constraints, and appropriate solving method.
5. **Check feasibility** — confirm that budget and nutrition targets are met, every customer is served once, and no truck exceeds capacity.
6. **Review results** — inspect the food plan and the route summary after Solver completes.

## Hasil
 
### Kasus 1: Perencanaan Gizi
 
**Tugas 1: pembelian optimal**
 
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
**Tugas 2: menu harian**
 
| Bahan | Per hari (unit) |
|---|---|
| Tempe | 2.946 |
| Bayam | 0.357 |
| Telur, Ayam, Oat | 0 |
 
- Protein **60 g/hari** (batas maksimum), serat **25 g/hari** (batas minimum).
- Biaya **Rp47.768/hari**, atau **Rp286.607** untuk 6 hari. Nilai ini sama dengan solusi Linear Programming.

### Kasus 2: Vehicle Routing Problem
 
| Truk | Rute | Jarak (km) | Muatan |
|---|---|---|---|
| 1 | 0 → 36 → 23 → 9 → 10 → 37 → 38 → 0 | 146.86 | 28 |
| 2 | 0 → 20 → 13 → 33 → 5 → 34 → 45 → 1 → 0 | 125.46 | 28 |
| 3 | 0 → 49 → 47 → 11 → 39 → 0 | 72.97 | 18 |
| 4 | 0 → 28 → 40 → 16 → 25 → 0 | 110.37 | 26 |
| 5 | 0 → 15 → 14 → 44 → 6 → 46 → 0 | 132.04 | 26 |
| 6 | 0 → 26 → 12 → 7 → 42 → 50 → 0 | 124.80 | 28 |
| 7 | 0 → 22 → 29 → 41 → 0 | 67.71 | 27 |
| 8 | 0 → 3 → 21 → 4 → 18 → 24 → 0 | 112.75 | 28 |
| 9 | 0 → 43 → 48 → 32 → 8 → 0 | 105.22 | 23 |
| 10 | 0 → 30 → 35 → 2 → 27 → 19 → 31 → 17 → 0 | 155.06 | 27 |
| **Total** | | **1,153.24** | **259** |
 
**Validasi constraint:**
- Semua 50 pelanggan dikunjungi tepat satu kali oleh satu truk.
- Setiap truk berangkat dan kembali ke depot.
- Muatan maksimum per truk 28 unit (tidak ada yang melebihi).
- Total demand 259 unit, sesuai total permintaan.
**Catatan pengembangan:** sebagai pembanding, solver routing khusus (Google OR-Tools, 120 detik) menemukan total jarak sekitar **1,068.8 km**, sekitar 7% lebih pendek. Solver Evolutionary di Excel menghasilkan solusi valid yang baik, tetapi belum tentu mendekati optimum. Pengembangan berikutnya: jalankan Solver beberapa kali dengan *random seed* berbeda, tambah waktu iterasi, atau mulai dari solusi awal berbasis heuristik (mis. *nearest neighbor*).
 
---
 
## Struktur Repository
 
```
excel-solver-optimization/
├── excel/
│   ├── excelhack03-perencanaan-gizi-dqlab.xlsx
│   └── excelhack03-perencanaan-routing-dqlab.xlsx
├── docs/
│   ├── soal-perencanaan-gizi.pdf
│   └── soal-vehicle-routing-problem.pdf
└── README.md
```
 
---

## How to Run

### Requirements

- Microsoft Excel desktop
- Excel **Solver Add-in** enabled
- Both `.xlsx` workbooks from this project

To enable Solver, go to **File → Options → Add-ins → Excel Add-ins → Go**, then check **Solver Add-in**.

### A. Run the Nutrition Budget model

1. Open `excelhack03-perencanaan-gizi-dqlab.xlsx` and select the `Bahan dan Menu` sheet.
2. Open **Data → Solver**.
3. Configure the model:
   - **Set Objective:** `H12` (total cost), select **Min**.
   - **By Changing Variable Cells:** `G7:G11` (planned ingredient quantities).
   - **Constraints:** `H12 <= B2`; `E2 >= B3`; `E3 >= B4`; `G7:G11 >= E7:E11`; and `G7:G11 <= F7:F11`.
   - **Select a Solving Method:** **Simplex LP**.
4. Click **Solve**, retain the solution if the constraints are satisfied, and review the updated cost and nutrition totals.

### B. Run the Vehicle Routing model

1. Open `excelhack03-perencanaan-routing-dqlab.xlsx`.
2. Open **Data → Solver**.
3. Configure the model:
   - **Set Objective:** `Solution!L13` (total route distance), select **Min**.
   - **By Changing Variable Cells:** `Helper!B3:B62` (encoded route sequence).
   - **Select a Solving Method:** **Evolutionary**.
   - Preserve the workbook's route encoding: customer IDs are `1–50`; values `51–60` are route separators / depot markers. For a fresh Solver setup, constrain `Helper!B3:B62` to integers from `1` to `60` and apply the **dif (alldifferent)** constraint to `Helper!B3:B62` so each ID is used once.
   - Add the capacity constraint `Helper!L3:L12 <= 28`. Keep each route within the available `Solution` layout (up to 8 customer stops per truck).
4. Click **Solve**. Review the `Solution` sheet and verify that every customer is included once, each route starts/ends at the depot as intended, and each truck's demand is no greater than 28 units.

> **Reproducibility note:** Solver outcomes can change with the initial solution, constraints, stopping settings, and Excel version. Always validate the final result against the model's constraints instead of relying only on the objective value.

## Reference

- [Microsoft Support: Define and solve a problem by using Solver](https://support.microsoft.com/en-us/excel/define-and-solve-a-problem-by-using-solver)

> Soal berasal dari Hackathon Excel Oktober 2026 (DQLab x UjiKompetensi.com) dan digunakan untuk tujuan pembelajaran dan portofolio.
