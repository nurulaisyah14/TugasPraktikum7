# Tugas Praktikum { Pertemuan ke 15 } <img src=https://logos-download.com/wp-content/uploads/2016/05/MySQL_logo_logotype.png width="130px" >


|**Nama**|**NIM**|**Kelas**|**Matkul**|
|----|---|-----|------|
|Nurul Aisyah|312310476|TI.22.A.5|Basis Data|

# Studi Kasus : Sub Query

## Soal Praktikum 7
![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/9d7470f2-c1c9-4cb4-bddd-dd6974d2bf49)
> **Keterangan** : Terdapat 5 tabel yang terdiri dari tabel Perusahaan, Departemen, Karyawan, Project dan Project Detail.

### *Script SQL berdasarkan Tabel Perusahaan*
```sql
CREATE TABLE Perusahaan(
id_p VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
alamat VARCHAR(45) DEFAULT NULL
);

INSERT INTO Perusahaan (id_p, nama, alamat) VALUES
('P01', 'Kantor Pusat', NULL),
('P02', 'Cabang Bekasi', NULL);
SELECT * FROM Perusahaan;
```
#### *Output Tabel Perusahaan :*
![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/a29bbcb5-ae88-4060-9269-7c18b6dab2c8)



### *Script SQL berdasarkan Tabel Departemen*
```sql
CREATE TABLE Departemen(
id_dept VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
id_p VARCHAR(10) NOT NULL,
manajer_nik VARCHAR(10) DEFAULT NULL
);

INSERT INTO Departemen (id_dept, nama, id_p, manajer_nik) VALUES
('D01', 'Produksi', 'P02', 'N01'),
('D02', 'Marketing', 'P01', 'N03'),
('D03', 'RnD', 'P02', NULL),
('D04', 'Logistik', 'P02', NULL);
SELECT * FROM Departemen;
```
#### *Output Tabel Departemen :*
![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/c5cf87eb-4355-4890-8b46-ef322e7f5072)



### *Script SQL berdasarkan Tabel Karyawan*
```sql
CREATE TABLE Karyawan(
nik VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
id_dept VARCHAR(10) NOT NULL,
sup_nik VARCHAR(10) DEFAULT NULL,
gaji_pokok INT
);

INSERT INTO Karyawan (nik, nama, id_dept, sup_nik, gaji_pokok) VALUES
('N01', 'Ari', 'D01', NULL, 2000000),
('N02', 'Dina', 'D01', NULL, 2500000),
('N03', 'Rika', 'D03', NULL, 2400000),
('N04', 'Ratih', 'D01', 'N01', 3000000),
('N05', 'Riko', 'D01', 'N01', 2800000),
('N06', 'Dani', 'D02', NULL, 2100000),
('N07', 'Anis', 'D02', 'N06', 5000000),
('N08', 'Dika', 'D02', 'N06', 4000000),
('N09', 'Raka', 'D03', 'N06', 2000000);
SELECT * FROM Karyawan;
```
#### *Output Tabel Karyawan :*
![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/2eca26ed-5473-421d-9500-1a6330a59481)



### *Script SQL berdasarkan Tabel Project*
```sql
CREATE TABLE Project(
id_proj VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
tgl_mulai DATETIME,
tgl_selesai DATETIME,
status TINYINT(1)
);

INSERT INTO Project (id_proj, nama, tgl_mulai, tgl_selesai, status) VALUES
('PJ01', 'A', '2019-01-10', '2019-03-10', '1'),
('PJ02', 'B', '2019-02-15', '2019-04-10', '1'),
('PJ03', 'C', '2019-03-21', '2019-05-10', '1');
SELECT * FROM Project;
```
#### *Output Tabel Project :*
![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/69d5b742-dfdf-499a-bd82-bf3317dec4f2)



### *Script SQL berdasarkan Tabel Project Detail*
```sql
CREATE TABLE Project_detail(
id_proj VARCHAR(10) NOT NULL,
nik VARCHAR(10) NOT NULL
);

INSERT INTO Project_detail (id_proj, nik) VALUES
('PJ01', 'N01'),
('PJ01', 'N02'),
('PJ01', 'N03'),
('PJ01', 'N04'),
('PJ01', 'N05'),
('PJ01', 'N07'),
('PJ01', 'N08'),
('PJ02', 'N01'),
('PJ02', 'N03'),
('PJ02', 'N05'),
('PJ03', 'N03'),
('PJ03', 'N07'),
('PJ03', 'N08');
SELECT * FROM Project_detail;
```
#### *Output Tabel Project Detail :*
![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/07f6c121-ad58-4661-ab6c-c54a08b10c03)





## Latihan Praktikum 7

### 1. Tampilkan data karyawan yang bekerja pada departemen yang sama dengan karyawan yang bernama Dika
**Script :**

```sql
SELECT nik, nama, id_dept FROM Karyawan WHERE id_dept = (SELECT id_dept FROM Karyawan WHERE nama = 'Dika');
```

**Output :**

![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/b2a8d5bb-62e8-4e95-b9d7-f409d754fb54)




### 2. Tampilkan data karyawan yang gajinya lebih besar dari rata-rata gaji semua karyawan. Urutkan menurun berdasarkan besaran gaji
**Script :**

```sql
SELECT nik, nama, id_dept, gaji_pokok FROM karyawan WHERE gaji_pokok > (SELECT AVG(gaji_pokok) FROM Karyawan) ORDER BY gaji_pokok DESC;
```

**Output :**

![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/cd873162-6687-461d-a27a-9a1c26db7c48)



### 3. Tampilkan nik dan nama karyawan untuk semua karyawan yang bekerja di departmen yang sama dengan karyawan dengan nama yang mengandung huruf 'K'.
**Script :**

```sql
SELECT nik, nama FROM Karyawan WHERE id_dept IN (SELECT id_dept FROM Karyawan WHERE nama LIKE '%K%');
```

**Output :**

![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/3c2ebc12-06ec-49d5-a361-7914a8b510c2)



### 4. Tampilkan data karyawan yang bekerja pada departemen yang ada di Kantor pusat.
**Script :**

```sql
SELECT karyawan.nik, karyawan.nama, karyawan.id_dept FROM karyawan JOIN departemen ON karyawan.id_dept = departemen.id_dept WHERE departemen.id_p = 'P01';
```

**Output :**

![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/3b7e9e5e-14ef-4e3c-9d7f-cb38ae5b05d3)


### 5. Tampilkan nik dan nama karyawan untuk semua karyawan yang bekerja di departmen yang sama dengan karyawan dengan nama yang mengandung huruf 'K' dan yang gajinya lebih besar dari rata-rata gaji semua karyawan
**Script :**

```sql
SELECT DISTINCT k1.nik, k1.nama FROM karyawan k1 JOIN karyawan k2 ON k1.id_dept = k2.id_dept WHERE k1.gaji_pokok > (SELECT AVG(gaji_pokok) FROM karyawan WHERE nama LIKE '%K%');
```

**Output :**

![image](https://github.com/nurulaisyah14/TugasPraktikum7/assets/148174512/83373363-8891-40db-9e33-7063b0bdcbe3)

