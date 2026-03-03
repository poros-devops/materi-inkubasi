Modul ini membahas implementasi kontainerisasi menggunakan Docker serta orkestrasi aplikasi multi-kontainer dengan Docker Compose. Untuk memperkuat pemahaman konseptual, modul ini dilengkapi dengan sesi praktik (*hands-on*) yang memandu pembaca dalam menerapkan materi secara langsung.

# Docker
## Penjelasan Singkat Docker
Docker adalah platform perangkat lunak **open source** yang digunakan untuk mengembangkan, mengemas, dan menjalankan aplikasi di dalam unit terisolasi yang disebut **container**. Berbeda dengan mesin virtual, container Docker berbagi kernel sistem operasi host sehingga lebih ringan dan efisien dalam penggunaan sumber daya \[1\].

Docker pertama kali diperkenalkan oleh **Solomon Hykes** pada konferensi PyCon pada bulan Maret **2013** sebagai proyek internal perusahaan **dotCloud**, sebuah penyedia Platform as a Service (PaaS). Respons komunitas yang sangat positif mendorong dotCloud untuk merilis Docker sebagai proyek open source. Pada tahun **2013**, perusahaan tersebut kemudian berganti nama menjadi **Docker, Inc.** untuk mencerminkan fokus utamanya pada teknologi Docker \[2\].

Docker menggunakan beberapa teknologi inti yang menjadi fondasi cara kerjanya, antara lain:
- **Container:** Unit standar perangkat lunak yang mengemas kode beserta seluruh dependensinya agar aplikasi dapat berjalan secara konsisten di berbagai lingkungan.
- **Docker Image:** Template read-only yang digunakan sebagai dasar pembuatan container. Image dibangun menggunakan file konfigurasi yang disebut **Dockerfile**.
- **Docker Engine:** Komponen inti Docker yang bertugas membangun, menjalankan, dan mengelola container.
- **Docker Hub:** Repositori berbasis cloud yang digunakan untuk menyimpan dan berbagi Docker image secara publik maupun privat.
- **Docker Compose:** Alat yang memungkinkan pengguna mendefinisikan dan menjalankan aplikasi multi-container menggunakan satu file konfigurasi berbasis YAML \[1\]\[3\].

Docker berperan penting dalam industri teknologi modern. Prinsip *"build once, run anywhere"* yang ditawarkan Docker membuatnya banyak digunakan dalam pipeline **CI/CD**, arsitektur **microservices**, dan ekosistem **komputasi awan** pada penyedia layanan cloud utama seperti AWS, Google Cloud, dan Microsoft Azure \[2\]\[3\].

## Instalasi Docker (Ubuntu)

Docker memiliki dukungan resmi untuk instalasi pada Ubuntu. Berikut adalah langkah-langkah yang dapat Anda lakukan:

1.  **Hapus Instalasi Docker Sebelumnya**
    Langkah ini dilakukan untuk menghapus package Docker lain yang mungkin dapat menyebabkan konflik.

``` bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

2.  **Set Up Repository Docker**
    Sebelum melakukan instalasi, Anda harus melakukan set up repository resmi Docker untuk Ubuntu. Hal ini mencakup penambahan GPG Docker dan penambahan repository Docker ke `apt`.

``` bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg   -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu  
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

3.  **Install Docker Packages**
    Langkah ini akan menginstal komponen-komponen Docker.

``` bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

4.  **Cek Instalasi Docker**
    Untuk mengecek instalasi, Anda dapat menjalankan perintah berikut. Perintah ini memastikan bahwa Docker diinstal secara sempurna.

``` bash
sudo docker run hello-world
```

5.  **Atur Permission User**
    Perlu dilakukan konfigurasi tambahan untuk mengatur permission agar user biasa dapat menggunakan Docker tanpa privilege root.

``` bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

Anda mungkin perlu melakukan login ulang pada VM Linux Anda.

## Operasi-operasi Dasar Docker

1.  **Mengambil dan Menjalankan Image dari Registry**

``` bash
docker run <nama-image>:<tag-image>
```

Contoh:

``` bash
docker run postgres:14-alpine
```

