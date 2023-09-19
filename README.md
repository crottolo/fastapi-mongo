# FASTAPI-MONGODB
## Ubuntu 22.04
# installare mongodb
```
sudo apt update && sudo apt upgrade\
sudo apt-get install gnupg curl\
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
sudo systemctl daemon-reload
sudo systemctl enable mongod
sudo systemctl status mongod
sudo systemctl stop mongod

sudo nano /etc/mongod.conf
```
in questo file modificare la riga 19 e 20
```
# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1
```
configurare l'accesso solo come localhost

```
sudo apt install python3-pip
sudo apt install python3-venv
sudo apt install python3.10-venv
```
clonare la repository 
```
git clone https://github.com/crottolo/fastapi-mongo.git
entrare nella cartella
# cd fastapi-mongo
```
creare e attivare il virtualenv
```
python3 -m venv .venv
source .venv/bin/activate
```

installare le dipendenze 
```
pip install -r requirements.txt\
```
creare environment file cp .env.example .env\
modificare il file .env con i parametri di configurazione\
settare password e username di mongodb (opzionale)\
settare il token di accesso per l'api dentro .env\
settare il nome del database nel file db.py\


creazione del servizio gunicorn
verificare la cartella del progetto
```
pwd
sudo nano /etc/systemd/system/fastapi.service
```
esempio:
*home/crottolo/fastapi-mongo

```
[Unit]
Description=FASTAPI service
After=network.target

[Service]
User=crotti
Group=crotti
WorkingDirectory=/home/crotti/fastapi-mongo
Environment="PATH=/home/crotti/fastapi-mongo/.venv/bin"
ExecStart=/home/crotti/fastapi-mongo/.venv/bin/gunicorn -c /home/crotti/fastapi-mongo/gunicorn_conf.py main:app
ExecReload=/bin/kill -s HUP $MAINPID
KillMode=mixed
TimeoutStopSec=5
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
sudo systemctl daemon-reload

sudo systemctl enable fastapi.service

sudo systemctl start fastapi.service

sudo systemctl status fastapi.service

verificare se con status viene creato il file di log, se ci fosse un errore creare la cartella log
