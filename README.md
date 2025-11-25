# TUGAS2-PROJECT2-SUCI
# LANGKAH 1 
# Buat direktori baru

~~~
mkdir project2_backup
~~~
~~~
mkdir source_files backup logs
~~~
# Membuat file menggunakan metode perulangan (loop)
~~~
for i in {1..2) do echo " Laporan minggu ke-$i " > laporan$i.txt;done
~~~
~~~
for i in {1..2} do echo " catatan evaluasi bulan ke-$i " > file$i.pdf; done
~~~

# LANGKAH 2
# Membuat script
# Buat script backup.sh di dalam direktori project

~~~
nano backup.sh
~~~

~
# ISI SCRIPT
~~~
/bin/bash
# Konfigurasi
SOURCE_DIR="Source_files"
BACKUP_DIR="backup"
LOG_FILE="logs/backup.log"
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
BACKUP_NAME="backup_$TIMESTAMP.tar.gz"

mkdir -p "$BACKUP_DIR"
mkdir -p "$(dirname "$LOG_FILE")"

echo "=== Backup dijalankan pada $TIMESTAMP ===" >>$LOG_FILE

# Cari file berdasarkan eksistensi & tanggal modifikasi
FILES=$(find "$SOURCE_DIR" -name "*.txt" -o -name "*.pdf" -mtime -1)

if [ -z "$FILES" ]; then
    echo "Tidak ada file yang memenuhi kriteria." >> $LOG_FILE
    echo "Backup gagal: tidak ada file ditemukan."
    exit 1
fi

# Kompres file
tar -czvf "$BACKUP_DIR/$BACKUP_NAME" $FILES 2>> $LOG_FILE

# Cek apakah proses kompres berhasil
if [ $? -eq 0 ]; then
    echo "Backup berhasil: $BACKUP_NAME" >> $LOG_FILE
    echo "Backup berhasil!"
else
    echo "Backup gagal saat kompres." >> $LOG_FILE
    echo "Backup gagal!"
    exit 1
fi
~~~

# Membuat variabel konjungsi

~~~
SOURCE_DIR="source_files"
BACKUP_DIR="backup"
LOG_FILE="logs/backup.log"
~~~

# Pemindahan file backup ke direktori khusus dengan timestamp

~~~
TIMESTAMP=$(date "+%Y-%m-%d_%0H-%0M-%0S")
BACKUP_NAME="backup_$TIMESTAMP.tar.gz"
~~~

# log file yang mencatat setiap aktifitas backup

~~~
echo "=== Backup dijalankan pada $TIMESTAMP ===" >> $LOG_FILE
~~~

# mencatat saat tidak ada file yang di temukan
~~~
echo "Tidak ada file yang memenuhi kriteria." >> $LOG_FILE
echo "Backup gagal: tidak ada file ditemukan." >> $LOG_FILE
~~~

# mencatat jika log berhasil
~~~
echo "Backup berhasil. $BACKUP_NAME" >> $LOG_FILE
~~~
# mencatat jika log gagal
~~~
echo "Backup gagal saat kompres." >> $LOG_FILE
~~~
# Notifikasi sederhana jika backup berhasil atau gagal
~~~
if [ $? -eq 0 ]; then
    echo "Backup berhasil!"
else
    echo "Backup gagal!"
fi
~~~

# BERI HAK EKSEKUSI
~~~
chmod +x backup.sh
~~~
# EKSEKUSI SCRIPT YANG TELAH DIBUAT
~~~
./backup.sh
~~~

*Penjelasan

`#!/bin/bash` → menentukan interpreter bash untuk menjalankan script

SOURCE_DIR=..., BACKUP_DIR=..., LOG_FILE= → menyimpan konfigurasi folder sumber, folder backup, dan file log

date "+format" → membuat timestamp untuk nama backup

echo "..." >> $LOG_FILE → mencatat aktivitas backup ke file log

find source -name … -mtime -1→ mencari file txt/pdf yang diubah dalam 24 jam terakhir

if [ -z "$FILES" ] → mengecek apakah file ditemukan atau tidak

tar -czvf file.tar.gz files → mengkompres file menjadi arsip .tar.gz

2>> $LOG_FILE → mencatat error dari perintah ke file log

$? → membaca status keberhasilan perintah terakhir
