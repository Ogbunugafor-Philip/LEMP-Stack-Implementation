# LEMP-Stack-Implementation

## LEMP Stack Implementation

### Introduction
This project demonstrates the implementation of a LEMP stack (Linux, Nginx, MySQL, PHP) to build a dynamic web application. The goal is to create a web-based application that allows users to manage tasks efficiently by leveraging the power of modern web technologies. The project involves setting up an Ubuntu server on AWS, configuring Nginx as the web server, using MySQL for database management, and employing PHP for server-side scripting.

### Understanding the LEMP Stack
- **Linux**: The operating system that serves as the foundation for the stack.
- **Nginx**: The web server that handles incoming requests and serves web pages to users.
- **MySQL**: The database management system that stores and organizes data.
- **PHP**: The server-side scripting language that acts as the messenger, fetching content from the database for users and delivering user requests to where they’re needed.

---
## Project Objectives
1. Set up and configure the LEMP stack on an AWS Ubuntu server.
2. Develop a custom Nginx server block to serve a web application.
3. Implement a MySQL database for storing and managing task data.
4. Build a PHP script to retrieve and display task data dynamically.

---
## Project Steps
### Step 1: Spin up Ubuntu Server and SSH into the Server
1. Go to your AWS Console and search for EC2 instance.
2. Click **Launch Instance** → Give Instance a name → Select **Ubuntu** as the OS image → Select **t2.micro** as the instance type → Create a new key pair → Click **Launch Instance**.
3. Open Git Bash and navigate to the directory where you saved your key pair:
   ```sh
   cd Downloads
   ```
4. Change the permission mode of your key pair to enhance security:
   ```sh
   chmod 400 Devops.pem
   ```
5. SSH into your instance:
   ```sh
   ssh -i Devops.pem ubuntu@your_instance_public_ip
   ```

### Step 2: Install Nginx
1. Update your package lists:
   ```sh
   sudo apt update
   ```
2. Install Nginx:
   ```sh
   sudo apt install nginx
   ```
3. Start and check Nginx status:
   ```sh
   sudo systemctl start nginx
   sudo systemctl status nginx
   ```
4. Confirm Nginx is running by visiting `http://your_public_ip`.

### Step 3: Install MySQL
1. Install MySQL Server:
   ```sh
   sudo apt install mysql-server
   ```
2. Secure MySQL installation:
   ```sh
   sudo mysql_secure_installation
   ```
3. Verify MySQL installation:
   ```sh
   sudo mysql
   ```

### Step 4: Install PHP and its Dependencies
1. Install PHP and required modules:
   ```sh
   sudo apt install php-fpm php-mysql
   ```
2. Check PHP version:
   ```sh
   php -v
   ```

### Step 5: Configure Nginx to Use PHP Processor
1. Create a root web directory:
   ```sh
   sudo mkdir /var/www/projectLEMP
   ```
2. Assign ownership of the directory:
   ```sh
   sudo chown -R $USER:$USER /var/www/projectLEMP
   ```
3. Open a new configuration file:
   ```sh
   sudo vi /etc/nginx/sites-available/projectLEMP
   ```
4. Paste and save the following configuration:
   ```nginx
   server {
       listen 80;
       server_name projectLEMP www.projectLEMP;
       root /var/www/projectLEMP;
       index index.html index.htm index.php;
       location / {
           try_files $uri $uri/ =404;
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
5. Enable configuration:
   ```sh
   sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
   ```
6. Test Nginx configuration:
   ```sh
   sudo nginx -t
   ```
7. Reload Nginx:
   ```sh
   sudo systemctl reload nginx
   ```

### Step 6: Create and Serve an HTML File
1. Navigate to the Nginx web directory:
   ```sh
   cd /var/www/projectLEMP
   ```
2. Create an `index.html` file:
   ```sh
   touch index.html
   ```
3. Edit the file with content:
   ```sh
   sudo vi index.html
   ```
4. Adjust permissions:
   ```sh
   sudo chown -R www-data:www-data /var/www/projectLEMP
   sudo chmod -R 755 /var/www/projectLEMP
   ```
5. Restart Nginx:
   ```sh
   sudo systemctl restart nginx
   ```

### Step 7: Test PHP with Nginx
1. Create a PHP test file:
   ```sh
   sudo vi /var/www/projectLEMP/info.php
   ```
2. Paste the following content and save:
   ```php
   <?php
   phpinfo();
   ?>
   ```
3. Access the test file via `http://your_public_ip/info.php`.
4. Remove the test file after verification:
   ```sh
   sudo rm /var/www/projectLEMP/info.php
   ```

### Step 8: Retrieve Data from MySQL Database with PHP
1. Create a MySQL database:
   ```sh
   sudo mysql
   CREATE DATABASE osita_database;
   ```
2. Create a new MySQL user and grant privileges:
   ```sh
   CREATE USER 'osita_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Osita@1989';
   GRANT ALL ON osita_database.* TO 'osita_user'@'%';
   ```
3. Create a table:
   ```sh
   CREATE TABLE osita_database.todo_list (item_id INT AUTO_INCREMENT, content VARCHAR(255), PRIMARY KEY(item_id));
   ```
4. Insert sample data:
   ```sh
   INSERT INTO osita_database.todo_list (content) VALUES ("My first important item");
   ```
5. Retrieve data:
   ```sh
   SELECT * FROM osita_database.todo_list;
   ```

### Step 9: Create a PHP Script to Display Data
1. Create a PHP file:
   ```sh
   sudo vi /var/www/projectLEMP/todo_list.php
   ```
2. Paste the following code:
   ```php
   <?php
   $user = "osita_user";
   $password = "Osita@1989";
   $database = "osita_database";
   $table = "todo_list";
   try {
       $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
       foreach($db->query("SELECT content FROM $table") as $row) {
           echo "<li>" . $row['content'] . "</li>";
       }
   } catch (PDOException $e) {
       print "Error!: " . $e->getMessage();
       die();
   }
   ?>
   ```

### Conclusion
This project successfully implemented a LEMP stack to build a dynamic web application on AWS.

