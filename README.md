# perpustakaan
### **1. Kelas Buku**

**Penjelasan**  
Kelas ini digunakan untuk merepresentasikan setiap buku yang ada di perpustakaan. Setiap buku memiliki beberapa atribut penting:  
- **`id_buku`**: Identifikasi unik untuk buku, misalnya "1111".  
- **`judul`**: Nama atau judul buku, seperti "Bahasa Pemrograman".  
- **`penulis`**: Nama penulis buku.  
- **`jumlah`**: Jumlah buku yang tersedia untuk dipinjam.

Kelas ini memungkinkan kita untuk membuat objek buku secara terorganisir.

**Kode**  
```python
class Buku:
    def __init__(self, id_buku, judul, penulis, jumlah):
        self.id_buku = id_buku
        self.judul = judul
        self.penulis = penulis
        self.jumlah = jumlah
```

**Contoh Penggunaan**  
Misalkan ada sebuah buku yang berjudul *"Bahasa Pemrograman"* karya *"Yudha"* dengan ID "1111" dan tersedia sebanyak 5 eksemplar. Buku ini dapat didefinisikan sebagai berikut:  

```python
buku = Buku("1111", "Bahasa Pemrograman", "Yudha", 5)
```

---

### **2. Kelas Perpustakaan**

**Penjelasan**  
Kelas `Perpustakaan` berfungsi untuk mengelola semua aktivitas perpustakaan. Kelas ini terdiri dari dua atribut utama:  
- **`buku_tersedia`**: Dictionary (kamus) yang menyimpan buku yang ada di perpustakaan. Kunci dictionary adalah ID buku, dan nilai adalah objek dari kelas `Buku`.  
- **`buku_dipinjam`**: Dictionary yang menyimpan daftar buku yang sedang dipinjam. Kunci dictionary adalah nama peminjam, dan nilainya adalah daftar judul buku yang dipinjam.

**Kode**  
```python
class Perpustakaan:
    def __init__(self):
        self.buku_tersedia = {}
        self.buku_dipinjam = {}
```

**Fungsi Utama di Kelas Perpustakaan**  
1. **`tambah_buku`**: Menambah buku baru ke koleksi perpustakaan. Jika buku sudah ada, hanya jumlahnya yang diperbarui.  
2. **`pinjam_buku`**: Memproses peminjaman buku oleh pengguna. Sistem memeriksa ketersediaan buku sebelum meminjamkan.  
3. **`kembalikan_buku`**: Memproses pengembalian buku. Buku yang dikembalikan akan ditambahkan kembali ke stok buku yang tersedia.  
4. **`daftar_buku`**: Menampilkan semua buku yang tersedia di perpustakaan beserta detailnya.

---

### **3. Fungsi `tambah_buku`**

**Penjelasan**  
Fungsi ini menambahkan buku baru ke koleksi perpustakaan. Jika buku dengan ID yang sama sudah ada, maka jumlah buku akan diperbarui.

**Kode**  
```python
def tambah_buku(self, buku):
    if buku.id_buku in self.buku_tersedia:
        self.buku_tersedia[buku.id_buku].jumlah += buku.jumlah
    else:
        self.buku_tersedia[buku.id_buku] = buku
    print(f"Buku '{buku.judul}' berhasil ditambahkan.")
```

**Proses Kerja**  
1. Memeriksa apakah `id_buku` sudah ada di koleksi.  
2. Jika sudah ada, jumlah buku ditambah. Jika belum ada, buku baru ditambahkan ke `buku_tersedia`.

**Contoh Penggunaan**  
Misalkan kita ingin menambahkan buku berjudul *"Python Lanjutan"* sebanyak 3 eksemplar:  
```python
buku_baru = Buku("2222", "Python Lanjutan", "Dafa", 3)
perpustakaan.tambah_buku(buku_baru)
```

---

### **4. Fungsi `pinjam_buku`**

**Penjelasan**  
Fungsi ini memungkinkan pengguna untuk meminjam buku. Sistem akan memeriksa apakah buku tersedia sebelum mengurangi jumlahnya.

**Kode**  
```python
def pinjam_buku(self, id_buku, nama_peminjam):
    if id_buku in self.buku_tersedia and self.buku_tersedia[id_buku].jumlah > 0:
        self.buku_tersedia[id_buku].jumlah -= 1
        if nama_peminjam not in self.buku_dipinjam:
            self.buku_dipinjam[nama_peminjam] = []
        self.buku_dipinjam[nama_peminjam].append(self.buku_tersedia[id_buku].judul)
        print(f"Buku '{self.buku_tersedia[id_buku].judul}' berhasil dipinjam oleh {nama_peminjam}.")
    else:
        print("Buku tidak tersedia untuk dipinjam.")
```

**Proses Kerja**  
1. Memeriksa apakah ID buku yang dimasukkan ada dalam koleksi dan jumlahnya lebih dari 0.  
2. Jika tersedia, jumlah buku dikurangi 1.  
3. Menambahkan buku tersebut ke daftar buku yang dipinjam oleh nama peminjam.

**Contoh Penggunaan**  
Misalkan *"Nabil"* ingin meminjam buku dengan ID *"1111"*:  
```python
perpustakaan.pinjam_buku("1111", "Nabil")
```

