# Install required Packages
sudo apt update
sudo apt install apache2 libapache2-mod-wsgi-py3
# Move your project files & directory to /var/www/
sudo cp -r /path/to/your/project /var/www/your-website
# Create a configuration file
sudo nano /etc/apache2/sites-available/website.conf:


  <VirtualHost *:80>
    ServerName localhost

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your-website/

    Alias /static /var/www/your-website/static
    <Directory /var/www/your-website/webapp/static>
        Require all granted
    </Directory>

    <Directory /var/www/your-website/webapp>
        <Files wsgi.py>
            Require all granted
        </Files>
    </Directory>

    WSGIDaemonProcess website python-path=/var/www/your-website
    WSGIProcessGroup website
    WSGIScriptAlias / /var/www/your-website/webapp/wsgi.py
</VirtualHost>


sample of what is in /var/www/your-website:   db.sqlite3  frontend  manage.py  static  webapp. webapp is where settings are
# Enable the site
sudo a2ensite website.conf
# Set the following in settigs.py
import os
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
# Collect static files
python3 manage.py collectstatic
-this makes a directory containing files that aache can read since it cannot directly read .py files
# Fix permissions
sudo chown -R www-data:www-data /var/www/website
# Start the Server
sudo systemctl start apache2


