### Perbedaan `new()` dan `build()`

Fungsi `new()` menggunakan `assert!(size > 0)` yang akan
menyebabkan program **panic** (crash) jika kondisi tidak
terpenuhi. Ini berarti caller tidak punya kesempatan untuk
menangani error jadi program langsung berhenti.

Sebaliknya, fungsi `build()` mengembalikan `Result<ThreadPool, 
PoolCreationError>`. Ini mengikuti idiom Rust yang menggunakan
`Result` untuk operasi yang berpotensi gagal, sehingga **caller
memiliki kontrol penuh** atas apa yang terjadi jika pembuatan
ThreadPool gagal — bisa fallback, log error, atau exit secara
graceful.

Konvensi di Rust adalah: gunakan `new()` untuk constructor yang
**tidak mungkin gagal** (seperti `Vec::new()`), dan gunakan
`build()` atau nama lain yang return `Result` untuk constructor
yang **mungkin gagal**. Pola ini membuat error handling menjadi
**eksplisit dan tidak bisa diabaikan** oleh caller, yang
merupakan salah satu filosofi utama Rust dalam menulis kode
yang aman dan reliable.

Dari eksperimen ini, saya menyimpulkan bahwa `build()` adalah
pilihan yang **lebih baik untuk production code** karena
memberikan fleksibilitas penanganan error, sementara `new()`
lebih cocok untuk kasus yang secara logika tidak mungkin gagal.