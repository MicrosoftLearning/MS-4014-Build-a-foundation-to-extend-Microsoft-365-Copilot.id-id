# Praktik Lab 1: Mengonfigurasi koneksi untuk konektor Microsoft Graph Anda

## Penyewa WWL - Ketentuan penggunaan

Jika Anda disediakan penyewa sebagai bagian dari penyelenggaraan pelatihan yang dipimpin oleh instruktur, perhatikan bahwa penyewa tersebut disediakan untuk mendukung praktik laboratorium dalam pelatihan yang dipimpin oleh instruktur.

Penyewa tidak boleh digunakan bersama atau digunakan untuk tujuan di luar praktik labor. Penyewa yang digunakan dalam kursus ini adalah penyewa percobaan dan tidak dapat digunakan atau diakses setelah kelas selesai dan tidak memenuhi syarat untuk diperpanjang.

Penyewa tidak boleh dikonversi ke langganan berbayar. Penyewa yang diperoleh sebagai bagian dari kursus ini tetap menjadi milik Microsoft Corporation dan kami berhak mendapatkan akses dan repositorinya kapan saja.

## Ringkasan

Di lab ini, Anda akan menggunakan pusat admin Microsoft 365 untuk membuat koneksi ke file pelanggan dengan menggunakan Microsoft File Share Connector.

Bayangkan Anda seorang manajer layanan pelanggan di Contoso, sebuah perusahaan berskala menengah. Tim Anda telah berjuang dengan waktu respons dan efisiensi keseluruhan saat menangani pertanyaan pelanggan. Anda memutuskan untuk memanfaatkan Pusat Admin Microsoft 365 untuk membuat koneksi FileShare yang memungkinkan agen layanan pelanggan Anda mengakses dan merujuk file pelanggan dengan cepat.

## Latihan 1: Mengonfigurasi koneksi di pusat admin Microsoft 365

### Masuk ke komputer virtual

Penyedia hosting lab Anda memberikan kata sandi untuk akun Administrator MOD, yang merupakan administrator penyewa default. Untuk tujuan keamanan, Microsoft telah mengonfigurasi penyewa uji coba Anda sehingga semua pengguna yang telah ditentukan harus mengubah kata sandi mereka saat masuk berikutnya. Masuk ke **LON-CL1** sebagai akun **Administrator** lokal yang dibuat oleh penyedia hosting lab Anda dengan kata sandi **Pa55w.rd**.

### Tugas 1: Memberikan izin di portal Azure

1. Masuk ke portal Azure **https://www.portal.azure.com** dan masuk dengan kredensial Administrasi Anda. Jangan simpan kata sandi, dan pilih **Ya** untuk **Tetap masuk?**.
2. Pada layar **Selamat datang di Microsoft Azure**, pilih **Batal**.
1. Pilih ikon menu tarik-turun di sisi kiri layar untuk menampilkan menu Portal. Pilih **Microsoft Entra ID -> Kelola -> Pendaftaran aplikasi**.
1. Pilih **Pendaftaran baru** dari bilah menu bagian atas. Halaman **Daftarkan aplikasi** akan tampil. Di halaman ini, berikan nama untuk aplikasi; mari kita beri nama aplikasi ini **Contoso Fileshare**. Biarkan opsi jenis akun default yang didukung sebagai **Akun dalam direktori organisasi ini saja (hanya Contoso - Penyewa tunggal).** Jangan pilih **Alihkan URI** opsional.
1. Pilih **Daftarkan**. Aplikasi Anda sudah dibuat, dan ID aplikasi diberikan padanya. Anda akan menggunakan informasi ini saat membuat Graph Connector Agent (GCA) dengan langkah-langkah berikut. Namun, sebelum kita membuat GCA, mari kita konfigurasikan pengaturan yang diperlukan.

Langkah selanjutnya adalah memberikan izin untuk agen konektor Graph di Portal Azure:

