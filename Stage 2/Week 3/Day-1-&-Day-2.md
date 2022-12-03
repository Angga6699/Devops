# MEMBUAT VM DI IDCH
### Websitenya : www.idcloudhost.com

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/1.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/2.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/3.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/4.png)

# Install Ansible Terlebih Dahulu

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/5.png)

### sudo apt install ansible

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/6.png)

### ansible --version

# Membuat Direktori Untuk Menyimpan File Ansible

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/7.png)

### saya akan buat direktori bernama ansibleku

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/7.png)

# Membuat SSH-KEYGEN Supaya Login Tanpa Password

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/8.png)

### Copy isi dari id_rsa.pub dan pastekan di authorized_keys

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/11.png)

### Kemudian copy id_rsa dan buat file .pem pada local Server/ansibleku

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/12.png)

### Save Dan Exit

### Kemudian Coba Login

### chmod 400 angga.pem

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/14.png)

# Membuat FILE Inventory Ansible

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/15.png)

### [monitoring]
### 103.55.37.187 ansible_user=angga

# Membuat FILE Ansible.cfg

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/16.png)

### [defaults]
### inventory = Inventory
### private_key_file = angga.pem

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/17.png)

### ansible all -m ping

# Install nginx menggunakan ansible-playbook

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/18.png)

- hosts: monitoring
  become: yes
  gather_facts: yes
  tasks:
        - name: 'install nginx'
          apt: 
            name:
             - nginx
            state: latest

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/19.png)

### ansible-playbook nginx.yml

## cek pada web browser

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/20.png)

# Install Docker menggunakan ansible-playbook

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/21.png)

- hosts: monitoring
  become: yes
  gather_facts: yes
  tasks:
         - name: 'update'
           apt:
            update_cache: yes

         - name: 'upgrade'
           apt:
            upgrade: dist


         - name: 'install dependencies'
           apt:
             name:
             - ca-certificates
             - curl
             - gnupg
             - lsb-release

         - name: 'add docker gpg key'
           apt_key:
            url: https://download.docker.com/linux/ubuntu/gpg

         - name: 'add repository docker'
           apt_repository:
             repo: deb  https://download.docker.com/linux/ubuntu focal stable

         - name: 'install docker engine'
           apt: 
            name:
             - docker-ce
             - docker-ce-cli
             - containerd.io
             - docker-compose-plugin

         - name: 'update'
           apt:
            update_cache: yes

         - name: 'install docker-compose'
           shell: curl -SL https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose


         - name: 'set permision for docker'
           shell: sudo chmod +x /usr/local/bin/docker-compose
        
         - name: 'docker without sudo'
           shell: sudo usermod -aG docker angga

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/23.png)

### ansible-playbook --syntax-check docker.yml

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/24.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/25.png)

# cek hasil install docker pada server

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/27.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/28.png)

# Install Appserver dengan Node Exporter , Prometheus dan Grafana

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/29.png)

 global:
 scrape_interval: 10s
scrape_configs:
 - job_name: 'prometheus_metrics'
   scrape_interval: 5s
   static_configs:
     - targets: ['103.55.37.187:9090'] #Localhost:port
 - job_name: 'node_exporter_metrics'
   scrape_interval: 5s
   static_configs:  
     - targets: ['103.55.37.187:9100','103.55.37.185:9100'] #Localhost:port

