# Pertemuan Minggu-04

## Docker - Menghubungkan antar jaringan

>[https://katacoda.com/courses/docker/5](https://katacoda.com/courses/docker/5)

### Step 1 - Start Redis

Umumnya dalam implementasi `links` adalah menghubungkan container (aplikasi) dengan data-store container, praktikum kali ini yaitu menggunakan docker image _redis data store_. Secara default ketika menjalankan sebuah container, docker akan membuat nama untuk container yang dijalankan. Untuk kemudahan dalam penggunaan `links` definisikan `name` container yang dijalankan.

```markdown
docker run -d --name redis-server redis
```
![01](images/1.jpg)
### Step 2 - Create Link

Untuk menghubungkan dengan container asal gunakan opsi `--link <container-name|id>:<alias>` ketika menjalankan container yang baru.

> Bagaimana Link bekerja?

Pertama, docker akan mengeset beberapa environment variable berdasarkan container sumber. Untuk menampilkan environment variable gunakan perintah: `docker run --link redis-server:redis alpine env`

![02](images/2.jpg)

Selanjutnya, Docker akan melakukan update terhadap file `HOSTS` pada container dengan 3 variable yaitu: _the original, the alias and the hash-id_

jalankan perintah `docker run --link redis-server:redis alpine cat /etc/hosts`

![03](images/3.jpg)

Ketika link sudah terbuat lakukan ping terhadap container dengan perintah `docker run --link redis-server:redis alpine ping -c 1 redis`

![04](images/4.jpg)

### Step 3 - Connect to app

Dengan Link yang dibuat, aplikasi dapat terhubung dan berkomunikasi dengan sumber container dengan cara biasa, terlepas dari kenyataan bahwa kedua layanan berjalan dalam container.

Berikut adalah aplikasi node.js sederhana yang terhubung ke redis menggunakan redis hostname.

```docker run -d -p 3000:3000 --link redis-server:redis katacoda/redis-node-docker-example```

![05](images/5.jpg)

> Test Koneksi

Mengirim permintaan HTTP ke aplikasi akan menyimpan permintaan di Redis dan mengembalikan hitungan. Jika Anda mengeluarkan beberapa permintaan, Anda akan melihat kenaikan penghitung saat item tetap ada.

```curl docker:3000```

![06](images/6.jpg)

### Step 4 - Connect to Redis CLI

Dengan cara yang sama,kita dapat menghubungkan Container ke aplikasi, kita juga dapat menghubungkannya dengan tools CLI mereka sendiri.

>Meluncurkan Redis CLI
Perintah di bawah ini akan meluncurkan sebuah instance dari  Redis-cli-tool dan terhubung ke server redis melalui alias-nya
```docker run -it --link redis-server:redis redis redis-cli -h redis redis:6379```

![07](images/7.jpg)

## Docker - Networks

>[https://katacoda.com/courses/docker/networking-intro](https://katacoda.com/courses/docker/networking-intro)