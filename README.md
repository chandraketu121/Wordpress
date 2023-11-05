# Wordpress
To install and deploy WordPress on an Ubuntu server with Nginx and MySQL, you can follow these steps:

**1. Connect to your Ubuntu server:**
   You can use SSH to connect to your server. Open your terminal and use the following command, replacing "4.240.106.196" with your server's IP address:

   ```bash
   ssh ubuntu@ip
   ```

   You will be prompted to enter your password.

**2. Update the system:**
   It's a good practice to update your server's package list and upgrade existing packages. Run the following commands:

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

**3. Install Nginx:**
   To install Nginx, use the following command:

   ```bash
   sudo apt install nginx
   ```

   Start Nginx and enable it to start on boot:

   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

**4. Install MySQL:**
   You can install MySQL with the following command:

   ```bash
   sudo apt install mysql-server
   ```

   During the installation, you will be prompted to set a password for the MySQL root user. Make sure to choose a strong password and remember it.

**5. Install PHP:**
   Install PHP and some required extensions for WordPress:

   ```bash
   sudo apt install php-fpm php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-zip
   ```

**6. Configure Nginx:**
   Create an Nginx configuration file for your WordPress site. You can use the default configuration as a template:

   ```bash
   sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/wordpress
   ```

   Edit the configuration file with your favorite text editor:

   ```bash
   sudo nano /etc/nginx/sites-available/wordpress
   ```

   Replace the contents with the following configuration:

   ```nginx
   server {
       listen 80;
       server_name your_domain_or_IP;

       root /var/www/html;
       index index.php;

       location / {
           try_files $uri $uri/ /index.php?$args;
       }

       location ~ \.php$ {
           include snippets/fastcgi-php.conf;
           fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
       }

       location ~ /\.ht {
           deny all;
       }
   }
   ```

   Save the file and exit the text editor.

**7. Enable the Nginx site:**
   Enable the WordPress site by creating a symbolic link in the `sites-enabled` directory:

   ```bash
   sudo ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
   ```

**8. Test Nginx configuration:**
   Before restarting Nginx, it's a good idea to test the configuration to ensure there are no syntax errors:

   ```bash
   sudo nginx -t
   ```

   If the test is successful, you can proceed to the next step.

**9. Reload Nginx:**
   Reload Nginx to apply the changes:

   ```bash
   sudo systemctl reload nginx
   ```

**10. Create a MySQL database:**
   Log in to MySQL with the root password you set during installation:

   ```bash
   mysql -u root -p
   ```

   Create a new database and user for WordPress:

   ```sql
   CREATE DATABASE wordpress;
   CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'your_password';
   GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

   Replace `'your_password'` with a strong password for the WordPress user.

**11. Install WordPress:**
   Download and extract WordPress to the webroot directory:

   ```bash
   cd /var/www/html
   sudo wget https://wordpress.org/latest.tar.gz
   sudo tar -xzvf latest.tar.gz
   sudo mv wordpress/* .
   sudo rmdir wordpress
   ```

**12. Configure WordPress:**
   Create a configuration file for WordPress:

   ```bash
   cp wp-config-sample.php wp-config.php
   ```

   Edit the `wp-config.php` file to configure your database settings:

   ```bash
   nano wp-config.php
   ```

   Modify the following lines with your database information:

   ```php
   define('DB_NAME', 'wordpress');
   define('DB_USER', 'wordpressuser');
   define('DB_PASSWORD', 'your_password');
   define('DB_HOST', 'localhost');
   ```

   Save the file and exit the text editor.

**13. Set the correct permissions:**
   Ensure that the web server can read and write to the necessary directories:

   ```bash
   sudo chown -R www-data:www-data /var/www/html
   ```

**14. Complete the WordPress installation:**
   Open your web browser and navigate to your server's IP address or domain name. Follow the on-screen instructions to complete the WordPress installation.

**15. Secure your site:**
   It's essential to secure your WordPress site. Consider installing an SSL certificate and configuring HTTPS, setting up a firewall, and implementing security plugins.

That's it! You've successfully installed and deployed WordPress on an Ubuntu server with Nginx and MySQL. Remember to keep your system and WordPress installation up to date and regularly back up your data.