# Membuat ansible-playbook untuk instalasi Node Exporter , Prometheus dan Grafana

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/30.png)

 - hosts: all
   become: yes
   gather_facts: yes
   tasks:
        - name: 'update'
          apt:
           update_cache: yes

        - name: 'upgrade'
          apt:
           upgrade: dist

        - name: 'Run Node Exporter'
          shell: docker run -d --net="host" --pid="host" -v "/:/host:ro,rslave" --name node_exporter quay.io/prometheus/node-exporter --path.rootfs=/host

 - hosts: monitoring
   tasks:
         - name: 'Make volume folder'
           file:
            path: /home/angga/prometheus
            state: directory

         - name: 'Copy Configuration Prometheus'
           copy:
            src: /home/angga/ansibleku/prometheus.yml
            dest: /home/angga/prometheus
        
         - name: 'install ptyhon docker'
           shell: sudo apt install python3-docker -y


         - name: 'Login DockerHub'
           docker_login:
            username:
            password: 

         - name: 'Pull Prometheus'
           docker_image:
            name: bitnami/prometheus
            source: pull
           

         - name: 'Container Prometheus'
           docker_container:
            name: prometheus
            image: bitnami/prometheus
            ports:
              - 9090:9090
            volumes: /home/angga/prometheus:/etc/prometheus

         - name: 'Pull Grafana'
           docker_image:
            name: grafana/grafana    
            source: pull

         - name: 'Container Grafana'
           docker_container:
            name: grafana
            image: grafana/grafana
            ports:
              - 3000:3000

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/32.png)

### ansible-playbook monitoring.yml

### kita cek pada docker di server appserver

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/34.png)

### cek juga pada web browser

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/35.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/36.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/37.png)

### Masukan username dan password default (admin) / (admin)

### Kemudian buat password baru sesuai keinginan kalian lalu submit

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/38.png)

### "DATA SOURCES"

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/39.png)

### Pilih Prometheus

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/40.png)

### Kemudian pada URL kalian isikan IP dan port dari Prometheus

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/42.png)

### Save & Test

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/43.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/44png)

### Pilih Dashboard Dan Pilih New Dashboard

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/45.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/46.png)

### Disini pilih metric apa yang mau kita monitoring kemudian run queries

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/48.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/50.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/51.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/52.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/53.png)

# Mengganti Tampilan Dashboard

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/54.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/55.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/56.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/59.png)

# Reverse Proxy untuk Monitoring

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/60.png)

# Masuk Ke Website Cloudflare Atau Login Dan Add Record DNS 
# Dan Masukan DNS DAN IP SERVER DARI KETIGA
# Yaitu Node Exportes, Prometheus, Grafana

### Masuk Ke Direktori /etc/nginx

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/61.png)

### Dan Buat Direktori Untuk File Reverse Proxy

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/62.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/63.png)

# Grafana

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/64.png)

# Node Exportter

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/65.png)

# Prometheus

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/66.png)

# Ketik Perintah cd .. Lalu Enter

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/67.png)

# Masuk Ke Direktori nginx.conf

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/68.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/69.png)

### Cek syntax

### sudo nginx -t

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/70.png)

### sudo systemctl restart nginx

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/71.png)

# Setup SSL dengan CertBot

1.Lakukan instalasi berikut

sudo apt-get install snapd
sudo snap install core; sudo snap refresh core
sudo apt-get install certbot python3-certbot-nginx

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/72.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/73.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/74.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/75.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/76.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/77.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/78.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/79.png)

2.Jalankan command ini untuk instalasi certificate secara otomatis

sudo certbot --nginx

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/80.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/81.png)

3.Dicek File Reverse Proxy


Grafana
![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/82.png)

Node Expotter

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/83.png)

Prometheus

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/84.png)

Dicek Apakah Sudah Menjadi Https???

Node Expotter

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/85.png)

Prometheus

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/86.png)

Grafana

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/87.png)

# Membuat User Dengan Menggunakan Ansible

sudo apt update

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/88.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/89.png)

sudo apt whois

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/90.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/91.png)

mkpasswd --method=sha-512

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/92.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/93.png)

Buat File use.yml Di Direktori ansibelku/file

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/94.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/95.png)

Cek Syntax

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/96.png)

Jalankan File User.yml

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/97.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/98.png)

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/100.png)

Login Dengan Menggunakan User Yang Tadi Dibuat Dengan Menggunakan Ansible

![](https://github.com/Angga6699/Week-3-Stage-2-Day-1-Dan-Day-2/blob/main/99.png)
