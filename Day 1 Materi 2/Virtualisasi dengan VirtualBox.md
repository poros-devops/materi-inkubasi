Modul ini membahas konsep dan implementasi virtualisasi menggunakan Oracle VirtualBox. Untuk memperkuat pemahaman teoretis melalui pengalaman praktis, modul ini dilengkapi dengan sesi *hands-on* yang memandu pengguna dalam menerapkan materi secara langsung.
# Prerequisites
- VirtualBox Installer:
Windows: [Direct Download](https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6a-172322-Win.exe)
Linux: [Download Page](https://www.virtualbox.org/wiki/Linux_Downloads)
- Ubuntu Server image: [Download Page](https://ubuntu.com/download/server)

# VirtualBox

## Penjelasan Singkat VirtualBox

VirtualBox pertama kali dikembangkan oleh perusahaan asal Jerman, **Innotek GmbH**, dan dirilis pada tahun **2007**. Setelah melalui serangkaian akuisisi oleh Sun Microsystems (2008) dan kemudian Oracle Corporation (2010), perangkat lunak ini kini dikenal sebagai **Oracle VM VirtualBox** \[1\].

VirtualBox menggunakan teknologi **hypervisor tipe 2**, yaitu hypervisor yang berjalan di atas sistem operasi host yang telah terinstal \[2\]. VirtualBox memanfaatkan fitur virtualisasi perangkat keras pada prosesor modern (Intel VT-x dan AMD-V) untuk meningkatkan performa mesin virtual. Selain itu, VirtualBox juga mendukung berbagai fitur unggulan seperti **snapshot** untuk menyimpan status mesin virtual pada titik tertentu, serta **Guest Additions** untuk meningkatkan integrasi antara sistem operasi host dan guest \[3\].

VirtualBox memegang peran penting dalam bidang **pengembangan perangkat lunak**, **pendidikan**, dan **keamanan siber**. Dengan sifatnya yang **gratis** dan **open source**, serta ketersediaan lintas platform (Windows, macOS, dan Linux), VirtualBox menjadi salah satu solusi virtualisasi yang paling banyak digunakan oleh pelajar, pengembang, maupun peneliti di seluruh dunia \[1\]\[3\].

## Instalasi VirtualBox

VirtualBox mendukung berbagai sistem operasi, termasuk Windows dan Linux. Berikut adalah panduan instalasi untuk kedua platform tersebut.

### Instalasi di Windows

VirtualBox menyediakan installer berbasis antarmuka grafis (GUI) untuk sistem operasi Windows, sehingga proses instalasi dapat dilakukan dengan mudah.

Langkah instalasi:
1. **Unduh Installer**
Unduh installer VirtualBox dari halaman resmi: [Direct Download](https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6a-172322-Win.exe)
2. **Jalankan Installer**
Setelah proses pengunduhan selesai, klik dua kali file installer untuk memulai installation wizard. Anda dapat menggunakan konfigurasi default untuk instalasi standar.
<img
src="Virtualisasi%20dengan%20VirtualBox-media/a149f91b010881cd092356d1196b3f3eb5199ca6.png"
class="wikilink" alt="Pastedimage20260301093546.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/b19face0fffda449141ec9717f908d0f1a5a22c7.png"
class="wikilink" alt="Pastedimage20260301093600.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/81e71ed58e47f840c08dc777c05b02e263635332.png"
class="wikilink" alt="Pastedimage20260301093608.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/6d8c209f3f384c66ec6f12cd18756516af2f8620.png"
class="wikilink" alt="Pastedimage20260301093618.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/3c875ba7d1321bc96ec2d2973740a2e8d6e6edb9.png"
class="wikilink" alt="Pastedimage20260301093632.png" />
3. **Verifikasi Instalasi**
Setelah instalasi selesai, jalankan aplikasi VirtualBox untuk memverifikasi bahwa instalasi telah berhasil dilakukan.
<img
src="Virtualisasi%20dengan%20VirtualBox-media/38643d9413a730e3a4afc23ec35405cb5b62e5e3.png"
class="wikilink" alt="Pastedimage20260301093641.png" />

### Instalasi di Linux

VirtualBox mendukung dua metode instalasi pada sistem operasi Linux, yaitu melalui *command line* dan melalui *software package*. Pada modul ini, metode yang digunakan adalah instalasi melalui *software package*. Berikut adalah langkah-langkahnya:

1.  **Unduh Software Package**
    Unduh *software package* yang sesuai dengan distribusi Linux yang Anda gunakan melalui tautan berikut: [Download Page](https://www.virtualbox.org/wiki/Linux_Downloads)<img
    src="Virtualisasi%20dengan%20VirtualBox-media/087d2218adb7a54e9a075aba87f87aeb01ae0981.png"
    class="wikilink" alt="Pastedimage20260228223644.png" />
2.  **Jalankan GUI Installer**
    Setelah proses pengunduhan selesai, jalankan *installer* dengan mengeklik berkas *software package* tersebut (umumnya tersimpan di folder **Downloads**).<img
    src="Virtualisasi%20dengan%20VirtualBox-media/a19f0e60c78bcab00d8c88f6b4ba0449de91c180.png"
    class="wikilink" alt="Pastedimage20260228165725.png" />
    *Package manager* pada sistem Anda akan menampilkan jendela konfirmasi instalasi. Klik tombol **Install** untuk melanjutkan proses instalasi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/68d9d8e56d20869e472bec3064fdb5b6142b73bc.png"
    class="wikilink" alt="Pastedimage20260228165620.png" />Tunggu hingga proses pemasangan selesai sepenuhnya.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/527ca7102000d6e1f971ac7b0920c5c3656324a4.png"
    class="wikilink" alt="Pastedimage20260228224312.png" />
3.  **Verifikasi Instalasi**
    Setelah proses instalasi selesai, jalankan aplikasi VirtualBox untuk memastikan bahwa instalasi telah berhasil dilakukan.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/9c5155a7b0197a0d5bedc2bd75325b59e5735cc0.png"
    class="wikilink" alt="Pastedimage20260228224728.png" />

## Operasi-operasi Dasar VirtualBox

VirtualBox mendukung berbagai operasi dasar untuk manajemen *Virtual Machine* (VM), mulai dari pembuatan, modifikasi, hingga penghapusan VM.

### Pembuatan VM Baru

Berikut adalah langkah-langkah untuk membuat *Virtual Machine* baru pada VirtualBox.

1.  **Buka Menu "New"**
    Klik tombol **New** yang terletak di bagian kanan atas antarmuka VirtualBox untuk membuka menu konfigurasi VM baru.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/87de9a71926d76cc9939d3a797019bc31149c51a.png"
    class="wikilink" alt="Pastedimage20260228225506.png" />
2.  **Konfigurasi Awal**
    Pada tahap ini, tentukan nama VM, pilih file ISO sistem operasi yang akan digunakan, serta sesuaikan pengaturan awal lainnya sesuai kebutuhan.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/8729068d58a9188dd7f361a2f9bf13ce19553026.png"
    class="wikilink" alt="Pastedimage20260228231500.png" />
3.  **Konfigurasi Hardware**
    Tentukan alokasi sumber daya perangkat keras (*hardware*) yang akan digunakan oleh VM, seperti RAM dan CPU. Untuk keperluan dasar, nilai alokasi *default* yang tersedia sudah mencukupi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/fba70783862f59d34cfe1a7237a11e7c94a20e6e.png"
    class="wikilink" alt="Pastedimage20260228230833.png" />
4.  **Review Konfigurasi**
    Periksa kembali seluruh konfigurasi VM yang telah ditentukan. Apabila semua pengaturan sudah sesuai, klik tombol **Next** untuk melanjutkan ke proses instalasi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/22fe226be9a694aa92e048fb0d9b91c0d1e9af9a.png"
    class="wikilink" alt="Pastedimage20260228231820.png" />
5.  **Jalankan Virtual Machine**
    Pada tahap ini, *Virtual Machine* telah berhasil dibuat dan siap dijalankan. Perlu diperhatikan bahwa umumnya masih terdapat tahap lanjutan berupa instalasi sistem operasi di dalam VM tersebut.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/a133c78c368678c8c1ab305d46a578250157b93f.png"
    class="wikilink" alt="Pastedimage20260301003716.png" />

### Modifikasi VM yang Sudah Ada

Untuk mengubah konfigurasi *Virtual Machine* yang sudah ada, pilih VM yang ingin dimodifikasi dari daftar, kemudian klik tombol **Settings** yang ditandai dengan ikon gir.<img
src="Virtualisasi%20dengan%20VirtualBox-media/5a5444c42e1b0df17f6563edda1c3830fbeb578b.png"
class="wikilink" alt="Pastedimage20260301005140.png" />

Setelah itu, sebuah jendela baru akan muncul dan menampilkan berbagai pilihan konfigurasi yang dapat disesuaikan.<img
src="Virtualisasi%20dengan%20VirtualBox-media/1496c8ce25d6672d1e94d2e25115bec983277555.png"
class="wikilink" alt="Pastedimage20260301005626.png" />

Salah satu konfigurasi yang umum dilakukan adalah mengubah alokasi RAM dan jumlah *core* CPU yang digunakan oleh VM.

#### Mengubah Alokasi RAM

Klik tab **System** pada panel sebelah kiri jendela pengaturan. Pada tab **Motherboard**, terdapat *slider* berlabel **Base Memory** yang menunjukkan jumlah RAM yang dialokasikan untuk VM. Geser *slider* tersebut ke kiri untuk mengurangi alokasi, atau ke kanan untuk menambahnya.<img
src="Virtualisasi%20dengan%20VirtualBox-media/c908492476f0e75db9ba5bd5cff47735dfd1b548.png"
class="wikilink" alt="Pastedimage20260301005915.png" />

#### Mengubah Alokasi Core CPU

Masih dalam tab **System**, klik sub-tab **Processor**. Pada tab ini, terdapat *slider* yang digunakan untuk mengatur jumlah *core* CPU yang dialokasikan untuk VM. Sesuaikan nilai tersebut sesuai dengan kebutuhan.<img
src="Virtualisasi%20dengan%20VirtualBox-media/e2c0392aa932d15ee4c4e8398daeb36abd7af806.png"
class="wikilink" alt="Pastedimage20260301010219.png" />

### Hapus Virtual Machine

Untuk menghapus sebuah VM, klik kanan pada VM yang ingin dihapus dari daftar, kemudian pilih opsi **Remove**. Sebuah jendela konfirmasi akan muncul untuk memastikan tindakan Anda.<img
src="Virtualisasi%20dengan%20VirtualBox-media/6e0b39de36274b6b5275bed2f399791d6ac4c39a.png"
class="wikilink" alt="Pastedimage20260301011758.png" />

Pada jendela konfirmasi tersebut, Anda dapat memilih untuk menghapus VM beserta seluruh file *virtual disk*-nya dengan mengaktifkan opsi **Delete the virtual machine files and virtual hard disks**, atau memilih untuk menghapus VM saja tanpa menghapus file *virtual disk* yang tersimpan.<img
src="Virtualisasi%20dengan%20VirtualBox-media/0c59a3b998b6ec3e9d54d07b3519f2e57f57891d.png"
class="wikilink" alt="Pastedimage20260301011853.png" />

Apabila Anda telah yakin dengan pilihan tersebut, klik tombol **Remove** untuk menyelesaikan proses penghapusan.

## Backup dengan Snapshots

### Membuat Snapshot

1.  **Pilih VM**
    Pilih VM yang ingin Anda ambil snapshot-nya.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/79485e13263d038c6f4a47b05ba656e6cec8e189.png"
    class="wikilink" alt="Pastedimage20260301090916.png" />
2.  **Klik Tab Snapshots**
    Klik tab **Snapshots** untuk masuk ke menu snapshot.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/a42f05444695858c502aa2c5e57ac0bed75ac0c4.png"
    class="wikilink" alt="Pastedimage20260301090726.png" />
3.  **Pilih Take**
    Klik tombol **Take** (kanan atas) untuk mengambil snapshot pada kondisi saat ini.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/b4606b1633b3318137e85b4ba6b13d03ce8efdab.png"
    class="wikilink" alt="Pastedimage20260301090802.png" />
    Gambar di bawah menunjukkan kondisi ketika snapshot telah berhasil diambil.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/c2655e6d4bcf91824918c6bb509bd6ed1641591b.png"
    class="wikilink" alt="Pastedimage20260301090851.png" />

### Memulihkan Snapshot

Untuk memulihkan snapshot, Anda harus masuk ke menu snapshot (lihat pada bagian cara membuat snapshot). Kemudian klik kanan snapshot yang ingin dituju, lalu pilih "restore". <img
src="Virtualisasi%20dengan%20VirtualBox-media/9b63c966e13496b0e704dfe98b876bf988d04ad5.png"
class="wikilink" alt="Pastedimage20260301091439.png" />

## Hands-on VirtualBox

**Prerequisites:**
- VirtualBox terinstal
- Ubuntu Server Image
- Koneksi Internet

**End Goal:**
Instalasi Ubuntu Server yang berjalan di atas VirtualBox

### Langkah-langkah Hands-on

#### Instalasi VirtualBox

VirtualBox mendukung berbagai sistem operasi, termasuk Windows dan Linux. Berikut adalah panduan instalasi untuk kedua platform tersebut.

##### Instalasi di Windows

VirtualBox menyediakan installer berbasis antarmuka grafis (GUI) untuk sistem operasi Windows, sehingga proses instalasi dapat dilakukan dengan mudah.

Langkah instalasi:
1. **Unduh Installer**
Unduh installer VirtualBox dari halaman resmi: https://download.virtualbox.org/virtualbox/7.2.6/VirtualBox-7.2.6a-172322-Win.exe
2. **Jalankan Installer**
Setelah proses pengunduhan selesai, klik dua kali file installer untuk memulai installation wizard. Anda dapat menggunakan konfigurasi default untuk instalasi standar.
<img
src="Virtualisasi%20dengan%20VirtualBox-media/a149f91b010881cd092356d1196b3f3eb5199ca6.png"
class="wikilink" alt="Pastedimage20260301093546.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/b19face0fffda449141ec9717f908d0f1a5a22c7.png"
class="wikilink" alt="Pastedimage20260301093600.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/81e71ed58e47f840c08dc777c05b02e263635332.png"
class="wikilink" alt="Pastedimage20260301093608.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/6d8c209f3f384c66ec6f12cd18756516af2f8620.png"
class="wikilink" alt="Pastedimage20260301093618.png" />
<img
src="Virtualisasi%20dengan%20VirtualBox-media/3c875ba7d1321bc96ec2d2973740a2e8d6e6edb9.png"
class="wikilink" alt="Pastedimage20260301093632.png" />
3. **Verifikasi Instalasi**
Setelah instalasi selesai, jalankan aplikasi VirtualBox untuk memverifikasi bahwa instalasi telah berhasil dilakukan.
<img
src="Virtualisasi%20dengan%20VirtualBox-media/38643d9413a730e3a4afc23ec35405cb5b62e5e3.png"
class="wikilink" alt="Pastedimage20260301093641.png" />

##### Instalasi di Linux

VirtualBox mendukung dua metode instalasi pada sistem operasi Linux, yaitu melalui *command line* dan melalui *software package*. Pada modul ini, metode yang digunakan adalah instalasi melalui *software package*. Berikut adalah langkah-langkahnya:

1.  **Unduh Software Package**
    Unduh *software package* yang sesuai dengan distribusi Linux yang Anda gunakan melalui tautan berikut: <https://www.virtualbox.org/wiki/Linux_Downloads><img
    src="Virtualisasi%20dengan%20VirtualBox-media/087d2218adb7a54e9a075aba87f87aeb01ae0981.png"
    class="wikilink" alt="Pastedimage20260228223644.png" />
2.  **Jalankan GUI Installer**
    Setelah proses pengunduhan selesai, jalankan *installer* dengan mengeklik berkas *software package* tersebut (umumnya tersimpan di folder **Downloads**).<img
    src="Virtualisasi%20dengan%20VirtualBox-media/a19f0e60c78bcab00d8c88f6b4ba0449de91c180.png"
    class="wikilink" alt="Pastedimage20260228165725.png" />
    *Package manager* pada sistem Anda akan menampilkan jendela konfirmasi instalasi. Klik tombol **Install** untuk melanjutkan proses instalasi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/68d9d8e56d20869e472bec3064fdb5b6142b73bc.png"
    class="wikilink" alt="Pastedimage20260228165620.png" />Tunggu hingga proses pemasangan selesai sepenuhnya.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/527ca7102000d6e1f971ac7b0920c5c3656324a4.png"
    class="wikilink" alt="Pastedimage20260228224312.png" />
3.  **Verifikasi Instalasi**
    Setelah proses instalasi selesai, jalankan aplikasi VirtualBox untuk memastikan bahwa instalasi telah berhasil dilakukan.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/9c5155a7b0197a0d5bedc2bd75325b59e5735cc0.png"
    class="wikilink" alt="Pastedimage20260228224728.png" />

#### Buat VM Baru

Pada sesi praktik ini, Anda akan membuat sebuah mesin virtual (VM) Ubuntu Server menggunakan VirtualBox. Berkas ISO Ubuntu Server dapat diunduh melalui [tautan resmi berikut](https://ubuntu.com/download/server). Berikut adalah panduan lengkap untuk pembuatan VM tersebut:

##### Pembuatan Virtual Machine

Langkah awal yang perlu dilakukan adalah membuat Virtual Machine baru di VirtualBox.

1.  **Buka Menu 'New'**
    Akses menu konfigurasi VM baru dengan mengklik tombol **New** yang terletak di bagian kanan atas antarmuka VirtualBox.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/87de9a71926d76cc9939d3a797019bc31149c51a.png"
    class="wikilink" alt="Pastedimage20260228225506.png" />

2.  **Konfigurasi Awal**
    Pada tahap ini, tentukan nama VM, pilih berkas ISO sistem operasi yang akan digunakan, serta sesuaikan pengaturan awal lainnya sesuai kebutuhan. Untuk modul ini, gunakan ISO Ubuntu Server yang dapat diunduh melalui [halaman resmi Ubuntu](https://ubuntu.com/download/server).<img
    src="Virtualisasi%20dengan%20VirtualBox-media/9ab6ff86321f1891ae80cb35ea143a052b071b9e.png"
    class="wikilink" alt="Pastedimage20260302162007.png" />

3.  **Konfigurasi Perangkat Keras**
    Alokasikan sumber daya perangkat keras (CPU, RAM, dan penyimpanan) untuk VM yang akan dibuat. Untuk keperluan praktik ini, konfigurasi bawaan (*default*) sudah mencukupi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/f2da6a818ce11ce9622a189534b277ccdbf0841b.png"
    class="wikilink" alt="Pastedimage20260302162040.png" />

4.  **Tinjauan Konfigurasi**
    Periksa kembali seluruh pengaturan VM yang telah Anda tentukan. Apabila seluruh konfigurasi telah sesuai, klik tombol **Next** untuk melanjutkan ke proses pembuatan VM.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/50062c91c7bdabc00d334e981eb6b77c4619a7d9.png"
    class="wikilink" alt="Pastedimage20260302162056.png" />

5.  **Jalankan Virtual Machine**
    Pada tahap ini, Virtual Machine telah berhasil dibuat. Namun, proses instalasi sistem operasi pada VM tersebut masih perlu dilakukan.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/a133c78c368678c8c1ab305d46a578250157b93f.png"
    class="wikilink" alt="Pastedimage20260301003716.png" />

6.  **Akses Live Server**
    Untuk memulai instalasi, pilih entri **"Try or Install Ubuntu Server"** pada menu boot.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/3d81971cce69003201ac24d6a344c1bb7e9e3716.png"
    class="wikilink" alt="Pastedimage20260302162151.png" />
    \> **Catatan:** Instalasi Ubuntu Server menggunakan antarmuka berbasis baris perintah (*command-line interface*) yang dikendalikan melalui keyboard. Gunakan tombol panah (atas, bawah, kiri, kanan) untuk navigasi dan tombol **Enter** untuk konfirmasi.

##### Instalasi Sistem Operasi

Setelah VM berhasil dibuat, langkah berikutnya adalah melakukan instalasi sistem operasi Ubuntu Server.

1.  **Konfigurasi Bahasa dan Tata Letak Keyboard**
    Pilih bahasa dan tata letak keyboard yang ingin digunakan selama instalasi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/a0db3b4ba3fe72ed3dbfc75b6656daf0e933f66a.png"
    class="wikilink" alt="Pastedimage20260302162316.png" /><img
    src="Virtualisasi%20dengan%20VirtualBox-media/759dba1d6a2e2e6bccfac8157166866eaaa6740b.png"
    class="wikilink" alt="Pastedimage20260302162534.png" /> Anda dapat menyesuaikan pilihan ini sesuai preferensi. Namun, untuk konsistensi sesi praktik ini, disarankan menggunakan konfigurasi bawaan (Bahasa: English, Keyboard: English (US)).

2.  **Pilih Tipe Instalasi**
    Layar ini menyediakan opsi tipe instalasi Ubuntu Server. Pada sesi ini, gunakan konfigurasi bawaan tanpa modifikasi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/e0fc93da86568d2c2c7204ab933222360ff8b5b4.png"
    class="wikilink" alt="Pastedimage20260302162557.png" />
    \> **Perbedaan Tipe Instalasi:**
    \> - **Ubuntu Server**: Versi standar dengan paket tambahan yang memudahkan pengelolaan sistem.
    \> - **Ubuntu Server (Minimized)**: Versi minimal yang hanya menginstal paket inti untuk mengoptimalkan penggunaan sumber daya.

3.  **Konfigurasi Jaringan**
    Konfigurasikan antarmuka jaringan yang akan digunakan oleh sistem. Untuk sesi praktik ini, gunakan konfigurasi bawaan.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/15133b042b6f4de322e033629e90b631f99c46a2.png"
    class="wikilink" alt="Pastedimage20260302162624.png" />

    Halaman berikutnya memungkinkan konfigurasi proxy. Biarkan kolom ini kosong karena praktik ini tidak memerlukan proxy.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/af726fd917678d93b1946dc8ded3029e20d11875.png"
    class="wikilink" alt="Pastedimage20260302162647.png" />

4.  **Pilih Archive Mirror**
    Pilih *mirror* server arsip yang akan digunakan untuk mengunduh paket selama instalasi. Gunakan konfigurasi bawaan untuk saat ini.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/fd9db3bf10efc0b3742a3ad03a219244a4dc18ba.png"
    class="wikilink" alt="Pastedimage20260302162928.png" />

5.  **Konfigurasi Penyimpanan**
    Pada tahap konfigurasi penyimpanan, pilih opsi **"Use an entire disk"** yang telah terpilih secara bawaan. Opsi ini akan menghapus seluruh partisi pada disk virtual.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/dec5b338d77dce34b66bb3c1deb311985484c2de.png"
    class="wikilink" alt="Pastedimage20260302163109.png" />
    \> **Peringatan:** Proses ini hanya memengaruhi disk virtual di dalam VM dan tidak akan berdampak pada data di komputer host Anda.

6.  **Tinjauan Instalasi**
    Tinjau kembali seluruh konfigurasi instalasi yang telah ditetapkan. Apabila seluruh pengaturan telah sesuai, konfirmasi untuk memulai proses instalasi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/e302e21294b4294630e2f0eb4f2df8ba3b03ce2e.png"
    class="wikilink" alt="Pastedimage20260302163132.png" /><img
    src="Virtualisasi%20dengan%20VirtualBox-media/ea5ce8f444234f478f35f9b0fda8b026ba0a8656.png"
    class="wikilink" alt="Pastedimage20260302163204.png" />

7.  **Konfigurasi Akun Pengguna**
    Pada layar ini, lakukan konfigurasi nama server (*hostname*), nama pengguna (*username*), dan kata sandi (*password*). Isi sesuai dengan preferensi Anda.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/d2b984a2319fd1523b91a0648aa76514b5d7f704.png"
    class="wikilink" alt="Pastedimage20260302163333.png" />

8.  **Ubuntu Pro**
    Layar ini menawarkan opsi untuk mengaktifkan langganan Ubuntu Pro. Untuk keperluan praktik ini, pilih **"Skip for now"**.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/a5d697602419d946bdecc32a59d1f0a1a011ee9c.png"
    class="wikilink" alt="Pastedimage20260302163347.png" />

9.  **Konfigurasi Tambahan**
    Anda dapat melakukan konfigurasi tambahan seperti instalasi paket SSH atau snap. Untuk sesi ini, lewati tahap ini dan lanjutkan ke proses instalasi.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/1488ab67904dbe86b15c70d7d20e839e8843c4ec.png"
    class="wikilink" alt="Pastedimage20260302163416.png" /><img
    src="Virtualisasi%20dengan%20VirtualBox-media/dbb199b73da41e95bb46cf18272f1d6f6673e7fc.png"
    class="wikilink" alt="Pastedimage20260302163450.png" />

10. **Tunggu Proses Instalasi**
    Biarkan proses instalasi berjalan hingga selesai. Setelah instalasi rampung, sistem akan meminta Anda untuk melakukan *reboot*.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/5763997eaf74cc77654417c3279fb034bde96a30.png"
    class="wikilink" alt="Pastedimage20260302163504.png" /><img
    src="Virtualisasi%20dengan%20VirtualBox-media/3f38f84119795faa21cd53cb8edff352b6d24986.png"
    class="wikilink" alt="662" />
    \> **Troubleshooting:** Jika proses terhenti pada pesan error terkait unmount CD-ROM, tekan tombol **Enter** untuk melanjutkan *reboot*. Pesan ini muncul karena sistem tidak dapat melepaskan media instalasi secara otomatis.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/b30f564f7537155f4b3698c8aeab60b5fb4c88ec.png"
    class="wikilink" alt="Pastedimage20260302164824.png" />

11. **Instalasi Selesai**
    Setelah *reboot*, jika sistem tampak terhenti pada layar hitam, tekan tombol **Enter** untuk melanjutkan ke *prompt* login.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/d49c6812cddd587ecc4699e3cf16aea7b9788779.png"
    class="wikilink" alt="Pastedimage20260302165025.png" />
    \> **Login ke Sistem:** Masukkan *username* yang telah Anda buat sebelumnya, tekan **Enter**, kemudian masukkan *password* yang sesuai dan tekan **Enter** kembali.<img
    src="Virtualisasi%20dengan%20VirtualBox-media/9a3422be021ec0800d5d5b2084b2b6c300bbc3d0.png"
    class="wikilink" alt="Pastedimage20260302174117.png" />

### Verifikasi Keberhasilan Hands-on

Untuk memastikan bahwa Anda telah berhasil menyelesaikan seluruh tahap praktik ini, lakukan login ke VM yang baru saja dibuat dan uji konektivitas jaringan dengan perintah `ping` ke internet.<img
src="Virtualisasi%20dengan%20VirtualBox-media/1defa505a265bcabb3a0c41010a9b5318fb17c35.png"
class="wikilink" alt="Pastedimage20260302165304.png" />

Apabila perintah `ping` menerima respons, maka Anda telah berhasil menyelesaikan sesi praktik ini.

### Rangkuman Modul

Modul ini telah membahas tiga teknologi fundamental dalam virtualisasi dan kontainerisasi:

1.  **VirtualBox**: Hypervisor tipe 2 yang memungkinkan pembuatan dan manajemen mesin virtual untuk lingkungan pengembangan yang terisolasi. Fitur unggulan meliputi snapshot, Guest Additions, dan fleksibilitas konfigurasi jaringan.

2.  **Docker**: Platform kontainerisasi yang memungkinkan pengemasan aplikasi beserta dependensinya dalam unit terisolasi yang ringan dan portabel. Dockerfile menjadi kunci dalam pembuatan image yang konsisten dan dapat direproduksi.

3.  **Docker Compose**: Alat orkestrasi yang menyederhanakan pengelolaan aplikasi multi-container melalui satu file konfigurasi YAML, memungkinkan definisi layanan, jaringan, dan volume secara terpusat.

------------------------------------------------------------------------

# Referensi

\[1\] Oracle Corporation. *Oracle VM VirtualBox User Manual*. https://www.virtualbox.org/manual/
\[2\] Smith, J.E., & Nair, R. (2005). *Virtual Machines: Versatile Platforms for Systems and Processes*. Morgan Kaufmann.
\[3\] Oracle. *VirtualBox Features Overview*. https://www.virtualbox.org/wiki/Features
