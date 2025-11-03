# INSTALASI GENIEACS PADA UBUNTU 22.04
## Instalasi Ubuntu Server 22.04
setelah melakukan instalasi Ubuntu Server 22.04 update dan upgrade
```
sudo apt update
```
```
sudo apt upgrade -y
```
Install CURL
```
sudo apt install curl
```

## Instalasi Node JS
```
curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
```
```
sudo bash nodesource_setup.sh
```
```
sudo apt install nodejs
```
cek apakah nodejs dan npm telah terinstall
```
node -v
```
```
npm -v
```
## Instalasi MongoDB
```
curl -fsSL https://www.mongodb.org/static/pgp/server-6.0.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/mongodb-6.gpg
```
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```
``` 
sudo apt update
```
```
sudo apt install -y mongodb-org
```
```
sudo systemctl start mongod
```
```
sudo systemctl enable mongod
```

## Instalasi Genieacs
[Dokumentasi](https://docs.genieacs.com/en/latest/installation-guide.html)

```
sudo npm install -g genieacs@1.2.13
```
```
sudo useradd --system --no-create-home --user-group genieacs
```
```
sudo mkdir /opt/genieacs
```
```
sudo mkdir /opt/genieacs/ext
```
```
sudo chown genieacs:genieacs /opt/genieacs/ext
```
Buat file genieacs.env
```
sudo nano /opt/genieacs/genieacs.env
```
Isi file tersebut dengan parameter sebagai berikut:
```
GENIEACS_CWMP_ACCESS_LOG_FILE=/var/log/genieacs/genieacs-cwmp-access.log
GENIEACS_NBI_ACCESS_LOG_FILE=/var/log/genieacs/genieacs-nbi-access.log
GENIEACS_FS_ACCESS_LOG_FILE=/var/log/genieacs/genieacs-fs-access.log
GENIEACS_UI_ACCESS_LOG_FILE=/var/log/genieacs/genieacs-ui-access.log
GENIEACS_DEBUG_FILE=/var/log/genieacs/genieacs-debug.yaml
NODE_OPTIONS=--enable-source-maps
GENIEACS_EXT_DIR=/opt/genieacs/ex
```
Simpan dengan ctrl+x kemudia Y kemudian Enter

Login ke Super user
```
sudo su -
```
```
node -e "console.log(\"GENIEACS_UI_JWT_SECRET=\" + require('crypto').randomBytes(128).toString('hex'))" >> /opt/genieacs/genieacs.env
```
```
exit
```
```
sudo chown genieacs:genieacs /opt/genieacs/genieacs.env
```
```
sudo chmod 600 /opt/genieacs/genieacs.env
```
```
sudo mkdir /var/log/genieacs
```
```
sudo chown genieacs:genieacs /var/log/genieacs
```

Genieacs Services
```
sudo systemctl edit --force --full genieacs-cwmp
```
ketikkan paramter ini
```
[Unit]
Description=GenieACS CWMP
After=network.target

[Service]
User=genieacs
EnvironmentFile=/opt/genieacs/genieacs.env
ExecStart=/usr/bin/genieacs-cwmp

[Install]
WantedBy=default.target
```
Simpan dengan ctrl+x kemudia Y kemudian Enter

```
sudo systemctl edit --force --full genieacs-nbi
```
ketikkan parameter ini
```
[Unit]
Description=GenieACS NBI
After=network.target
[Service]
User=genieacs
EnvironmentFile=/opt/genieacs/genieacs.env
ExecStart=/usr/bin/genieacs-nbi
[Install]
WantedBy=default.target
```    
Simpan dengan ctrl+x kemudia Y kemudian Enter

```    
sudo systemctl edit --force --full genieacs-fs
```
ketikkan parameter ini
```
[Unit]
Description=GenieACS FS
After=network.target
[Service]
User=genieacs
EnvironmentFile=/opt/genieacs/genieacs.env
ExecStart=/usr/bin/genieacs-fs
[Install]
WantedBy=default.target
```
Simpan dengan ctrl+x kemudia Y kemudian Enter

```
sudo systemctl edit --force --full genieacs-ui
```
ketikkan parateter ini
```    
[Unit]
Description=GenieACS UI
After=network.target
[Service]
User=genieacs
EnvironmentFile=/opt/genieacs/genieacs.env
ExecStart=/usr/bin/genieacs-ui
[Install]
WantedBy=default.target
```
Simpan dengan ctrl+x kemudia Y kemudian Enter

## Restore Config Genieacs pada repositori ini.
```
git clone https://github.com/ariemus-smk/genieacs.git
```
```
cd genieacs/genieacs-update
```
```
sudo mongorestore --db genieacs --drop genieacs
```