<p align="center">
  <a href="https://github.com/drewmarsh/osTicket-installation">
    <img src="/images/osticket-banner.png" width="598" alt="Banner">
  </a>
</p>

<div align="center">

### 📍 Part 1: Prerequisites & osTicket Installation
### 👉 [Part 2: Post-Installation Configuration](https://github.com/drewmarsh/osTicket-post-install-configuration)
### 👉 [Part 3: Ticket Lifecycle Examples](https://github.com/drewmarsh/osTicket-ticket-lifecycle-demo)

</div>

# 🧠 Technologies Used
- Microsoft Azure (Cloud computing)
- Microsoft Remote Desktop
- Internet Information Services (IIS)

# 📋 List of Prerequisites
- Microsoft Azure Virtual Machine (Windows 10 Professional x64 22H2)
- osTicket v1.15.8
- MySQL v5.5.62 (win32)
- Heidi SQL build 12.3.0.6589
- php-7.3.8-nts-Win32-VC15-x86
- PHP Manager for IIS v1.5.0
- IIS URL Rewrite Module 2
- Microsoft Visual C++ 2015-2022 Redistributable (x86) - 14.34.31931

# 📥 Setup & Installation

### 🖥️ [Create and Access Azure Virtual Machine](https://github.com/drewmarsh/azure-creating-VM)

### 🌐 Enable Internet Information Services (IIS)

1. Navigate to **Control Panel** > `Uninstall a program` > `Turn Windows features on or off`
2. Tick &#9989;`Internet Information Services`
3. Expand **Internet Information Services** > **World Wide Web Services** > **Application Development Features**
4. Tick &#9989;`CGI` > `OK` > After it finishes, click `Close`

<img src="/images/enable-iss-and-CGI.png" alt="Enable ISS and CGI"> <br>

### 🗄️ Install Required Software

1. Install MySQL Server v5.5.62
   - Choose `Typical setup`
   - Select `Developer machine`
   - Choose `Multifunctional database`
   - Tick &#9989;`Include Bin Directory in Windows PATH`
   - Enter root password
   - Click `Execute`
   - After it concludes, click `Finish`

<img src="/images/install-mysql.png" alt="Install MySQL"> <br>

2. Install Visual C++ Redistributable

<img src="/images/install-visc.png" alt="Install Visual C++ Redistributable"> <br>

3. Install PHP Manager for IIS

<img src="/images/install-phpman.png" alt="Install PHP Manager for IIS"> <br>

4. Install URL Rewrite Module for IIS

<img src="/images/install-rewrite.png" alt="Install URL Rewrite Module for IIS"> <br>

### 🐘 Install and Configure PHP

1. Download PHP binaries for Windows
2. Extract files to `C:\PHP`
3. Rename `php-7.3.8-nts-Win32-VC15-x86` to `PHP`
4. In the PHP folder, rename `php.ini-production` to `php.ini`
5. Open `php.ini` and make the following changes:
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
      cgi.fix_pathinfo = 1
      cgi.force_redirect = 0
      
      ;Security setting 
      open_basedir = "C:\inetpub\wwwroot;C:\Windows\Temp;C:\PHP\temp"
      ```

<img src="/images/configure-phpini.png" alt="Configure PHP.ini"> <br>

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

<img src="/images/configure-iss-for-php.png" alt="Configure IIS for PHP"> <br>

### ⚙️ Set Up FastCGI Settings

1. In **Internet Information Services (IIS) Manager**, select the server
2. Open `FastCGI Settings`
3. Find the PHP entry and double-click it
4. Under **Environment Variables** click `…` and then `Add`
5. Add the following environment variables and then press `OK` > `OK`:
   - `PHP_FCGI_MAX_REQUESTS` : `10000`
   - `PHPRC` : `C:\PHP`

<img src="/images/fastcgi-settings.png" alt="FastCGI Settings"> <br>

### 📄 Configure Default Document

1. In **Internet Information Services (IIS) Manager**, select the server
2. Open `Default Document`
3. Click `Add…` in the right panel and then add `index.php` to the list

<img src="/images/configure-defaultdoc.png" alt="Configure Default Document"> <br>

### 🧪 Restart IIS & Test PHP
1. In **Internet Information Services (IIS) Manager**, select the server
2. Click `Restart` in the right pane
3. Create a file named `phpinfo.php` in your web root (`C:\inetpub\wwwroot`) with this content:

   ```php
   <?php phpinfo(); ?>
   ```
4. Access it through your browser: `http://localhost/phpinfo.php`

