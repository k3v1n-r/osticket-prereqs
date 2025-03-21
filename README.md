# Installing osTicket on Windows 10

Prerequisites and installation guide for Windows 10 systems.

## Operating Systems 
- Windows 10 OS
  
## Prerequisites'
- Internet Information Services (IIS)
- [PHP Manager for IIS 1.5.0](https://github.com/RonaldCarter/PHPManager/releases)
- [PHP 7.3.8](https://drive.google.com/file/d/1-2YCXqRO4gnxIb3RFembWDunQp2M2ckT/view?usp=drive_link)
- [Visual Studio Redistributable x86 2015-2022](https://drive.google.com/file/d/1s1OsGF3-ioO0_9LYizPRiVuIkb3lFJgH/view)
- [MySQL 5.5.62](https://drive.google.com/file/d/1_OWh9p7VQLcrB0q_V7qT8yHl0xo5gv7z/view?usp=share_link)
- [IIS Rewrite Module](https://drive.google.com/file/d/1-5dBgKmMcfmbU2wdlcAkjrzfUIrbUW8y/view)
- [HeidiSQL 12.3.0.6589](https://www.google.com/url?q=https://www.heidisql.com/installers/HeidiSQL_12.3.0.6589_Setup.exe&sa=D&source=docs&ust=1742585174925249&usg=AOvVaw04gbDP2ul5D0JTS6D-3SVn)
- [osTicket 1.15.8](https://drive.google.com/file/d/1-FF0ZwqQXqzOhMXlgWQ7CSBRWEMNwr7i/view?usp=drive_link)

## Installation Steps
1. **Enable IIS on Windows 10 with CGI**
   - Go to Control Panel → Programs → Programs and Features → Turn Windows features on or off
   ![Guide Image]()
   - Enable **Internet Information Services**.
   - Expand **Internet Information Services → World Wide Web Services → Application Development Features**.
   - Check **CGI**, then click OK and restart your computer.

3. **Install Required Components**
   - Install **PHP Manager for IIS**.
   - Install **IIS Rewrite Module**.
   - Install **Visual C++ Redistributable**.

4. **Set Up PHP for IIS**
   - Create a directory `C:\PHP`.
   - Extract `php-7.3.8-nts-Win32-VC15-x86.zip` to `C:\PHP`.
   - Open **IIS Manager** → Select your server → Open **PHP Manager**.
   - Register PHP by setting the path to `C:\PHP\php-cgi.exe`.
   - Restart IIS (`iisreset` in PowerShell or stop/start IIS manually).

5. **Install MySQL and Configure Database**
   - Install **MySQL**.
   - Choose **Typical Setup** and launch the **Configuration Wizard** after installation.
   - Select **Standard Configuration** and set an username and password.
   - Install **HeidiSQL**, open it, and create a new session with MySQL username and password.
   - Create a database called `osticket`.

6. **Install osTicket**
   - Extract **osTicket `.zip` folder**.
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
- File permissions are correctly assigned to `ost-config.php`.

