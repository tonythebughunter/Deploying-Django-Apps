# 🚀 Deploying a Django Project with Apache on Ubuntu

This guide helps you deploy a Django project using Apache and `mod_wsgi`.

## 📦 Step 1: Install Required Packages


sudo apt update
sudo apt install apache2 libapache2-mod-wsgi-py3

## 📂 Step 2: Move Your Project to Apache Directory


sudo cp -r /path/to/your/project /var/www/website
Your project should now be located at:
                        /var/www/website/
                        ├── db.sqlite3
                        ├── frontend/
                        ├── manage.py
                        ├── static/
                        └── webapp/         ← contains settings.py, wsgi.py

## ⚙️ Step 3: Create Apache Configuration File

sudo nano /etc/apache2/sites-available/website.conf
Paste the following config:

        <VirtualHost *:80>
            ServerName localhost
            ServerAdmin webmaster@localhost
            DocumentRoot /var/www/website/
        
            Alias /static /var/www/website/static
            <Directory /var/www/website/static>
                Require all granted
            </Directory>
        
            <Directory /var/www/website/webapp>
                <Files wsgi.py>
                    Require all granted
                </Files>
            </Directory>
        
            WSGIDaemonProcess website python-path=/var/www/website
            WSGIProcessGroup website
            WSGIScriptAlias / /var/www/website/webapp/wsgi.py
        </VirtualHost>

## ✅ Step 4: Enable the Site

sudo a2ensite website.conf

## 🛠️ Step 5: Update Django Settings.py

import os
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')

## 📦 Step 6: Collect Static Files

python3 manage.py collectstatic 

This collects all static files (CSS, JS, etc.) into one directory for Apache to serve. Apache cannot read .py files, so static assets need to be centralized.

## 🔒 Step 7: Fix File Permissions

sudo chown -R www-data:www-data /var/www/website

## 🚀 Step 8: Start Apache Server

sudo systemctl start apache2


## 📌 Notes
Your wsgi.py should be in: /var/www/website/webapp/wsgi.py

All apps (like frontend, etc.) live inside /var/www/website/

Apache only needs to know how to serve static files and hand the rest to Django via WSGI.




