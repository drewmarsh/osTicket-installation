# Environments and Technologies Used
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)
- Windows 10 Professional (22H2)

# List of Prerequisites
- Microsoft Azure Virtual Machine
- osTicket v1.15.8
- MySQL v5.5.62 (win32)
- Heidi SQL build 12.3.0.6589
- php-7.3.8-nts-Win32-VC15-x86
- PHP Manager for IIS v1.5.0
- IIS URL Rewrite Module 2
- Microsoft Visual C++ 2015-2022 Redistributable (x86) - 14.34.31931

# 📥 Installation

### 🖥️ [Create and Access Azure Virtual Machine](https://github.com/drewmarsh/azure-creating-VM)

### 🌐 Enable Internet Information Services (IIS)

1. Navigate to **Control Panel** > `Uninstall a program` > `Turn Windows features on or off`
2. Tick &#9989;`Internet Information Services` > `OK` > After it finishes, click `Close`
3. Expand **Internet Information Services** > **World Wide Web Services** > **Application Development Features**
4. Tick &#9989;`CGI`

### 🗄️ Install Required Software

1. Install MySQL Community Server
   - Choose `Typical setup`
   - Select `Developer machine`
   - Choose `Multifunctional database`
   - Enter root password
   - Click `Execute`

2. Install Visual C++ Redistributable
3. Install PHP Manager for IIS
4. Install URL Rewrite Module for IIS

### 🛠️ Add MySQL to PATH

If mysql.exe exists, add its directory to your system's PATH:

1. Right-click on `This PC` or `My Computer` and select `Properties`
2. Click on `Advanced system settings`
3. Click `Environment Variables`
4. Under **System variables**, find and select `Path`, then click `Edit`
5. Click `New` and add the full path to your MySQL bin directory (e.g., `C:\Program Files (x86)\MySQL\MySQL Server 5.5\bin`)
6. Click `OK` on all windows to save changes

### 🐘 Install and Configure PHP

1. Download PHP binaries for Windows
2. Extract files to `C:\PHP`
3. Rename `php-7.3.8-nts-Win32-VC15-x86` to `PHP`
4. Open `php.ini` and make the following changes:
   - Uncomment `;extension_dir = "ext"` by removing the semicolon
   - Add the following lines to the bottom of the file:
    
      ```
      ;Enable extensions
      extension=C:\PHP\ext\php_mysqli.dll
      extension=C:\PHP\ext\php_gd2.dll
      extension=C:\PHP\ext\php_imap.dll
      extension=C:\PHP\ext\php_curl.dll
      extension=C:\PHP\ext\php_pdo_mysql.dll
      extension=C:\PHP\ext\php_mbstring.dll
      extension=C:\PHP\ext\php_intl.dll
      zend_extension=C:\PHP\ext\php_opcache.dll
      
      ;FastCGI settings
      fastcgi.impersonate = 1
      cgi.fix_pathinfo = 0
      cgi.force_redirect = 0
      
      ;Security setting 
      open_basedir = "C:\inetpub\wwwroot;C:\Windows\Temp;C:\PHP\temp"
      ```

### 🔧 Configure IIS for PHP

1. Open **Internet Information Services (IIS) Manager**
2. Select your server in the left pane
3. Open `Handler Mappings`
4. Click `Add Module Mapping` in the right pane
5. Fill in the following:
   - Request path: `*.php`
   - Module: `FastCgiModule`
   - Executable: `C:\PHP\php-cgi.exe`
   - Name: `PHP_via_FastCGI`
6. Click `OK` and accept the prompt to create a FastCGI application

### ⚙️ Set Up FastCGI Settings

1. In **Internet Information Services (IIS) Manager**, select the server
2. Open `FastCGI Settings`
3. Find the PHP entry and double-click it
4. In `Environment Variables`, add:
   - `PHP_FCGI_MAX_REQUESTS` : `10000`
   - `PHPRC` : `C:\PHP`

### 📄 Configure Default Document

1. In **Internet Information Services (IIS) Manager**, select the server
2. Open `Default Document`
3. Add `index.php` to the list

### 🔄 Restart IIS

1. In **Internet Information Services (IIS) Manager**, select the server
2. Click `Restart` in the right pane

### 🧪 Test PHP

1. Create a file named `phpinfo.php` in your web root with this content:

   ```php
   <?php phpinfo(); ?>
   ```
3. Access it through your browser: `http://localhost/phpinfo.php`
   
### 🎫 Install osTicket

1. Extract the contents of osTicket-v1.15.8 ("scripts" and "upload" folder)
2. Place the "upload" folder in `C:\inetpub\wwwroot`
3. Rename the "upload" folder to `osTicket`
4. Then, in **Internet Information Services (IIS) Manager**, select the server and click `Restart` in the right pane
5. In the left panel, expand **Sites** > **Default Web Site**, and then click "osTicket". Next, click `Browse *:80 (http)` in the right panel

### 🔐 Configure osTicket

1. Navigate to `C:\inetpub\wwwroot\osTicket\include`
2. Rename `ost-sampleconfig.php` to `ost-config.php`
3. Set permissions for `ost-config.php`:
   - Right-click  `ost-config.php` > `Properties` > `Security` tab > `Advanced`
   - `Disable inheritance` > `Remove all inherited permissions from this object`
   - Click `Add` > `Select a principal` >  In the text field, enter "everyone"
   - Tick &#9989;`Full control` > `OK` > `Apply` > `OK` > `OK`

### 🗃️ Set Up Database

1. Install HeidiSQL
2. Open HeidiSQL and create a new session using the MySQL root password
3. Create a new database named `osTicket`

### 🏁 Complete osTicket Installation

1. Navigate to `localhost/osTicket/setup` and click `Continue >>`
2. Fill out the form (example):
   - Helpdesk Name: `osTicket`
   - Default Email: `osTicket.support@gmail.com`
   - First Name: `Drew`
   - Last Name: `Marsh`
   - Email address: `osTicket.drewmarsh@gmail.com`
   - Username: `drewmarsh`
   - Password: Enter secure password
     
3. Fill out the database settings (example):
   - MySQL Table Prefix: `ost_`
   - MySQL Hostname: `localhost`
   - MySQL Database: `osTicket`
   - MySQL Username: `root`
   - MySQL Password: Enter secure password
4. Click `Install Now`
6. Once you see the "Congratulations!" screen, click the `Admin Panel` hyperlink
