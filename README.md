# WordPress Deployment on Apache Server with Let's Encrypt SSL Configuration

This repository provides a comprehensive guide for deploying WordPress on an Apache server, along with configuring Let's Encrypt SSL for secure communication. Each step is outlined with clear instructions and corresponding bash commands, ensuring a smooth setup process.

### Step 0: Update and upgrade packages
```bash
sudo apt update
sudo apt upgrade -y
```

### Step 1: Add a new user
```bash
sudo adduser username
sudo usermod -aG sudo username
```

### Step 2: Configure SSH
```bash
sudo nano /etc/ssh/sshd_config
sudo systemctl restart sshd
```

### Step 3: Install Apache
```bash
sudo apt install apache2 -y
sudo systemctl reload apache2
```

### Step 4: Configure firewall
```bash
sudo ufw allow 'Apache Full'
sudo ufw allow OpenSSH
sudo ufw allow 'https'  
sudo ufw enable
```

### Step 5: Install PHP and required modules
```bash
sudo apt install php libapache2-mod-php php-mysql php-redis php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y
```

### Step 6: Install Let's Encrypt for SSL
```bash
sudo apt install certbot python3-certbot-apache -y
```

### Step 7: Create Apache virtual host configuration
```bash
sudo nano /etc/apache2/sites-available/yourdomain.com.conf 
```
#### Add the virtual host configuration block mentioned in your setup
```bash
<VirtualHost *:80>
    ServerName yourdomain.com 
    ServerAlias www.yourdomain.com 
    DocumentRoot /var/www/yourdomain.com 
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory /var/www/yourdomain.com/> 
        AllowOverride All
    </Directory>
</VirtualHost>
```

### Step 8: Enable virtual host and SSL, and reload Apache
```bash
sudo a2ensite yourdomain.com 
sudo certbot --apache
sudo systemctl reload apache2
```

### Step 9: Create directory for WordPress installation
```bash
sudo mkdir /var/www/yourdomain.com
```

### Step 10: Download and extract WordPress
```bash
sudo wget -c http://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz -C /var/www/yourdomain.com --strip-components=1  
sudo rm latest.tar.gz
```

### Step 11: Set permissions
```bash
sudo chown -R www-data:www-data /var/www/yourdomain.com 
sudo chmod -R 775 /var/www/yourdomain.com 
```

### Step 12: Modify Apache configuration for WordPress directory
```bash
sudo nano /etc/apache2/apache2.conf
```
#### Update AllowOverride directive
```bash
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride no -> All # Update no to All
        Require all granted
</Directory>
```

### Step 13: MySQL setup
```bash
sudo apt install mysql-server -y
sudo mysql_secure_installation
sudo mysql -u root -e "CREATE DATABASE your-database-name;"
sudo mysql -u root -e "CREATE USER 'your-database-username'@'localhost' IDENTIFIED BY 'YourSecurePassword';"
sudo mysql -u root -e "GRANT ALL PRIVILEGES ON your-database-username.* TO 'your-database-name'@'localhost';"
sudo mysql -u root -e "FLUSH PRIVILEGES;"

Follow these steps carefully to configure your Apache servers on Ubuntu AWS. Happy coding!
