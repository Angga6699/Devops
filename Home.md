# Membuat VM IDCH Di Websitenya : www.idcloudhost.com

### 1.Masuk Ke Webnya IDCh Dan Login Atau Daftar Terlebih Dahulu

![](https://github.com/Angga6699/Stage2-Day1/blob/main/1.png)

### 2.Klik Menu Compute Kemudian Klik Menu New

![](https://github.com/Angga6699/Stage2-Day1/blob/main/3.png)

### 3.Buat VM IDCH Dengan Ketentuan Sebagai Berikut : 

![](https://github.com/Angga6699/Stage2-Day1/blob/main/4.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/5.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/6.png)

### 4.Selesai Membuat VM IDCH

![](https://github.com/Angga6699/Stage2-Day1/blob/main/6.png)

### 5.Akses VM IDCH yang Tadi Dibuat Dengan Menggunakan ssh

![](https://github.com/Angga6699/Stage2-Day1/blob/main/7.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/8.png)

# Set Up Dumbflix

### 1.Lakukan Perintah ini Untuk Meninstall Curl : 

### curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

![](https://github.com/Angga6699/Stage2-Day1/blob/main/10.png)

### exec bash

![](https://github.com/Angga6699/Stage2-Day1/blob/main/11.png)

### nvm install 10

![](https://github.com/Angga6699/Stage2-Day1/blob/main/12.png)

# Set Up pm2

### 1.npm install -g pm2

![](https://github.com/Angga6699/Stage2-Day1/blob/main/13.png)

### 2.git clone https://github.com/dumbwaysdev/dumbflix-frontend.git

![](https://github.com/Angga6699/Stage2-Day1/blob/main/14.png)

### 3.Masuk ke direktori lalu jalankan npm install

### cd dumbflix-frontend/
### npm install

![](https://github.com/Angga6699/Stage2-Day1/blob/main/15.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/16.png)

### 4.Jalankan Perintah Berikut : 

### pm2 init simple

![](https://github.com/Angga6699/Stage2-Day1/blob/main/17.png)

### 5.Edit file ecosystem lalu rubah bagian script menjadi npm start

![](https://github.com/Angga6699/Stage2-Day1/blob/main/18.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/29.png)

### 5.Jalankan script dengan pm2

### pm2 start ecosystem.config.js

![](https://github.com/Angga6699/Stage2-Day1/blob/main/30.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/31.png)

### 6.Akses Di Web Browser Menggunakan ip:3000(portnya)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/34.png)

# Setup DNS Cloudflare

### Buat VM Untuk Server Nginx

![](https://github.com/Angga6699/Stage2-Day1/blob/main/36.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/38.png)

### 1.Pertama, kita akan membuat dns record dulu di cloudflare

![](https://github.com/Angga6699/Stage2-Day1/blob/main/40.png)

### Isikan Ipv4 address dengan ip server nginx

![](https://github.com/Angga6699/Stage2-Day1/blob/main/43.png)

### 2.Lakukan instalasi nginx di server

### sudo apt-get update
### sudo apt-get install nginx

![](https://github.com/Angga6699/Stage2-Day1/blob/main/44.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/45.png)

### 3.Buat file konfigurasi baru di folder /etc/nginx/sites-enabled

### sudo nano /etc/nginx/sites-enabled/rp-dumbflix.conf

### Lalu isikan berikut

server{
    server_name <url-domain>;
    location / {
        proxy_pass http://<ip-server>:<port>;
    }
}

![](https://github.com/Angga6699/Stage2-Day1/blob/main/46.png)

![](https://github.com/Angga6699/Stage2-Day1/blob/main/47.png)

### 4.sudo nginx -t

![](https://github.com/Angga6699/Stage2-Day1/blob/main/48.png)

### 5.Lakukan reload / restart ke service nginx

### sudo systemctl reload nginx

![](https://github.com/Angga6699/Stage2-Day1/blob/main/50.png)

### 6.Cek url melalui browser

![](https://github.com/Angga6699/Stage2-Day1/blob/main/53.png)