2.  **Memulai Docker Container**

``` bash
docker start <nama-container>
```

3.  **Menghentikan Container yang Berjalan**

``` bash
docker stop <nama-container>
```

4.  **Melihat Container yang Berjalan**

``` bash
docker ps

# atau seluruh container
docker ps -a
```

5.  **Menghapus Container**

``` bash
docker rm <nama-container>
```

6.  **Melihat Semua Image**

``` bash
docker image ls
```

7.  **Menghapus Image**

``` bash
docker image rm <nama-image>
```

Untuk informasi mengenai seluruh perintah yang tersedia, Anda dapat menggunakan perintah:

``` bash
docker --help
```

## Pembuatan Image Baru

Untuk membuat image baru, Anda harus mendefinisikan image tersebut menggunakan Dockerfile. Dockerfile merupakan sebuah file yang berisi instruksi dalam pembuatan sebuah image.

### Pengenalan Sintaks Dockerfile

Dockerfile adalah sebuah file teks berisi sekumpulan instruksi yang digunakan oleh Docker untuk membangun sebuah **Docker Image** secara otomatis. Setiap instruksi dalam Dockerfile merepresentasikan sebuah lapisan (*layer*) dalam image yang dihasilkan. Dengan menggunakan Dockerfile, proses pembuatan image menjadi **terdokumentasi**, **konsisten**, dan **dapat direproduksi** di berbagai lingkungan.

#### Struktur File

Secara umum, sebuah Dockerfile memiliki struktur sebagai berikut:

``` dockerfile
# Komentar

# 1. Tentukan base image
FROM <base_image>

# 2. Tetapkan metadata image
LABEL <key>=<value>

# 3. Tetapkan direktori kerja
WORKDIR <direktori>

# 4. Salin file dari host ke dalam image
COPY <sumber> <tujuan>

# 5. Jalankan perintah selama proses build
RUN <perintah>

# 6. Ekspos port yang digunakan aplikasi
EXPOSE <port>

# 7. Tentukan perintah yang dijalankan saat container dimulai
CMD ["<perintah>", "<argumen>"]
```

#### Struktur Sintaks

Setiap baris instruksi dalam Dockerfile mengikuti struktur sintaks sebagai berikut:

``` dockerfile
INSTRUKSI argumen
```

Beberapa hal penting terkait sintaks Dockerfile adalah:
- **Instruksi** ditulis dalam huruf kapital (misalnya `FROM`, `RUN`, `COPY`) meskipun Docker sebenarnya tidak membedakan huruf besar dan kecil. Penulisan huruf kapital merupakan konvensi yang dianjurkan untuk memudahkan keterbacaan.
- **Komentar** diawali dengan tanda pagar (`#`) dan akan diabaikan oleh Docker pada saat proses *build*.
- **Urutan instruksi** sangat berpengaruh karena Docker memproses setiap instruksi secara berurutan dari atas ke bawah.
- Setiap instruksi akan membentuk sebuah **layer** baru dalam image. Oleh karena itu, menggabungkan beberapa perintah dalam satu instruksi `RUN` menggunakan operator `&&` adalah praktik yang dianjurkan untuk meminimalkan jumlah layer.

#### Instruksi Dasar

Berikut adalah instruksi-instruksi yang paling umum digunakan dalam penulisan Dockerfile.

##### `FROM`

Instruksi `FROM` digunakan untuk menentukan **base image** yang menjadi fondasi dari image yang akan dibangun. Setiap Dockerfile wajib diawali dengan instruksi ini.

``` dockerfile
FROM ubuntu:22.04
```

Argumen yang dapat digunakan:

| Argumen            | Keterangan                                        |
|--------------------|---------------------------------------------------|
| `<image>`          | Nama image yang digunakan sebagai base            |
| `<image>:<tag>`    | Nama image beserta versi spesifik (*tag*)         |
| `<image>@<digest>` | Nama image beserta *digest* SHA256 untuk keamanan |

> **Catatan:** Apabila tag tidak ditentukan, Docker akan menggunakan tag `latest` secara otomatis. Namun, sebaiknya selalu tentukan versi secara eksplisit untuk memastikan konsistensi.

