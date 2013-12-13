Using PDO in Codeigniter 2.x
============================
Add PDO to Codeigniter using all of the regular PHP PDO functionality => TRUE PDO  
Based off of Codeigniter 2.1-stable version  
The files contained in the develop branch are based of Codeigniter 2.1-stable branch, in effect making this a 3rd party standalone.  


Files different from Ellis Labs Codeigniter
*******************************************
*  README.txt
*  PDOschema.sql
*  /application/config/config.php
*  /application/config/database.php
*  /application/libraries/Session.php
*  /system/database/DB.php
  

Complete Step By Step Installation  
----------------------------------
For a complete step by step installation tutorial, visit  
http://christopherickes.com/web-app-development/pdo-for-codeigniter-2/  


Brief Synopsis  
**************
1.  Replace your current system/database/DB.php file
2.  Add application/libraries/Session.php file
3.  Edit application/config/database.php file
    1.  Set the following values
  
        ```php    
            $active_record = FALSE;    
            $PDO_conn = TRUE; 
        ```
    2. Fill the remaining values based on your unique configuration.
4.  If storing sessions in the database (recommended)
    1.  Create a session table as instructed by Codeigniter
    
        ```SQL
            CREATE TABLE IF NOT EXISTS  `ci_sessions` (  
              session_id varchar(40) DEFAULT '0' NOT NULL,  
              ip_address varchar(45) DEFAULT '0' NOT NULL,  
              user_agent varchar(120) NOT NULL,  
              last_activity int(10) unsigned DEFAULT 0 NOT NULL,  
              user_data text NOT NULL,  
              PRIMARY KEY (session_id),  
              KEY `last_activity_idx` (`last_activity`)  
            );
        ```
    2.  Edit application/config/config.php
    
        ```php
        $config['sess_use_database']	= TRUE;				
        $config['sess_table_name']      = 'ci_sessions';
        $config['sess_cookie_Name']     = 'somethingwithoutunderscores'
        // or whatever you named your created table
		
        // If using encryption (recommended)
        $config['sess_encrypt_cookie']	= TRUE;
        $config['encryption_key'] = '';
        // 32 upper & lower case plus numbers
        ```

Additional Steps for Installation and Security
----------------------------------------------
1.  Rename the application directory to *new-app-dir*  

    ```linux
    mv application *new-app-dir*
    ```
2.  Create a public_html directory  

    ```linux
    mkdir public_html
    ```
3.  Move index.php to the public_html directory  

    ```linux
    mv index.php public_html/
    ```
4.  Change the server root directory to the public_html directory  
This step is web server specific.  Use Google, if needed.  
HINT for Apache:  Edit Virtual Host configuration.
5.  Update index.php  

    ```php  
    define('ENVIRONMENT', 'production');  
    $system_path = '/full/path/to/system_dir/system'  
    $application_path = '/full/path/to/app_dir/*new-app-dir*'  
   ```
6.  Update *new-app-dir*/config/config.php  
    ```php
    $config['base_url']        = 'www.myappdomain.com';  
    # removing index.php requires rewrite rules   
    $config['index_page']      = '';  
    $config['global_xss_filtering'] = TRUE;  
    $config['csrf_token_name'] = 'sometokenname';  
    $config['csrf_cookie_name'] = 'somecookiename';  
    ```
7.  Update rewrite rules  
ADVANCED: If possible, disable .htaccess and make updates in Virtual Host configurations  
    ```linux
    RewriteEngine On  
    RewriteBase /  
    RewriteCond %{REQUEST_URI} ^system.*  
    RewriteRule ^(.*)$ /index.php?/$1 [L]  
    RewriteCond %{REQUEST_URI} ^new-app-dir.*  
    RewriteRule ^(.*)$ /index.php?/$1 [L]  
    RewriteCond %{THE_REQUEST} ^GET.*index\.php [NC]  
    RewriteCond %{THE_REQUEST} !/system/.*  
    RewriteRule (.*?)index\.php/*(.*) /$1$2 [R=301,L]  
    RewriteCond %{REQUEST_FILENAME} !-f  
    RewriteCond %{REQUEST_FILENAME} !-d  
    RewriteRule ^(.*)$ index.php?/$1 [L]  
    ```
8.  Update *new-app-dir*/config/database.php  
    ```php
    $db['default']['hostname'] = 'localhost';  
    $db['default']['username'] = '';  
    $db['default']['password'] = '';  
    $db['default']['database'] = '';  
   ```
9.  Update *new-app-dir*/config/autoload.php appropriately.
