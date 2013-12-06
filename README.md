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
3.  Add application/config/database.php file
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
    2.  Add application/config/config.php
    
        ```php
        $config['sess_use_database']	= TRUE;				
        $config['sess_table_name']		= 'ci_sessions';	
        // or whatever you named your created table
		
        // If using encryption (recommended)
        $config['sess_encrypt_cookie']	= TRUE;
        $config['encryption_key'] = '';
        // 32 upper & lower case plus numbers
        ```
