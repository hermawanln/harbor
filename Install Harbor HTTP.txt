1. Install Docker
2. Install Docker Engine
3. Install Docker Compose

#Install Harbor
---------------
$ wget https://github.com/goharbor/harbor/releases/download/v2.4.1/harbor-offline-installer-v2.4.1.tgz
$ tar xzvf harbor-offline-installer-v2.4.1.tgz harbor/
$ cd harbor
$ cp harbor.yml.tmpl harbor.yml
$ sudo nano harbor.yml

update dibawah
______________
hostname: <ip_host>:<port>

ex -> hostname : 172.23.251.141:8930
______________

comment command dibawah
_______________________
# https related config
#https:
  # https port for harbor, default is 443
#  port: 443
#  # The path of cert and key files for nginx
#  certificate: /your/certificate/path
#  private_key: /your/private/key/path
_______________________

$ sudo touch /etc/docker/daemon.json
$ sudo nano /etc/docker/daemon.json

tambahkan dibawah (sesuai hostname di harbor.yml)
_________________
{
"insecure-registries" : ["<ip_host>:<port>"]
}
_________________

$ sudo systemctl daemon-reload
$ sudo systemctl restart docker

$ sudo ./prepare --with-trivy --with-chartmuseum

$ sudo nano docker-compose.yml

CTRL + W -> ketik "proxy:"
ubah port: dibawah proxy 
dari 80:8080 menjadi <port>:8080 (sesuai hostname di harbor.yml

$ sudo docker-compose up -d

# Push dan Pull Images 
----------------------
$ sudo docker login <ip_host>:<port> (sesuai hostname di harbor.yml)
masukkan username & password seperti di harbor webpage

$ sudo docker tag alpine:latest 172.23.251.141:8930/library/alpine:latest
note : "library" adalah nama project yang ada di harbor, jika belum dibuat dapat ditambahkan lewat harbor webpage

$ sudo docker push 172.23.251.141:8930/library/alpine:latest

$ sudo docker pull 172.23.251.141:8930/library/alpine:latest