##### `LABEL`

Instruksi `LABEL` digunakan untuk menambahkan **metadata** pada image dalam bentuk pasangan *key-value*.

``` dockerfile
LABEL maintainer="nama@email.com"
LABEL version="1.0"
LABEL description="Image untuk aplikasi web sederhana"
```

##### `WORKDIR`

Instruksi `WORKDIR` digunakan untuk menetapkan **direktori kerja** di dalam container. Seluruh instruksi `RUN`, `COPY`, `ADD`, dan `CMD` yang mengikutinya akan dieksekusi relatif terhadap direktori ini. Apabila direktori yang ditentukan belum ada, Docker akan membuatnya secara otomatis.

``` dockerfile
WORKDIR /app
```

##### `COPY` dan `ADD`

Kedua instruksi ini digunakan untuk menyalin file dari sistem host ke dalam image.

``` dockerfile
# Menyalin satu file
COPY index.html /app/index.html

# Menyalin seluruh isi direktori
COPY ./src /app/src
```

Perbedaan antara `COPY` dan `ADD`:

| Instruksi | Keterangan |
|----|----|
| `COPY` | Hanya menyalin file atau direktori dari host ke image |
| `ADD` | Memiliki kemampuan tambahan, yaitu dapat mengekstrak arsip `.tar` secara otomatis dan mengunduh file dari URL |

> **Catatan:** Penggunaan `COPY` lebih dianjurkan karena perilakunya lebih eksplisit dan mudah diprediksi. Gunakan `ADD` hanya apabila fitur tambahannya memang dibutuhkan.

##### `RUN`

Instruksi `RUN` digunakan untuk mengeksekusi **perintah shell** selama proses *build* image berlangsung. Instruksi ini umumnya digunakan untuk menginstal dependensi atau melakukan konfigurasi awal.

``` dockerfile
# Menginstal dependensi pada image berbasis Ubuntu
RUN apt-get update && apt-get install -y \
    curl \
    git
# Menginstal dependensi Python
RUN pip install -r requirements.txt
```

##### `EXPOSE`

Instruksi `EXPOSE` digunakan untuk mendokumentasikan **port jaringan** yang akan digunakan oleh container saat berjalan. Perlu diperhatikan bahwa instruksi ini tidak secara otomatis membuka port tersebut ke host, melainkan hanya berfungsi sebagai informasi bagi pengguna dan alat seperti Docker Compose.

    EXPOSE 8080

##### `ENV`

Instruksi `ENV` digunakan untuk menetapkan **variabel lingkungan** (*environment variable*) di dalam container. Variabel ini dapat diakses oleh aplikasi yang berjalan di dalam container.

``` dockerfile
ENV APP_ENV=production
ENV PORT=8080
```

##### `ARG`

Instruksi `ARG` digunakan untuk mendefinisikan **variabel build-time**, yaitu variabel yang hanya tersedia selama proses *build* berlangsung dan tidak tersimpan dalam image final. Nilai dari variabel `ARG` dapat diberikan melalui perintah `docker build` menggunakan flag `--build-arg`.

``` dockerfile
ARG APP_VERSION=1.0
RUN echo "Membangun versi $APP_VERSION"
```

Perbedaan antara `ENV` dan `ARG`:

|                           | `ENV` | `ARG`                  |
|---------------------------|-------|------------------------|
| Tersedia saat *build*     | Ya    | Ya                     |
| Tersedia saat *runtime*   | Ya    | Tidak                  |
| Dapat diubah saat *build* | Tidak | Ya (via `--build-arg`) |

##### `CMD` dan `ENTRYPOINT`

Kedua instruksi ini digunakan untuk menentukan **perintah yang dijalankan** saat container dimulai.

``` dockerfile
# Menggunakan CMD
CMD ["python", "app.py"]

# Menggunakan ENTRYPOINT
ENTRYPOINT ["python", "app.py"]
```

Perbedaan antara `CMD` dan `ENTRYPOINT`:

