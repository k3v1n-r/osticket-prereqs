# Installing osTicket on Windows 10

Prerequisites and installation guide for Windows 10 systems.

## Prerequisites
- Windows 10 OS
- Internet Information Services (IIS)
- [PHP Manager for IIS](https://www.iis.net/downloads/community/2018/05/php-manager-150-for-iis-10)
- [PHP](https://windows.php.net/download/)
- [Visual Studio Redistributable x86](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)
- [MySQL](https://dev.mysql.com/downloads/installer/)
- [IIS Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)
- [HeidiSQL](https://www.heidisql.com/download.php)
- [osTicket](https://osticket.com/download/)

## Installation Steps
1. **Enable IIS on Windows 10 with CGI**
   - Go to Control Panel → Programs → Programs and Features → Turn Windows features on or off
   - Enable **Internet Information Services**.
   - Expand **Internet Information Services → World Wide Web Services → Application Development Features**.
   - Check **CGI**, then click OK and restart your computer.

3. **Install Required Components**
   - Install **PHP Manager for IIS** (`PHPManagerForIIS_V1.5.0.msi`).
   - Install **IIS Rewrite Module** (`rewrite_amd64_en-US.msi`).
   - Install **Visual C++ Redistributable** (`VC_redist.x86.exe`).

4. **Set Up PHP for IIS**
   - Create a directory `C:\PHP`.
   - Download and extract **PHP NTS** to `C:\PHP`.
   - Open IIS Manager → Select your server → Open **PHP Manager**.
   - Register PHP by setting the path to `C:\PHP\php-cgi.exe`.
   - Restart IIS (`iisreset` in PowerShell or stop/start IIS manually).

5. **Install MySQL and Configure Database**
   - Install **MySQL**.
   - Choose **Typical Setup** and launch the **Configuration Wizard** after installation. change this
   - Select **Standard Configuration** and set an username and password.
   - Install **HeidiSQL**, open it, and create a new session with MySQL username and password.
   - Create a database called `osticket`.

6. **Install osTicket**
   - Extract **osTicket .zip folder**.
   - Copy the `upload` folder to `C:\inetpub\wwwroot` and rename it to `osTicket`.
   - Open IIS Manager → Sites → Default Web Site → osTicket.
   - Click **Browse *:80** to open the osTicket checklist in your browser.

7. **Enable Required PHP Extensions**
   - Open IIS Manager → Sites → Default Web Site → osTicket.
   - Open **PHP Manager** → Click **Enable or disable an extension**.
   - Enable:
     - `php_imap.dll`
     - `php_intl.dll`
     - `php_opcache.dll`
   - Refresh the osTicket site in your browser to confirm extensions have been enabled.

8. **Configure osTicket Files**
   - Rename `C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php` to `ost-config.php`
   - Right-click the file → **Properties** → **Security** → **Advanced**.
   - Disable inheritance, remove all permissions, and add **Everyone** with full control.
   - *Only allow permissions to authorized personnel*

9. **Complete osTicket Setup in the Browser**
   - Click **Continue** and set up the helpdesk name and default email.
   - Provide the database details:
     - MySQL Database: `osticket`
     - MySQL Username: `default`
     - MySQL Password: `default`
   - Click **Install Now!**

## Post-Installation
Once installed, you can access the osTicket admin panel at:

[http://localhost/osticket/scp/](http://localhost/osticket/scp/)

## Troubleshooting
If you encounter issues, ensure:
- IIS and MySQL are running.
- PHP is correctly mapped in IIS.
- Required PHP extensions are enabled.
- The `setup` directory is deleted after installation.
- File permissions are correctly assigned to `ost-config.php`.

