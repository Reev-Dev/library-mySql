# Buat Database Perpustakaan Sederhana

Pembuatan Database Perpustakaan sederhana mencakup informasi tentang buku, anggota, peminjaman, pengembalian, dan inventarisasi perpustakaan.
Sebelumnya masuk dulu ke mysql menggunakan terminal dengan mengetikkan :
mysql -u root -p

kemudian masukkan password kosong saja (lansgung enter)

## Buat Database Baru

Query :
CREATE DATABASE library;

Kemudian kita masuk ke databasenya dengan menggunakan query :
USE library;

## Table

Kita buat table baru :
CREATE TABLE buku (
    id_buku INT AUTO_INCREMENT PRIMARY KEY,
    judul VARCHAR(255) NOT NULL,
    pengarang VARCHAR(255) NOT NULL,
    tahun_terbit YEAR,
    stok INT NOT NULL
);

CREATE TABLE anggota (
    id_anggota INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(255) NOT NULL,
    alamat TEXT,
    tanggal_lahir DATE
);

CREATE TABLE peminjaman (
    id_peminjaman INT AUTO_INCREMENT PRIMARY KEY,
    id_buku INT NOT NULL,
    id_anggota INT NOT NULL,
    tanggal_pinjam DATE NOT NULL,
    tanggal_kembali DATE,
    FOREIGN KEY (id_buku) REFERENCES buku(id_buku) ON DELETE CASCADE,
    FOREIGN KEY (id_anggota) REFERENCES anggota(id_anggota) ON DELETE CASCADE
);

CREATE TABLE pengembalian (
    id_pengembalian INT AUTO_INCREMENT PRIMARY KEY,
    id_peminjaman INT NOT NULL,
    tanggal_kembali DATE NOT NULL,
    denda DECIMAL(10,2),
    FOREIGN KEY (id_peminjaman) REFERENCES peminjaman(id_peminjaman) ON DELETE CASCADE
);

CREATE TABLE inventarisasi (
    id_invent INT AUTO_INCREMENT PRIMARY KEY,
    id_buku INT NOT NULL,
    kondisi VARCHAR(50) NOT NULL,
    keterangan TEXT,
    FOREIGN KEY (id_buku) REFERENCES buku(id_buku) ON DELETE CASCADE
);

## Tambahkan Data

INSERT INTO anggota
    (nama, alamat,tanggal_lahir)
    VALUES
    ("Adli Rayhan", "Tanjung Priuk", "2008-01-29"),
    ("Raqi Ahmad", "Bojong Gede", "2007-02-13"),
    ("Radid Aditia", "Lampung Selatan", "2007-01-30")
;

INSERT INTO buku
    (judul, pengarang, tahun_terbit, stok)
    VALUES
    ('Bumi', 'Tere Liye', 2014, 321),
    ('Ily', 'Tere Liye', 2023, 120),
    ('Parable', 'Brian Khrisna', 2021, 209),
    ('9 dari Nadira', 'Leila S.Chudori', 2009, 64),
    ('My Hero Academia', 'Kohei Horikoshi', 2013, 123)
;

select * from buku;

INSERT INTO peminjaman
    (id_buku, id_anggota, tanggal_pinjam, tanggal_kembali)
    VALUES
    (3, 1, '2024-02-06', '2024-02-20'),
    (2, 2, '2023-12-12', '2024-01-12'),
    (1, 3, '2023-11-20', '2024-01-06'),
    (5, 1, '2024-02-12', '2024-02-26')
;

INSERT INTO Pengembalian
    (id_peminjaman, tanggal_kembali, denda)
    VALUES
    (1, '2024-02-20', NULL),
    (2, '2024-01-12', NULL),
    (3, '2024-01-13', 10.00),
    (4, '2024-02-26', NULL)
;

## Gabungkan Data

--Show ibfk
SHOW CREATE TABLE peminjaman;

-- Join
SELECT
    peminjaman.id_peminjaman,
    anggota.nama,
    buku.judul,
    peminjaman.tanggal_pinjam,
    peminjaman.tanggal_kembali
    FROM
    peminjaman
    JOIN
    anggota ON peminjaman.id_anggota = anggota.id_anggota
    JOIN
    buku ON peminjaman.id_buku = buku.id_buku
;

-- Left Join
SELECT
    buku.judul,
    anggota.nama
    FROM
    buku
    LEFT JOIN
    peminjaman ON buku.id_buku = peminjaman.id_buku
    LEFT JOIN
    anggota ON peminjaman.id_anggota = anggota.id_anggota
;

-- Right Join
SELECT
    buku.judul,
    anggota.nama,
    peminjaman.tanggal_pinjam
FROM
    buku
RIGHT JOIN
    peminjaman ON buku.id_buku = peminjaman.id_buku
RIGHT JOIN
    anggota ON peminjaman.id_anggota = anggota.id_anggota;