|  | `CMD` | `ENTRYPOINT` |
|----|----|----|
| Dapat ditimpa saat `docker run` | Ya | Tidak |
| Fungsi utama | Memberikan perintah *default* | Menetapkan perintah utama container |

Keduanya sering digunakan bersama, di mana `ENTRYPOINT` menentukan perintah utama dan `CMD` memberikan argumen *default*-nya:

``` dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

### Langkah Pembuatan Image

Proses pembuatan Docker Image dari sebuah Dockerfile dilakukan menggunakan perintah `docker build`. Berikut adalah struktur dasar perintahnya:

``` bash
docker build -t <nama_image>:<tag> <path_dockerfile>
```

Keterangan:

| Bagian | Keterangan |
|----|----|
| `-t <nama_image>:<tag>` | Menentukan nama dan tag dari image yang akan dibuat |
| `<path_dockerfile>` | Lokasi direktori yang berisi Dockerfile. Gunakan `.` apabila perintah dijalankan dari direktori yang sama |

Contoh penggunaan:

    docker build -t aplikasi-web:1.0 .

## Hands-on Docker

**Prerequisites:**
- Instalasi Ubuntu Dekstop yang berfungsi
- Repositori proyek (bebas)
- Koneksi Internet
- Instalasi Git

**End Goal:**
Docker Image lokal dari sebuah proyek
### Langkah-langkah Hands-on
Pada hands-on kali ini, kita akan membuat Docker image dari sebuah repository. Berikut adalah hal-hal yang harus dilakukan:
#### Instalasi Docker (Ubuntu)
Docker memiliki dukungan resmi untuk instalasi pada Ubuntu. Berikut adalah langkah-langkah yang dapat Anda lakukan:

1.  **Hapus Instalasi Docker Sebelumnya**
    Langkah ini dilakukan untuk menghapus package Docker lain yang mungkin dapat menyebabkan konflik.

``` bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

2.  **Set Up Repository Docker**
    Sebelum melakukan instalasi, Anda harus melakukan set up repository resmi Docker untuk Ubuntu. Hal ini mencakup penambahan GPG Docker dan penambahan repository Docker ke `apt`.

``` bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg   -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu  
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

3.  **Install Docker Packages**
    Langkah ini akan menginstal komponen-komponen Docker.

``` bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

4.  **Cek Instalasi Docker**
    Untuk mengecek instalasi, Anda dapat menjalankan perintah berikut. Perintah ini memastikan bahwa Docker diinstal secara sempurna.

``` bash
sudo docker run hello-world
```

5.  **Atur Permission User**
    Perlu dilakukan konfigurasi tambahan untuk mengatur permission agar user biasa dapat menggunakan Docker tanpa privilege root.

``` bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

Anda mungkin perlu melakukan login ulang pada VM Linux Anda.
#### Membuat Image Baru

1.  **Kloning Repository Proyek**
    Unduh repository yang akan digunakan dengan menggunakan Git. Jika belum terinstal, jalankan perintah:

<!-- -->

    sudo apt-get install -y git

Jika sudah terinstal, lakukan cloning repository.

``` bash
git clone https://github.com/kinsta/hello-world-nextjs.git  
cd hello-world-nextjs
```

2.  **Buat Dockerfile**
    Dalam direktori proyek tersebut, buat file baru bernama `Dockerfile`.

``` bash
touch Dockerfile
```

Lalu tulis instruksi sesuai dengan isi file di bawah. Anda dapat menggunakan teks editor pilihan Anda.

``` dockerfile
# Stage 1: Install dependencies dan build aplikasi
# Menggunakan Node.js 18 dengan Alpine Linux sebagai base image agar ukuran image lebih kecil
FROM node:18-alpine AS builder

# Menetapkan direktori kerja di dalam container
WORKDIR /app

# Menyalin file package.json dan package-lock.json untuk install dependencies
COPY package.json package-lock.json* ./

# Menginstall dependencies berdasarkan lockfile agar hasil konsisten
RUN npm ci

# Menyalin seluruh source code proyek ke dalam container
COPY . .

# Menjalankan build Next.js yang menghasilkan static export ke folder out/
RUN npm run build

