# INSTALACION DE ODOO 16 LIMPIO UBUNTU 22.04
## INSTALACION DE ODOO 16 LIMPIO EN UBUNTU 22.04 SIN POSTGRESQL 🤖👨🏽‍💻

_Pasos para instalar Odoo sin postgreSQL y sin librerias innecesarias_
Cambiar de version si se desea otra version de Odoo

## 1. Requisitos y Sources :bookmark_tabs:
```
echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list
wget -O - https://nightly.odoo.com/odoo.key | apt-key add -
echo "deb http://nightly.odoo.com/16.0/nightly/deb/ ./" >> /etc/apt/sources.list

sudo apt update
sudo apt-get update
```

## 2. WKHTMLTOPDF para reportes :page_facing_up:
```
sudo apt-get install libssl1.1
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb
sudo apt install ./wkhtmltox_0.12.6-1.focal_amd64.deb
```

## 3. Instalacion Odoo :desktop_computer:
Aqui se instala desde el repositorio previamente cargado Odoo
```
sudo apt install odoo
sudo systemctl enable --now odoo
sudo mkdir -p /mnt/extra-addons/
```

## 4. Configuracion de Odoo
Editar el archivo de configuracion de Odoo de acuerdo a necesidad
```
sudo nano /etc/odoo/odoo.conf
```
```
[options]
; This is the password that allows database operations:
admin_passwd = MasterPassword
db_host = IP_REMOTE_POSRGRESQL_SERVER
db_port = PORT
db_user = DB_USER
db_password = DB_PASS 
addons_path = /usr/lib/python3/dist-packages/odoo/addons, /mnt/extra-addons
```
## 5. Configuracion de Odoo
```
sudo service odoo restart
```

Odoo queda configurado y listo en el puerto 8069

## OPCIONAL 1: Configurar Nginx como reverse Proxy 👨🏽‍💻
Seguir los pasos del repo 

_https://github.com/mamfredym/nginx_odoo_

Nginx
```
sudo apt-get install nginx
cd /etc/nginx/sites-available
git clone https://github.com/mamfredym/nginx_odoo
cd nginx_odoo/
sudo cp /etc/nginx/sites-available/nginx_odoo/default.conf /etc/nginx/sites-available/default.conf
cd ..
mv default default-temp
mv default.conf default
nginx -s reload
```
_Abrir archivo de config de Odoo_
```
nano /etc/odoo/odoo.conf
```
_Cambiar/Adicionar estos parametros en el archivo de configuracion de Odoo_
```
xmlrpc_interface = 127.0.0.1
netrpc_interface = 127.0.0.1
proxy_mode = True
```
_Reiniciar Odoo_
```
sudo service odoo restart
```

## OPCIONAL 2: Instalar certificados SSL con CERTBOT 🤖
Seguir los pasos del webiste de CertBot

_https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal_
```
sudo snap install core; sudo snap refresh core
sudo apt-get remove certbot
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
sudo certbot certonly --nginx
sudo certbot renew --dry-run
```
