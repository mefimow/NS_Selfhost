#requirements: install Debian 9, connect to internet, update, forward HTTP, HTTPS, SSH if nessesary. all of operation were made from root
#Данное руководство не претендует на полноту и использование best practices, предварительно необходимо установить Debian 9, подключить его к интернету, обновить, пробросить HTTP, HTTPS, SSH
# все манипуляции проводились под пользователем root. Необходимы минимальные навыки использования редактора nano

#enable ssh for root if nessesary
#разрешаем ssh для root если необходимо

cd /etc/ssh
nano sshd_config

#add string "PermitRootLogin yes" 
#добавляем строку "PermitRootLogin yes"

#save file
#сохраняем

systemctl restart ssh

#go to putty if u want
#переходим для удобства в putty

apt-get install sudo
apt-get -y install curl
apt-get -y install mc #not necessary  #не обязательно

#for Hyper-V install integration services 
#для Hyper-V ставим средства интеграции

nano /etc/initramfs-tools/modules

#add strings
#добавляем строки

hv_vmbus
hv_storvsc
hv_blkvsc
hv_netvsc

#save file
#сохраняем

sudo apt-get -y install hyperv-daemons
update-initramfs -u
reboot

#for vmware 
#для vmware

apt-get install -y open-vm-tools open-vm-tools-desktop
vmware-user-suid-wrapper
reboot

#validate actual release of mongo https://www.mongodb.com/download-center/community . if the version of mongo is different of 4.2 - actualise it bellow
#смотрим актуальный релиз mongo https://www.mongodb.com/download-center/community и при наличии более свежей версии меняем значения ниже

curl https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
nano /etc/apt/sources.list.d/mongodb-org-4.2.list

#add string
#добавляем строку 

deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.2 main

#save file 
#сохраняем

apt-get update
apt-get -y install mongodb-org
systemctl enable mongod
systemctl start mongod

#create DB - change username and pass
#создаем пользователя БД - имя и пароль свои

mongo
> use Nightscout
> db.createUser({user: "username", pwd: "password", roles:["readWrite"]})
> quit()

#export database from existing database on mongo. adress, user, pass and port you can get from environment (heroku or azure) of existing NS
#mongoexport must be run from your OS command shell, not in mongo shell.
#Экспорт из существующей БД на монго. адрес, пользователь, пароль и порт берем из переменных среды на существующем heroku or azure
#mongoexport запускается из командной строки ОС

mongodump -h _some_adress_from_env.mlab.com --port _port_from_env_ -d _DB_name_from_env_  --username _user_from_env_ --password _password_from_env_
mongorestore -d Nightscout dump/_DB_name_from_env_


apt-get -y install git

apt-get -y install curl software-properties-common
curl -sL https://deb.nodesource.com/setup_10.x | sudo bash -
apt-get install -y nodejs

apt-get install -y gcc

apt-get install -y build-essential

#go to folder where u want to install. In this case /opt
#Перходим в папку установки - может быть произвольной. В данном варианте /opt

cd /opt 
mkdir nightscout
cd nightscout
git clone https://github.com/nightscout/cgm-remote-monitor.git
cd cgm-remote-monitor
npm install --unsafe-perm

#environment
#переменные среды

#if you want - add to global environment - nano /etc/environment
# or create you own file with environments - nano start.sh
# можно прописать в глобальных системных переменных nano /etc/environment 
# либо создаем файлик nano start.sh

#!/bin/bash
export DISPLAY_UNITS="mg/dl"
export MONGO_CONNECTION="mongodb://username:password@localhost:27017/Nightscout"
export PORT=1337
export API_SECRET="Api_Secret_min_12_symbols"
export PUMP_FIELDS="reservoir battery status"
export DEVICESTATUS_ADVANCED=true
export ENABLE="careportal basal cage sage boluscalc rawbg iob bwp bage mmconnect bridge openaps pump iob maker"
export TIME_FORMAT=24
export BASE_URL="YOURS_INTERNET_URL.RU"
export INSECURE_USE_HTTP=true