# Stage 2: Menyajikan file statis menggunakan nginx
# Menggunakan nginx Alpine sebagai web server yang ringan
FROM nginx:alpine

# Menyalin hasil build (folder out/) dari stage builder ke direktori default nginx
COPY --from=builder /app/out /usr/share/nginx/html

# Mengekspos port 80 agar container dapat diakses dari luar
EXPOSE 80

# Menjalankan nginx di foreground agar container tetap hidup
CMD ["nginx", "-g", "daemon off;"]
```

3.  **Buat .dockerignore**
    Dalam direktori proyek tersebut, buat file baru bernama `.dockerignore`.

``` bash
touch .dockerignore
```

Lalu tulis file/direktori yang ingin diabaikan oleh Docker. Anda dapat menggunakan teks editor pilihan Anda.

``` .dockerignore
node_modules
out
.next
.git
Dockerfile
.dockerignore
README.md
```

4.  **Jalankan Docker Build**
    Jalankan Docker Build untuk membuat image sesuai dengan Dockerfile yang telah Anda tulis, tunggu hingga selesai.

``` bash
docker build -t hello-world-nextjs .
```

### Verifikasi Keberhasilan Hands-on

Sebagai verifikasi bahwa image Anda berhasil dibuat, jalankan perintah di bawah ini. Kemudian:

``` bash
docker run -d -p 3000:80 --name hello-nextjs hello-world-nextjs
```

Buka http://localhost:3000 pada browser Anda, Anda seharusnya akan mendapatkan tampilan seperti ini.<img
src="Docker%20dan%20Docker%20Compose-media/7674faf1afad7b6277c84a5ad006956722e54919.png"
class="wikilink" alt="Pastedimage20260301074459.png" />

# Docker Compose

## Penjelasan Singkat Docker Compose

Docker Compose adalah alat (*tool*) yang digunakan untuk mendefinisikan dan menjalankan aplikasi Docker yang terdiri dari **beberapa container sekaligus**. Dengan menggunakan Docker Compose, seluruh konfigurasi layanan, jaringan, dan volume yang dibutuhkan oleh sebuah aplikasi dapat didefinisikan dalam satu file konfigurasi berbasis **YAML**, sehingga seluruh komponen aplikasi dapat dijalankan hanya dengan satu perintah \[4\].

Docker Compose berawal dari proyek open source bernama **Fig** yang dikembangkan oleh perusahaan **Orchard** pada tahun **2013**. Fig dirancang untuk menyederhanakan pengelolaan aplikasi multi-container pada Docker. Melihat potensinya, **Docker, Inc.** mengakuisisi Orchard pada tahun **2014** dan mengintegrasikan Fig ke dalam ekosistem Docker dengan nama baru **Docker Compose**. Pada tahun **2020**, Docker Compose mengalami pembaruan besar dengan diperkenalkannya **Compose V2** yang ditulis ulang menggunakan bahasa pemrograman Go, menggantikan versi sebelumnya yang ditulis dalam Python \[5\].

Docker Compose berperan penting dalam menyederhanakan proses **pengembangan**, **pengujian**, dan **penerapan** (*deployment*) aplikasi berbasis arsitektur *microservices*. Dengan kemampuannya mendefinisikan seluruh tumpukan layanan (*service stack*) dalam satu file konfigurasi, Docker Compose banyak digunakan dalam lingkungan pengembangan lokal maupun pipeline **CI/CD** untuk memastikan konsistensi lingkungan di seluruh tahap siklus hidup perangkat lunak \[5\]\[6\].

## Orkestrasi dengan Docker Compose

### Penjelasan Struktur dan Sintaks File Compose

#### Struktur Umum

File Compose ditulis dalam format **YAML** (*YAML Ain't Markup Language*) dan secara konvensi diberi nama `compose.yaml`. Secara umum, file Compose memiliki empat elemen tingkat atas (*top-level elements*) utama, yaitu `services`, `networks`, `volumes`, dan `configs`.

``` yaml
services:    # Mendefinisikan container yang akan dijalankan
  ...

networks:    # Mendefinisikan jaringan yang digunakan antar service
  ...

volumes:     # Mendefinisikan volume untuk penyimpanan data persisten
  ...