1. Pilih opsi **Kelola -> Izin API** dari menu sebelah kiri.
1. Pilih **Tambahkan izin -> Microsoft Graph -> Izin aplikasi**, dan berikan izin ke API berikut ini:

    - Direktori -> Direktori.Read.All
    - ExternalConnection -> ExternalConnection.ReadWrite.OwnedBy
    - ExternalItem -> ExternalItem.ReadWrite.All
      
1. Pilih **tombol Tambahkan izin akses**.
1. Pilih **Berikan persetujuan admin untuk Contoso** dan konfirmasi dengan memilih **Ya**.

**Catatan:** Jangan tutup tab browser Edge ini. Tugas berikut mengharuskan Anda menyalin dan menempelkan informasi dari portal Azure.

### Tugas 2: Instal GCA

1. Buka tab browser Microsoft Edge baru. Buka URL berikut untuk mengunduh Agen Konektor Graph: **https://www.microsoft.com/en-us/download/details.aspx?id=104045**. Pilih tombol **Unduh**. 
1. Buka file **GcaInstaller_3.1.1.0.msi** dan ikuti perintah di wizard Penyetelan. 
2. Pada bilah **Pencarian** di bagian bawah layar, masukkan **Graph connector agent config** lalu pilih aplikasi dari menu jika aplikasi itu muncul.
3. Izinkan aplikasi membuat perubahan pada perangkat dengan memilih **Ya**.
4. Masuk dan Daftarkan GCA menggunakan akun **Administrasi MOD**. Konfirmasi bahwa autentikasi selesai muncul di browser Edge. Anda dapat menutup jendela ini.
5. Buka aplikasi GCA dengan memilih ikon di bagian bawah layar.
1. Beri nama agen ini **ContosoFiles**.
1. Pilih tab portal Azure Microsoft Edge (tertulis **Contoso Fileshare - Microsoft Azure**), buka layar **Ikhtisar** dan salin **ID Aplikasi (klien)**. Tempelkan ke aplikasi penginstalan GCA.

Selanjutnya, Anda perlu mengatur Rahasia Klien untuk aplikasi ini di portal Azure.

1. Kembali ke tab tepi portal Azure dan buka **Sertifikat & rahasia**.
1. Pilih **+ Rahasia klien baru**. Tambahkan **Deskripsi** dengan memasukkan **Contoso Files**. Anda dapat membiarkan bidang kedaluwarsa diatur ke default 180 hari.
2. Pilih **Tambahkan**.
3. Salin bidang **Nilai** rahasia senyap.
1. Kembali ke aplikasi agen konektor Graph dan tempelkan nilai ke bidang **Kata sandi aplikasi (rahasia klien)** pada aplikasi penginstal GCA.
1. Pilih **Daftarkan**.
1. Setelah pendaftaran selesai, tutup aplikasi Penginstal.

### Tugas 3: Mengunduh file sumber daya dari Github

Untuk mengonfigurasi konektor menggunakan GCA, Anda memerlukan file lokal di sistem Anda. 

1. Buka peramban Edge baru dan masukkan **https://github.com/MicrosoftLearning/MS-4014-Build-a-foundation-to-extend-Microsoft-365-Copilot/tree/master/ResourceFiles** di bilah alamat.
2. Pilih file pertama di folder, **tren pasar Contoso Chai Tea 2023.xls** untuk membukanya.
3. Pilih tombol **elipsis (tindakan file lainnya)** di kanan atas layar, lalu pilih **Unduh**.
4. Ulangi langkah 3 untuk setiap file yang tersisa.
5. Anda dapat menutup jendela ini setelah unduhan selesai.
6. Gunakan file explorer dan buka direktori C:\. Buat folder baru bernama **ResourceFiles**.
7. Buka jendela file explorer baru dan buka folder **Unduh** untuk mengakses file Contoso yang Anda unduh dari GitHub. Salin file-file ini ke direktori **C:\ResourceFiles**.