<img src="/images/test-php.png" alt="Test PHP"> <br>
   
### 🎫 osTicket Setup

1. Extract the contents of osTicket-v1.15.8 ("scripts" and "upload" folder)
2. Place the "upload" folder in `C:\inetpub\wwwroot`
3. Rename the "upload" folder to `osTicket`

4. Navigate to `C:\inetpub\wwwroot\osTicket\include`
5. Rename `ost-sampleconfig.php` to `ost-config.php`
6. Set permissions for `ost-config.php`:
   - Right-click  `ost-config.php` > `Properties` > `Security` tab > `Advanced`
   - `Disable inheritance` > `Remove all inherited permissions from this object`
   - Click `Add` > `Select a principal` >  In the text field, enter "everyone"
   - Tick &#9989;`Full control` > `OK` > `Apply` > `OK` > `OK`

<img src="/images/setup-osticket1.png" alt="osTicket Setup 1"> <br>

7. Then, in **Internet Information Services (IIS) Manager**, select the server and click `Restart` in the right pane
8. In the left panel, expand **Sites** > **Default Web Site**, and then click **osTicket**

> [!NOTE]
> If **osTicket** does not show up, restart the entire **Internet Information Services (IIS) Manager** application

9. Click `Browse *:80 (http)` in the right panel

<img src="/images/setup-osticket2.png" alt="osTicket Setup 2"> <br>

### 🗃️ HeidiSQL Installation & Configuration

1. Install and open HeidiSQL. Dismiss the donation message and then press `+New`
2. In the **Password:** field, enter the root password that was configured previously during the setup of MySQL Server 5.1
3. Click `Open`
4. On the left panel of HeidiSQL, right-click `Unnamed` and then navigate to `Create new` > `Database`, in the **Name:** field, enter `osTicket` and then press `OK`. Finally, close out of HeidiSQL.

<img src="/images/install-heidisql.png" alt="Install HeidiSQL"> <br>

<img src="/images/configure-heidisql.png" alt="Configure HeidiSQL"> <br>

### 🏁 Complete osTicket Installation

1. Navigate to `localhost/osTicket/setup` and click `Continue »`
2. Fill out the **System Settings** section:
   - Helpdesk Name: `osTicket`
   - Default Email: `osTicket.support@gmail.com`

3. Fill out the **Admin User** section:
   - First Name: `Drew`
   - Last Name: `Marsh`
   - Email address: `osTicket.drewmarsh@gmail.com`
   - Username: `drewmarsh`
   - Password: `securepassword`
     
4. Fill out the **Database Settings** section:
   - MySQL Table Prefix: `ost_`
   - MySQL Hostname: `localhost`
   - MySQL Database: `osTicket`
   - MySQL Username: `root`
   - MySQL Password: `securepassword`
     
5. Click `Install Now`

6. Once you see the "Congratulations!" screen, click the `Admin Panel` hyperlink

7. Finally, enter the username and password to access the osTicket dashboard.

<img src="/images/complete-osticket-installation.png" width="674" alt="Complete osTicket Installation"> <br>

<img src="/images/osticket-dashboard.png" alt="osTicket Dashboard"> <br>

<br><div align="center">

### 📍 Part 1: Prerequisites & osTicket Installation
### 👉 [Part 2: Post-Installation Configuration](https://github.com/drewmarsh/osTicket-post-install-configuration)
### 👉 [Part 3: Ticket Lifecycle Examples](https://github.com/drewmarsh/osTicket-ticket-lifecycle-demo)

</div>