configs:     # Mendefinisikan konfigurasi eksternal yang digunakan service
  ...
```

#### Sintaks YAML Dasar

Sebelum memahami struktur file Compose lebih lanjut, penting untuk memahami beberapa aturan dasar penulisan YAML yang digunakan dalam file Compose.

**Pasangan Key-Value**
Setiap konfigurasi ditulis dalam bentuk pasangan *key-value* yang dipisahkan oleh tanda titik dua (`:`):

``` yaml
nama_kunci: nilai
```

**Indentasi**
YAML menggunakan **indentasi spasi** (bukan tab) untuk menunjukkan hierarki atau hubungan antar elemen. Umumnya digunakan kelipatan **2 spasi** untuk setiap tingkat indentasi:

``` yaml
services:
  web:              # Tingkat 1 (2 spasi)
    image: nginx    # Tingkat 2 (4 spasi)
```

**Daftar (List)**
Daftar atau array ditulis menggunakan tanda hubung (`-`) di awal setiap elemen:

``` yaml
ports:
  - "8080:80"
  - "443:443"
```

**Komentar**
Komentar diawali dengan tanda pagar (`#`) dan akan diabaikan oleh Docker Compose:

``` yaml
# Ini adalah komentar
services:
  web:
    image: nginx  # Ini juga komentar
```

#### Elemen `services`

Elemen `services` adalah elemen yang paling utama dalam file Compose. Setiap *service* merepresentasikan satu container yang akan dijalankan. Berikut adalah properti-properti yang umum digunakan dalam mendefinisikan sebuah *service*:

##### `image`

Menentukan Docker Image yang digunakan sebagai dasar container. Apabila image tidak tersedia secara lokal, Docker akan mengunduhnya secara otomatis dari Docker Hub.

``` yaml
services:
  web:
    image: nginx:latest
```

##### `build`

Digunakan apabila image perlu dibangun terlebih dahulu dari sebuah Dockerfile, bukan diambil langsung dari registry.

``` yaml
services:
  web:
    build:
      context: .          # Direktori yang berisi Dockerfile
      dockerfile: Dockerfile
```

##### `ports`

Memetakan port pada container ke port pada host, mengikuti format `<port_host>:<port_container>`.

``` yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

##### `environment`

Menetapkan variabel lingkungan (*environment variable*) yang akan tersedia di dalam container.

``` yaml
services:
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: appdb
```

##### `volumes`

Menghubungkan *volume* atau direktori pada host ke dalam container untuk keperluan penyimpanan data persisten.

``` yaml
services:
  db:
    image: postgres:15
    volumes:
      - db-data:/var/lib/postgresql/data  # Named volume
      - ./config:/etc/config              # Bind mount
```

##### `depends_on`

Menentukan urutan *startup* antar *service*. Docker Compose akan memastikan *service* yang menjadi dependensi telah berjalan sebelum menjalankan *service* yang membutuhkannya.

``` yaml
services:
  web:
    image: nginx
    depends_on:
      - db
  db:
    image: postgres:15
```

> **Catatan:** `depends_on` hanya memastikan urutan *startup* container, bukan memastikan bahwa layanan di dalam container tersebut telah siap menerima koneksi.

##### `networks`

Menentukan jaringan yang digunakan oleh sebuah *service* untuk berkomunikasi dengan *service* lainnya.

``` yaml
services:
  web:
    image: nginx
    networks:
      - frontend
```

##### `restart`

Menentukan kebijakan *restart* container apabila container berhenti atau mengalami kegagalan.

``` yaml
services:
  web:
    image: nginx
    restart: always
