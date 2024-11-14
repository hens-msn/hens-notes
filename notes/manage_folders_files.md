# Panduan Mengelola File dan Folder (Linux & PowerShell)

## 1. Menghapus Folder Tertentu

### Linux:
Command:
```bash
# Menghapus folder dengan nama spesifik seperti "node_modules" di semua subdirektori
find /path/to/start -type d -name "node_modules" -exec rm -rf {} +

# Menghapus beberapa folder dengan nama spesifik (contoh: "build", "dist", atau "temp")
find /path/to/start -type d \( -name "build" -o -name "dist" -o -name "temp" \) -exec rm -rf {} +

# Menghapus folder dengan pola nama tertentu (contoh: semua folder yang diawali "backup")
find /path/to/start -type d -name "backup*" -exec rm -rf {} +
```
**Output**: Tidak ada output jika berhasil, kecuali ada error seperti "Permission denied".

### PowerShell:
Command:
```powershell
# Menghapus folder dengan nama spesifik seperti "node_modules" di semua subdirektori
Get-ChildItem -Path "C:\path\to\start" -Directory -Recurse -Filter "node_modules" | Remove-Item -Recurse -Force

# Menghapus beberapa folder dengan nama spesifik (contoh: "build", "dist", atau "temp")
Get-ChildItem -Path "C:\path\to\start" -Directory -Recurse | Where-Object { $_.Name -in @("build", "dist", "temp") } | Remove-Item -Recurse -Force

# Menghapus folder dengan pola nama tertentu (contoh: semua folder yang diawali "backup")
Get-ChildItem -Path "C:\path\to\start" -Directory -Recurse | Where-Object { $_.Name -like "backup*" } | Remove-Item -Recurse -Force
```
**Output**:
Jika berhasil: Tidak ada output.
Jika gagal: Pesan error seperti "Access to the path is denied".

---

## 2. Menghapus File Berdasarkan Pola Tertentu

### Linux:
Command:
```bash
# Menghapus file spesifik berdasarkan nama
find /path/to/start -type f -name "package-lock.json" -exec rm -f {} +

# Menghapus semua file dengan pola tertentu, seperti semua file log
find /path/to/start -type f -name "*.log" -exec rm -f {} +

# Menghapus file dengan pola kompleks (misalnya semua file JSON atau log kecuali tertentu)
find /path/to/start -type f \( -name "*.json" -o -name "*.log" \) ! -name "important.log" -exec rm -f {} +
```
**Output**: Tidak ada output jika berhasil.

### PowerShell:
Command:
```powershell
# Menghapus file spesifik berdasarkan nama
Get-ChildItem -Path "C:\path\to\start" -Recurse -Filter "package-lock.json" | Remove-Item -Force

# Menghapus semua file dengan pola tertentu, seperti semua file log
Get-ChildItem -Path "C:\path\to\start" -Recurse -Filter "*.log" | Remove-Item -Force

# Menghapus file dengan pola kompleks (misalnya semua file JSON atau log kecuali tertentu)
Get-ChildItem -Path "C:\path\to\start" -Recurse | Where-Object { $_.Name -match '\.(json|log)$' -and $_.Name -ne 'important.log' } | Remove-Item -Force
```
**Output**:
Jika berhasil: Tidak ada output.
Jika gagal: Pesan error seperti "Access to the path is denied".

---

## 3. Menampilkan Folder Terbesar Berdasarkan Ukuran

### Linux:
Command:
```bash
du -ah /path/to/folder | sort -rh | head -n 10
```
**Output**: Daftar folder/file terbesar, misalnya:
```
5.0G   /path/to/folder1
1.2G   /path/to/folder2
...
```

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\folder" -Recurse | Sort-Object Length -Descending | Select-Object -First 10
```
**Output**: Daftar folder/file terbesar, misalnya:
```
    Directory: C:\path\to\folder

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/13/2024   12:34 PM   5368709120 file1
-a---          11/13/2024   12:35 PM   1342177280 file2
...
```

---

## 4. Melihat File Terbesar

### Linux:
Command:
```bash
find /path/to/folder -type f -exec du -h {} + | sort -rh | head -n 1
```
**Output**: Nama dan ukuran file terbesar, misalnya:
```
5.0G   /path/to/file
```

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\folder" -File -Recurse | Sort-Object Length -Descending | Select-Object -First 1
```
**Output**: Informasi file terbesar, misalnya:
```
    Directory: C:\path\to\folder

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/13/2024   12:34 PM   5368709120 file1
```

---

## 5. Menghitung Total Ukuran File Berdasarkan Ekstensi