---

### **5. Fungsi `kembalikan_buku`**

**Penjelasan**  
Fungsi ini digunakan untuk memproses pengembalian buku yang telah dipinjam. Buku yang dikembalikan akan ditambahkan kembali ke stok buku yang tersedia.

**Kode**  
```python
def kembalikan_buku(self, nama_peminjam, judul_buku):
    if nama_peminjam in self.buku_dipinjam and judul_buku in self.buku_dipinjam[nama_peminjam]:
        self.buku_dipinjam[nama_peminjam].remove(judul_buku)
        for id_buku, buku in self.buku_tersedia.items():
            if buku.judul == judul_buku:
                buku.jumlah += 1
                break
        print(f"Buku '{judul_buku}' berhasil dikembalikan oleh {nama_peminjam}.")
    else:
        print("Peminjaman buku tidak ditemukan atau data salah.")
```

**Proses Kerja**  
1. Memeriksa apakah peminjam dan buku yang dikembalikan cocok.  
2. Menghapus buku dari daftar peminjaman peminjam.  
3. Menambahkan kembali stok buku.

**Contoh Penggunaan**  
Jika *"Nabil"* ingin mengembalikan buku *"Bahasa Pemrograman"*:  
```python
perpustakaan.kembalikan_buku("Nabil", "Bahasa Pemrograman")
```

---

### **6. Fungsi `daftar_buku`**

**Penjelasan**  
Fungsi ini digunakan untuk menampilkan daftar semua buku yang tersedia di perpustakaan, termasuk ID, judul, penulis, dan jumlahnya.

**Kode**  
```python
def daftar_buku(self):
    print("\nDaftar Buku Tersedia:")
    for buku in self.buku_tersedia.values():
        print(f"ID: {buku.id_buku}, Judul: {buku.judul}, Penulis: {buku.penulis}, Jumlah: {buku.jumlah}")
```

**Contoh Penggunaan**  
Untuk melihat semua buku di perpustakaan:  
```python
perpustakaan.daftar_buku()
```

---

### **7. Interaksi Pengguna**

**Penjelasan**  
Di bagian ini, sistem menampilkan menu pilihan untuk pengguna, seperti menambah buku, meminjam buku, mengembalikan buku, melihat daftar buku, atau keluar dari sistem.

---


# Hasil coding

### **Contoh Interaksi dengan Sistem**

**1. Menambahkan Buku ke Koleksi**  
Input pengguna:  
```plaintext
1
Masukkan ID Buku: 1111
Masukkan Judul Buku: bahasa pemrograman
Masukkan Penulis Buku: yudha
Masukkan Jumlah Buku: 5
```

Output sistem:  
```plaintext
Buku 'Bahasa Pemrograman' berhasil ditambahkan.
```

---

**2. Melihat Daftar Buku**  
Input pengguna:  
```plaintext
4
```

Output sistem:  
```plaintext
Daftar Buku Tersedia:
ID: 1111, Judul: Bahasa Pemrograman, Penulis: Yudha, Jumlah: 5
```

---

**3. Meminjam Buku**  
Input pengguna:  
```plaintext
2
Masukkan ID Buku yang Ingin Dipinjam: 1111
Masukkan Nama Peminjam: Nabil
```

Output sistem:  
```plaintext
Buku 'Bahasa Pemrograman' berhasil dipinjam oleh Nabil.
```

---

**4. Melihat Daftar Buku Setelah Peminjaman**  
Input pengguna:  
```plaintext
4
```

Output sistem:  
```plaintext
Daftar Buku Tersedia:
ID: 1111, Judul: Bahasa Pemrograman, Penulis: Yudha, Jumlah: 4
```

---

**5. Mengembalikan Buku**  
Input pengguna:  
```plaintext
3
Masukkan Nama Peminjam: Nabil
Masukkan Judul Buku yang Ingin Dikembalikan: Bahasa Pemrograman
```

Output sistem:  
```plaintext
Buku 'Pemrograman Python Dasar' berhasil dikembalikan oleh Nabil.
```

---

**6. Melihat Daftar Buku Setelah Pengembalian**  
Input pengguna:  
```plaintext
4
```

Output sistem:  
```plaintext
Daftar Buku Tersedia:
ID: 1111, Judul: Bahasa Pemrograman, Penulis: yudha, Jumlah: 5
```

---

**7. Keluar dari Sistem**  
Input pengguna:  
```plaintext
5
```

Output sistem:  
```plaintext
Terima kasih telah menggunakan sistem perpustakaan.
```

---

### **Catatan**

1. **Kondisi yang Ditangani**:  
   - Jika pengguna mencoba meminjam buku yang tidak tersedia, sistem akan memberikan pesan:  
     ```plaintext
     Buku tidak tersedia untuk dipinjam.
     ```

   - Jika pengguna mengembalikan buku yang tidak sesuai dengan data, sistem akan memberikan pesan:  
     ```plaintext
     Peminjaman buku tidak ditemukan atau data salah.
     ```

2. **Penggunaan Berkelanjutan**:  
   - Program akan terus berjalan hingga pengguna memilih opsi keluar (`5`).  

---