```

Pilihan nilai yang tersedia:

| Nilai | Keterangan |
|----|----|
| `no` | Container tidak akan di-*restart* secara otomatis (default) |
| `always` | Container selalu di-*restart* apabila berhenti |
| `on-failure` | Container di-*restart* hanya apabila berhenti karena error |
| `unless-stopped` | Container selalu di-*restart* kecuali dihentikan secara manual |

#### Elemen `networks`

Elemen `networks` digunakan untuk mendefinisikan jaringan kustom yang menghubungkan antar *service*. Apabila tidak didefinisikan secara eksplisit, Docker Compose akan membuat satu jaringan *default* secara otomatis untuk seluruh *service* dalam file Compose.

``` yaml
networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
```

Dengan mendefinisikan jaringan terpisah, Anda dapat mengontrol *service* mana saja yang dapat saling berkomunikasi. Sebagai contoh, *service* `web` dapat ditempatkan pada jaringan `frontend`, sementara *service* `db` hanya berada pada jaringan `backend`.

#### Elemen `volumes`

Elemen `volumes` digunakan untuk mendefinisikan *named volume* yang dapat digunakan bersama oleh beberapa *service*. Data yang tersimpan dalam *volume* akan tetap ada meskipun container dihentikan atau dihapus.

``` yamlvolumes:
  db-data:
    driver: local
  app-config:
    driver: local
```

Dengan mendefinisikan *named volume*, Anda dapat memastikan bahwa data penting seperti basis data atau file konfigurasi tetap tersedia meskipun container dihapus atau diganti.

#### Elemen `configs` dan `secrets`

Elemen `configs` digunakan untuk mengelola konfigurasi eksternal yang dapat diakses oleh service, sementara `secrets` digunakan untuk mengelola informasi sensitif seperti kata sandi atau kunci API. Kedua elemen ini membantu memisahkan konfigurasi dari kode aplikasi untuk meningkatkan keamanan dan fleksibilitas.

``` yaml
configs:
  app_config:
    file: ./config.json

secrets:
  db_password:
    file: ./secrets/db_password.txt
```

### Perintah Dasar Docker Compose

Berikut adalah perintah-perintah dasar yang umum digunakan dalam pengelolaan aplikasi dengan Docker Compose:

1.  **Menjalankan Aplikasi**
    Perintah ini akan membangun, membuat, dan memulai semua container yang didefinisikan dalam file Compose.

``` bash
docker compose up
```

Untuk menjalankan dalam mode latar belakang (*detached mode*):

``` bash
docker compose up -d
```

2.  **Menghentikan Aplikasi**
    Perintah ini akan menghentikan dan menghapus container yang dibuat oleh Compose.

``` bash
docker compose down
```

3.  **Melihat Status Service**
    Menampilkan status dari semua service yang dikelola oleh Compose.

``` bash
docker compose ps
```

4.  **Melihat Log Service**
    Menampilkan log dari service yang sedang berjalan.

``` bash
docker compose logs
# atau untuk service tertentu
docker compose logs <nama_service>
```

5.  **Membangun Ulang Image**
    Membangun atau membangun ulang image untuk service yang didefinisikan.

``` bash
docker compose build
```

6.  **Menjalankan Perintah dalam Service**
    Menjalankan perintah tertentu di dalam container dari sebuah service.

``` bash
docker compose exec <nama_service> <perintah>
```

7.  **Melihat Konfigurasi yang Dihasilkan**
    Menampilkan konfigurasi YAML yang telah diproses oleh Docker Compose.

``` bash
docker compose config
```

### Menjalankan Service berdasarkan File Compose

## Hands-on Docker Compose

Prerequisites:
- Instalasi Docker
- Docker Image (Dari Hands-on sebelumnya)
- Koneksi Internet
End Goal:
Kumpulan Service docker yang diorkestrasi oleh docker compose.
### Langkah-langkah Hands-on
Pada hands on kali ini, kita akan membuat sebuah compose yang terdiri dari dua layanan, yaitu image yang baru kita buat pada hands on sebelumnya, dan proxy nginx. Berikut adalah langkah-langkah yang harus dilakukan.
1. Masuk ke Direktori Proyek Sebelumnya
Masuk ke direktori proyek yang telah anda buat image nya pada hands on sebelumnya.

``` bash
cd hello-world-nextjs
```

2.  Buat File Compose
    Buat file baru bernama `compose.yaml` dalam direktori proyek anda saat ini

``` bash
touch compose.yaml
```

Lalu tulis isi file compose sesuai dengan petunjuk dibawah ini:

``` yaml
# Definisi semua service yang akan dijalankan
services:

  # Service 1: Aplikasi Next.js (static site) yang di-serve oleh nginx internal
  app:
    # Build image dari Dockerfile di direktori saat ini
    build: .
    # Restart otomatis kecuali container dihentikan secara manual
    restart: unless-stopped

  # Service 2: Nginx reverse proxy sebagai entry point utama
  proxy:
    # Menggunakan image nginx versi Alpine yang ringan
    image: nginx:alpine
    # Mapping port 3000 di host ke port 80 di container
    ports:
      - "3000:80"
    # Mount file konfigurasi nginx dari host ke container (read-only, dengan label SELinux)
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf:ro,z
    # Pastikan service app sudah berjalan sebelum proxy dimulai
    depends_on:
      - app
    # Restart otomatis kecuali container dihentikan secara manual
    restart: unless-stopped
