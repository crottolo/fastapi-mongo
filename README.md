# servizioenergiasync

installare mongodb\
sudo apt install mongodb\
configurare l'accesso solo come localhost\

creare la cartella del progetto o scaricare la repository e poi entrare\
creare il virtualenv python3 -m venv .venv\
attivare il virtualenv source .venv/bin/activate\
installare le dipendenze pip install -r requirements.txt\
creare environment file cp .env.example .env\
modificare il file .env con i parametri di configurazione\
settare password e username di mongodb\
settare il token di accesso per l'api\
settare il nome del database\

creazione del servizio gunicorn\
sudo nano /etc/systemd/system/fastapi.service\

```
[Unit]
Description=FASTAPI service
After=network.target

[Service]
User=crotti
Group=crotti
WorkingDirectory=/home/crotti/fastapi-mongo
Environment="PATH=/home/crotti/fastapi/.venv/bin"
ExecStart=/home/crotti/fastapi/.venv/bin/gunicorn -c /home/crotti/fastapi/gunicorn_conf.py main:app
ExecReload=/bin/kill -s HUP $MAINPID
KillMode=mixed
TimeoutStopSec=5
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
sudo systemctl daemon-reload\
sudo systemctl enable fastapi.service\
sudo systemctl start fastapi.service\
