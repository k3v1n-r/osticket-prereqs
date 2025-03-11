# osTicket Installation Guide on Microsoft Azure

## Prerequisites
Before installing osTicket on Microsoft Azure, ensure that you have the following:

- **Azure Virtual Machine (VM):** Running Ubuntu 22.04 (Recommended) or Windows Server (with IIS and PHP configured)
- **Web Server:** Apache for Linux or IIS for Windows Server
- **PHP:** Version 7.2 - 8.0 (Recommended: PHP 7.4)
- **Database:** Azure Database for MySQL or MariaDB
- **Required PHP Extensions:**
  - `mysqli`
  - `gd`
  - `gettext`
  - `imap`
  - `json`
  - `mbstring`
  - `xml`
  - `intl`
  - `apcu` (recommended for caching)

## Installation Steps

### Step 1: Set Up an Azure Virtual Machine
1. Log in to [Azure Portal](https://portal.azure.com/).
2. Create a new Virtual Machine (Ubuntu 22.04 or Windows Server).
3. Choose an appropriate VM size (Standard B2s or higher recommended).
4. Configure inbound security rules to allow HTTP (80) and HTTPS (443).

### Step 2: Install Required Software
For Ubuntu:
```sh
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 php php-mysqli php-gd php-gettext php-imap php-json php-mbstring php-xml php-intl php-apcu unzip -y
```

For Windows Server, install IIS and PHP using the Web Platform Installer.

### Step 3: Download osTicket
Download the latest stable release of osTicket from the official website:
[osTicket Downloads](https://osticket.com/download/)

Extract and move it to the web root:
```sh
wget https://github.com/osTicket/osTicket/releases/latest/download/osTicket-v1.17.zip
unzip osTicket-v1.17.zip -d /var/www/html/osticket
```

### Step 4: Set File Permissions
Ensure the following directories are writable:
```sh
sudo chmod -R 755 /var/www/html/osticket
sudo chmod 664 /var/www/html/osticket/include/ost-config.php
sudo chown www-data:www-data /var/www/html/osticket/include/ost-config.php
```

### Step 5: Create an Azure Database for MySQL
1. In the Azure portal, create an **Azure Database for MySQL**.
2. Configure firewall rules to allow connections from your VM.
3. Run the following commands to create the database:
```sh
mysql -h your-mysql-server.mysql.database.azure.com -u osticket_user -p
CREATE DATABASE osticket;
GRANT ALL PRIVILEGES ON osticket.* TO 'osticket_user'@'your-vm-ip' IDENTIFIED BY 'secure_password';
FLUSH PRIVILEGES;
EXIT;
```

### Step 6: Configure osTicket
1. Open your browser and go to:
   ```
   http://your-vm-ip/osticket/setup/
   ```
2. Follow the on-screen instructions to complete installation.

### Step 7: Finalize Installation
1. Remove the setup directory:
```sh
sudo rm -rf /var/www/html/osticket/setup/
```
2. Revert permissions for `ost-config.php`:
```sh
sudo chmod 644 /var/www/html/osticket/include/ost-config.php
sudo chown www-data:www-data /var/www/html/osticket/include/ost-config.php
```
3. Restart Apache:
```sh
sudo systemctl restart apache2
```

### Step 8: Access osTicket Admin Panel
- Navigate to:
  ```
  http://your-vm-ip/osticket/scp/
  ```
- Default Admin Credentials (Change after first login!):
  - **Username:** `admin`
  - **Password:** `admin`

## Additional Configuration
- Set up email piping for ticket creation.
- Configure Azure cron jobs for automated tasks. Example:
  ```sh
  echo "* * * * * www-data php /var/www/html/osticket/api/cron.php" | sudo tee -a /etc/crontab > /dev/null
  ```
- Install language packs if needed.

For more details, refer to the [osTicket Documentation](https://docs.osticket.com/).