```

1.  Jalankan Compose
    Jalankan compose dengan menjalankan perintah berikut

``` bash
docker compose up -d --build
```

<img
src="Docker%20dan%20Docker%20Compose-media/095153d71faa716a67f13e04f54beaeae7bbd815.png"
class="wikilink" alt="Pastedimage20260301081908.png" />
### Verifikasi Keberhasilan Hands-on
Untuk memverifikasi bahwa seluruh service telah berjalan, jalankan perintah berikut:

``` bash
docker compose ps
```

yang akan mengeluarkan output seperti berikut:
<img
src="Docker%20dan%20Docker%20Compose-media/9196998455df77c459b506041c3f12fbde7c3025.png"
class="wikilink" alt="Pastedimage20260301085028.png" />

Selain itu, anda dapat masuk ke `http://localhost:3000` untuk melakukan verifikasi. Anda seharusnya mendapatkan halaman berikut.
<img
src="Docker%20dan%20Docker%20Compose-media/6f461db0971a99bb5e5ce288fc60917fc84688d2.png"
class="wikilink" alt="Pastedimage20260301085646.png" />

### Troubleshooting Umum Docker Compose

Berikut adalah beberapa masalah yang umum dihadapi saat menggunakan Docker Compose beserta solusinya:

1.  **Service Tidak Dapat Saling Terhubung**
    - Pastikan semua service berada dalam jaringan yang sama atau telah didefinisikan dalam elemen `networks` yang sesuai.
    - Gunakan nama service sebagai hostname untuk komunikasi antar container, bukan `localhost`.
2.  **Volume Tidak Tersimpan dengan Benar**
    - Periksa path volume pada file Compose. Pastikan path relatif atau absolut sudah benar.
    - Untuk *named volume*, pastikan volume telah didefinisikan pada elemen tingkat atas `volumes`.
3.  **Port Conflict pada Host**
    - Jika port yang dipetakan sudah digunakan oleh aplikasi lain, ubah port host pada konfigurasi `ports`.
    - Contoh: Ubah `"3000:80"` menjadi `"8080:80"` untuk menghindari konflik.
4.  **Image Tidak Dibangun Ulang**
    - Docker Compose menggunakan cache image secara default. Gunakan flag `--build` untuk memaksa pembangunan ulang image.
    - Contoh: `docker compose up -d --build`
5.  **Variabel Lingkungan Tidak Terbaca**
    - Pastikan variabel lingkungan didefinisikan dengan benar pada elemen `environment` atau melalui file `.env`.
    - Gunakan perintah `docker compose config` untuk memverifikasi konfigurasi yang telah diproses.

------------------------------------------------------------------------

# Referensi

\[1\] Docker Inc. *What is Docker?* https://docs.docker.com/get-started/overview/
\[2\] Docker Inc. *Docker History*. https://www.docker.com/company/history/
\[3\] Docker Inc. *Docker Documentation*. https://docs.docker.com/
\[4\] Docker Inc. *About Docker Compose*. https://docs.docker.com/compose/
\[5\] Docker Inc. *Compose V2 Migration Guide*. https://docs.docker.com/compose/migrate/
\[6\] Microsoft. *Developing with Docker Compose*. https://learn.microsoft.com/en-us/dotnet/architecture/microservices/docker-application-development