Anda akan menggunakan file-file ini sebagai konten sumber untuk koneksi yang Anda buat di pusat admin Microsoft.

### Tugas 4: Membuka pusat admin Microsoft

1. Buka tab peramban Microsoft Edge baru. Di peramban Microsoft Edge Anda, buka halaman **Microsoft 365 Home** dengan memasukkan URL berikut di bilah alamat: **https://portal.office.com**
1. Jika Anda diminta untuk masuk, masukkan LON-CL1 **Nama Pengguna** dan **Kata Sandi** yang disediakan oleh penyedia hosting lab Anda untuk penyewa uji coba Microsoft 365. Nama pengguna harus dalam bentuk **<admin@xxxxxZZZZZZ.onmicrosoft.com>**, di mana xxxxxZZZZZZ adalah awalan penyewa yang ditetapkan oleh penyedia hosting lab Anda. 
1. Halaman **Selamat datang di Microsoft 365** muncul di peramban Microsoft Edge Anda di tab **Home | Microsoft 365**. Halaman ini adalah halaman beranda MOD Administrator Microsoft 365.
1. Pilih **Admin** dari daftar ikon aplikasi yang muncul di panel navigasi; tindakan ini membuka **Pusat admin Microsoft 365** di tab peramban baru.
1. Pilih **... Perlihatkan semua** untuk menampilkan menu navigasi lengkap. Pilih **Pengaturan** -> **Pencarian & kecerdasan.**
1. Pada tab **Ikhtisar** yang muncul, gulir ke bawah. Pilih **Tambahkan koneksi pertama Anda** dari kotak **Konektor**.

Dari sini, daftar sumber data yang tersedia tercantum. Kami mengonfigurasi koneksi menggunakan konektor File Share. Konektor ini memungkinkan pencarian Microsoft 365 Copilot untuk merujuk file pelanggan dalam pencarian Copilot dan memungkinkan waktu respons yang lebih cepat dan respons yang lebih akurat saat agen layanan pelanggan menemukan jawaban.

1. Pilih **File Share**, lalu pilih **Berikutnya**.
1. Masukkan **Nama Tampilan**. Nama tampilan adalah nama unik yang ditampilkan kepada pengguna. Mari masukkan **File Contoso** di bidang.
1. Masukkan **Jalur folder.** Kami memiliki contoh file yang dikonfigurasi di direktori berikut:

   **C:\ResourceFiles**

Jalur ini adalah direktori tempat file disimpan.

1. Pilih GCA yang telah kita buat pada langkah sebelumnya (ContosoFiles) di bidang **Agen penghubung Graph** jika bukan merupakan opsi default.
1. Pilih **Windows** sebagai jenis Autentikasi, lalu masukkan **Nama Pengguna** dan **Kata Sandi** LON-CLI untuk autentikasi Windows.
1. Pilih kotak centang untuk mengizinkan Microsoft membuat indeks data pihak ketiga di penyewa Anda.
1. Pilih **Buat**.  Tampil layar tentang keberhasilan, lalu koneksi Anda mulai disinkronkan. Anda dapat memasukkan jenis konten tertentu, departemen di organisasi Anda yang mungkin mendapatkan manfaat dari penggunaannya, atau contoh alur kerja di kotak **Deskripsi konektor**. Contohnya:

    **Konektor ini berisi informasi yang terdapat dalam server berbagi file di tempat. Informasi ini mencakup profil pelanggan dan pertanyaan pelanggan yang mungkin bermanfaat bagi bagian dukungan dalam menjawab pertanyaan pelanggan.**
1. Pilih **Selesai**. Koneksi Anda sekarang muncul di tab **Pencarian & kecerdasan** di pusat admin Microsoft 365, dan dapat dirujuk pada hasil pencarian Anda atau saat Anda membuat agen Microsoft 365 Copilot.