### Linux:
Command:
```bash
find /path/to/folder -type f -name "*.ext" -exec du -ch {} + | grep total
```
**Output**: Total ukuran file dengan ekstensi tertentu, misalnya:
```
1.2G   total
```

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\folder" -Recurse -Include "*.ext" | Measure-Object -Property Length -Sum | ForEach-Object { "Total Size: $([math]::Round($_.Sum / 1GB, 2)) GB" }
```
**Output**: Total ukuran file dengan ekstensi tertentu, misalnya:
```
Total Size: 1.2 GB
```

---

## 6. Menghapus File Berdasarkan Umur

### Linux:
Command:
```bash
find /path/to/folder -type f -mtime +30 -delete
```
**Output**: Tidak ada output jika berhasil.

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\folder" -File -Recurse | Where-Object { $_.LastWriteTime -lt (Get-Date).AddDays(-30) } | Remove-Item -Force
```
**Output**: Tidak ada output jika berhasil.

---

## 7. Mencari File dengan Pola Nama Tertentu

### Linux:
Command:
```bash
find /path/to/start -type f -name "*.log"
```
**Output**: Daftar file yang cocok dengan pola, misalnya:
```
/path/to/start/file1.log
/path/to/start/subdir/file2.log
```

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\start" -Recurse -Filter "*.log"
```
**Output**: Daftar file yang cocok dengan pola, misalnya:
```
    Directory: C:\path\to\start

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/13/2024   12:34 PM       1024 file1.log
-a---          11/13/2024   12:35 PM       2048 file2.log
```

---

## 8. Menghitung Jumlah File Berdasarkan Ekstensi

### Linux:
Command:
```bash
find /path/to/folder -type f -name "*.ext" | wc -l
```
**Output**: Jumlah file dengan ekstensi tertentu, misalnya:
```
42
```

### PowerShell:
Command:
```powershell
(Get-ChildItem -Path "C:\path\to\folder" -Recurse -Include "*.ext").Count
```
**Output**: Jumlah file dengan ekstensi tertentu, misalnya:
```
42
```

---

## 9. Menyalin Semua File dengan Ekstensi Tertentu ke Folder Lain

### Linux:
Command:
```bash
find /path/to/source -type f -name "*.ext" -exec cp {} /path/to/destination \;
```
**Output**: Tidak ada output jika berhasil.

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\source" -Recurse -Include "*.ext" | ForEach-Object { Copy-Item $_.FullName -Destination "C:\path\to\destination" }
```
**Output**: Tidak ada output jika berhasil.

---

## 10. Menemukan File Duplikat Berdasarkan Ukuran dan Nama

### Linux:
Command:
```bash
find /path/to/folder -type f -exec md5sum {} + | sort | uniq -w32 -d
```
**Output**: Daftar file duplikat, misalnya:
```
5d41402abc4b2a76b9719d911017c592  /path/to/folder/file1.txt
5d41402abc4b2a76b9719d911017c592  /path/to/folder/file2.txt
```

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\folder" -File -Recurse | Group-Object Length, Name | Where-Object { $_.Count -gt 1 } | ForEach-Object { $_.Group }
```
**Output**: Daftar file duplikat, misalnya:
```
    Directory: C:\path\to\folder

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/13/2024   12:34 PM       1024 file1.txt
-a---          11/13/2024   12:35 PM       1024 file2.txt
```

---

## 11. Backup Folder ke File ZIP

### Linux:
Command:
```bash
zip -r /path/to/backup.zip /path/to/folder
```
**Output**: File ZIP akan dibuat di lokasi yang ditentukan.

### PowerShell:
Command:
```powershell
Compress-Archive -Path "C:\path\to\folder" -DestinationPath "C:\path\to\backup.zip"
```
**Output**: File ZIP akan dibuat di lokasi yang ditentukan.

---

## 12. Menampilkan File yang Terakhir Diubah

### Linux:
Command:
```bash
find /path/to/folder -type f -printf '%T@ %p\n' | sort -n | tail -1 | cut -d' ' -f2-
```
**Output**: File yang terakhir diubah, misalnya:
```
/path/to/folder/updated_file.txt
```

### PowerShell:
Command:
```powershell
Get-ChildItem -Path "C:\path\to\folder" -File -Recurse | Sort-Object LastWriteTime -Descending | Select-Object -First 1
```
**Output**: Informasi file yang terakhir diubah, misalnya:
```
    Directory: C:\path\to\folder

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/14/2024   12:34 PM       1024 updated_file.txt
```

---

**Catatan**: Pastikan menjalankan terminal dengan hak akses administrator jika diperlukan. Selalu berhati-hati saat menggunakan perintah yang memodifikasi atau menghapus file.

