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
![Image](https://github.com/user-attachments/assets/c7117693-e161-4f38-8b04-319fae88128e)
4. Open Git Bash and navigate to the directory where you saved your key pair:
   ```sh
   cd Downloads
   ```
![Image](https://github.com/user-attachments/assets/38bd14b6-a0cd-4d3c-920e-a8ffaf3443b5)
5. Change the permission mode of your key pair to enhance security:
   ```sh
   chmod 400 Devops.pem
   ```
![Image](https://github.com/user-attachments/assets/a04c983a-a364-4102-9179-7a752661b8e6)
6. SSH into your instance:
   ```sh
   ssh -i Devops.pem ubuntu@your_instance_public_ip
   ```
![Image](https://github.com/user-attachments/assets/65a7b76c-ff72-4b23-ba81-6ceac4181f38)
### Step 2: Install Nginx
1. Update your package lists:
   ```sh
   sudo apt update
   ```
2. Install Nginx:
   ```sh
   sudo apt install nginx
   ```
![Image](https://github.com/user-attachments/assets/0abbee89-7fe6-4b20-9c18-a0303481ce33)
3. Start and check Nginx status:
   ```sh
   sudo systemctl start nginx
   sudo systemctl status nginx
   ```
![Image](https://github.com/user-attachments/assets/ad1de532-2063-4c07-a7ee-3ee7999ba045)
4. Confirm Nginx is running by visiting `http://your_public_ip`.
![Image](https://github.com/user-attachments/assets/45da7e83-4e8f-4065-bbe9-067eeb544636)

### Step 3: Install MySQL
1. Install MySQL Server:
   ```sh
   sudo apt install mysql-server
   ```
![Image](https://github.com/user-attachments/assets/13173c03-7eeb-4b43-a0bf-661da2883539)
2. Secure MySQL installation:
   ```sh
   sudo mysql_secure_installation
   ```
3. Verify MySQL installation:
   ```sh
   sudo mysql
   ```
![Image](https://github.com/user-attachments/assets/4df5118c-68d2-4b55-b3b7-35c6f781caea)

### Step 4: Install PHP and its Dependencies
1. Install PHP and required modules:
   ```sh
   sudo apt install php-fpm php-mysql
   ```
![Image](https://github.com/user-attachments/assets/40414eb3-a7d9-456a-aaaf-eff2720d21a8)
2. Check PHP version:
   ```sh
   php -v
   ```
![Image](https://github.com/user-attachments/assets/139afc4b-3ce6-4e2f-ab75-80b4198f7b73)

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
![Image](https://github.com/user-attachments/assets/4bb7aba9-69ac-45ac-85e3-6b029b6a999b)
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
![Image](https://github.com/user-attachments/assets/71b7ff3c-493a-4407-886f-ca1ed1337925)
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
![Image](https://github.com/user-attachments/assets/f2aaf157-0533-4804-b060-df9fe36616e4)

3. Access the test file via `http://your_public_ip/info.php`.
   ![Image](https://github.com/user-attachments/assets/8d341371-3542-4961-adca-d209243307b7)
5. Remove the test file after verification:
   ```sh
   sudo rm /var/www/projectLEMP/info.php
   ```

### Step 8: Retrieve Data from MySQL Database with PHP
1. Create a MySQL database:
   ```sh
   sudo mysql
![Image](https://github.com/user-attachments/assets/b0f5f27f-1e1d-4bde-9145-d56305253557)
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
![Image](https://github.com/user-attachments/assets/4123b9d9-ac97-44b9-b345-af56ba37bf68)

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
![Image](https://github.com/user-attachments/assets/5aec8e1f-24bc-4483-a988-a5ebf09d37fb)

![Image](https://github.com/user-attachments/assets/9be319b2-a659-4f49-941a-0fc61a580f63)

### Conclusion
This project successfully implemented a LEMP stack to build a dynamic web application on AWS.