export ALARM_HIGH=on
export ALARM_LOW=on
export ALARM_TIMEAGO_URGENT=on
export ALARM_TIMEAGO_URGENT_MINS=30
export ALARM_TIMEAGO_WARN=on
export ALARM_TIMEAGO_WARN_MINS=15
export ALARM_TYPES=simple
export ALARM_URGENT_HIGH=on
export ALARM_URGENT_LOW=on
export AUTH_DEFAULT_ROLES=denied
export BG_HIGH=180
export BG_LOW=72
export BG_TARGET_BOTTOM=90
export BG_TARGET_TOP=162
export BRIDGE_MAX_COUNT=1
export BRIDGE_PASSWORD=
export BRIDGE_SERVER=EU
export BRIDGE_USER_NAME=
export CUSTOM_TITLE=MyTitle
export DISABLE=
export MONGO_COLLECTION=entries
export NIGHT_MODE=on
export OPENAPS_ENABLE_ALERTS=true
export OPENAPS_FIELDS='status-symbol status-label iob meal-assist rssi'
export OPENAPS_RETRO_FIELDS='status-symbol status-label iob meal-assist rssi'
export OPENAPS_URGENT=60
export OPENAPS_WARN=20
#export PAPERTRAIL_API_TOKEN=some_token
export PUMP_ENABLE_ALERTS=true
export PUMP_FIELDS='battery reservoir clock status'
export PUMP_RETRO_FIELDS='battery reservoir clock status'
export PUMP_URGENT_BATT_V=1.3
export PUMP_URGENT_CLOCK=30
export PUMP_URGENT_RES=10
export PUSHOVER=
export SHOW_FORECAST=openaps
export SHOW_PLUGINS='openaps pump iob sage cage careportal'
export SHOW_RAWBG=noise
export THEME=colors

node server.js     # start server

#save file
#сохраняем

chmod +x start.sh
./start.sh


#after start ./start.sh in few minutes you must see in loop
#после запуска ./start.sh через несколько минут в цикле должно появится

#reloading sandbox data
#all buckets are empty
#For the Basal plugin to function you need a treatment profile
#OpenAPS hasn't reported a loop yet
#WS: running websocket.update
#delta calculation indicates no new data is present
#tick 2019-11-28T10:28:28.794Z
#Load Complete:
#

#after that go to http://ip_of_debian:1337 - NS must open with api_secret dilog box
#после чего можно пробовать заходить http://ip_of_debian:1337 - должен открыться НС и попросить api_secert 

#installing autorun 
# настраиваем автозапуск
  nano /etc/systemd/system/nightscout.service
# add strings
# вставляем строки
	[Unit]
	Description=Nightscout Service      
	After=network.target

	[Service]
	Type=simple
	WorkingDirectory=/opt/nightscout/cgm-remote-monitor
	ExecStart=/opt/nightscout/cgm-remote-monitor/start.sh

	[Install]
	WantedBy=multi-user.target

#save file
# cохраняем  

#enable autorun
# разрешаем автозапуск
	systemctl daemon-reload
	systemctl enable nightscout.service
	systemctl start nightscout.service 

# verifying if the service is started
# проверка, запущен ли сервис:

   systemctl status nightscout.service 
 
#installation and configuration a reverse proxy
#установка и настройка обратного прокси

	apt-get install nginx
	systemctl enable nginx

#configuration nginx
#конфиг ngnix
	nano /etc/nginx/sites-available/default
#add strings
#добавляем строки

server {
   listen 80;
   server_name YOURS_INTERNET_URL.RU;
   root /usr/share/nginx/html;
   location ~ /.well-known {
               allow all;
   }
  } 
server {
   listen 443 ssl;
   server_name YOURS_INTERNET_URL.RU;
   root /usr/share/nginx/html;

#       ssl_certificate     /etc/letsencrypt/live/YOURS_INTERNET_URL.RU/fullchain.pem;
#       ssl_certificate_key /etc/letsencrypt/live/YOURS_INTERNET_URL.RU/privkey.pem;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_session_timeout 1d;
        ssl_session_cache shared:SSL:50m;
        ssl_stapling on;
        ssl_stapling_verify on;
        add_header Strict-Transport-Security max-age=15768000;
        resolver 8.8.8.8 8.8.4.4 valid=300s;

       location ~ /.well-known {
               allow all;
       }
       location / {
          proxy_pass http://127.0.0.1:1337;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }


}
#save ctrl+x
#сохраняем ctrl+x

# get the certificate
# получение сертификата
	sudo apt-get install certbot
	certbot certonly --webroot --agree-tos --email youmail@mail.com -w /usr/share/nginx/html/ -d YOURS_INTERNET_URL.RU 
	openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
#in configuration file
# в конфиг ngnix 
	nano /etc/nginx/sites-available/default

#in section 'server' add the strings
#в секцию server добавляем строки
	ssl_certificate     /etc/letsencrypt/live/YOURS_INTERNET_URL.RU/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/YOURS_INTERNET_URL.RU/privkey.pem;

#restart nginx
#перезапуск nginx
	systemctl reload nginx

#check startup nginx
#проверка запуска ngnix
	systemctl status nginx

# NightScout must be available at https://YOURS_INTERNET_URL.RU
# NightScout доступен по адресу https://YOURS_INTERNET_URL.RU
