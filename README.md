Following are the tasks that are performed in the provided sequence, to build this server.


Company Use Cases:

Case 1: Create a Singal tier Architect enviroment for CMS like wordpresss, Magento App on AWS Insatnce

Server Configure Details:

AWS Instance Type: t2.micro

AMI or Server OS: CentOS 7 (x86_64) - with Updates HVM

Disk Layout: 

	-- Disk1: 8GB for / ( for the root volume )

	-- Attach 2 more encrypted Volume with disk accidental termination policy at least 4GB Size of volume when you lauched your instance 
	
	-- Make these volume LVM type for future app data size grow requirement 
	
	-- Format with xfs file system 
	
	-- Disk2: 4GB for Home ( For web data, mount this on /home with "no execution binary" security flag)
	
	-- Disk3: 4Gb for MySQL ( mount this drive on /var/lib/mysql "no execution binary" before installation of mariadb server )
	
	-- Note that, both volume will be mounted automaticlly whenever you reboot your server

-- Attach one Elastic IP to your instance 

-- Disable Selinux 

-- Set timezone of Server to IST

-- Congiure your motd file on server so i can get more infomatin dynamically about server whenever we logged in and you can find out format of motd through  https://adhocnw.org/motd.txt

-- Update all your server packages before start below work and reboot your server 

Now start server setup, configuration, test and deploy 

1. Packages:

	-- > Install Apache 2.4.x server with SSL and proxy module support
	
	-- > Install PHP v7.x with essential php modules like php-mysql, php-devel, php-mbstring etc which is required by Application. In our case we need php module Wordpress and Magento 

	**** By Default Centos 7.x provide PHP 5.4.x upport so use Software Collection’s repository for official php 7.x support in CentOS 7
	
	-- > Install Mariadb 10.3 stable Server [ You have to setup offical repo's from mariadb offical website and use gpgcheck=1 ]
		-- Secure your Mariadb Server
		-- Set strong root password
		-- Bind Mariadb Server with loopback IP or localhost or lo or 127.0.0.1
		-- Increase the default value of max_allowed_packet to 1 GB so that mysqldump command will take backup of till 1GB size table otherwsie it will through error in mysqldump

Note: Make your services automatically restart whenever server reboot in future [ Try to reboot server to check services ]


2. User Setup:

	-- > Create 2 users as wp and magento with passwordless login for both user
	-- > Create public_html folder under each user home directory 
	-- > Use this public_html folder for wp ( /home/wp/public_html ) and magento ( /home/magento/public_html ) web files and that will be your document root in Apache 

3. MySQL Requirement:
	-- > Create two databases as wp and magento
	-- > Cretae two mysql user and grant privildges to respective database 

4. VirtualHost Configuration:

	-- > Create two virtualhost configuration under /etc/httpd/conf.d as wp.conf and magento.conf for better management rather than /etc/httpd/httpd.conf as follows:

		-- ServerName will be yourname-wp.adhocnw.com and yourname-mag.adhocnw.com in your virtual hosting

		-- Also setup seprate error_log file for both subdomains

		-- As of now, Adhoc Networks own adhocnw.com domain so you can't point your instance IP to these subdomain but you can test your application to write dns inforamtion in /etc/hosts file until we will not updated your dns entry to dns server. [ mail to linux@linux.com with your both subdomain name and Instance Elastic IP address ]
							
							OR

		--  You can buy a free domain on https://www.namecheap.com/ using your student GIT Pack and create 2 subdomains yourname-wp.yourdomain.me and yourname-mag.yourdomain.me. After that point A record to your instance IP

5. Now you server is ready to setup wordpress and magento . You can find out more info on Google

6. Test your application using /etc/hosts until your subdomain not pointed to your Instance IP

4. Update your server Packages to fix packages vunlerabilty and reboot the server and recheck all things 

5. Now automate all above tasks using ansible playbook or bash script ( Skip creation of instance in ansible playbook )

7. Most Important:

Create a document for your process and submit to linux@linux.com and ashutoshh@linux.com

Docs used for Reference:

1. https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/system_administrators_guide/index#s1-sysinfo-filesystems ( Check 14.1.6. Setting Up Virtual Hosts for virtualhosting, 14.1.7. section ssl Setup )
2. Wordpress Web files: https://wordpress.org/download/
3. Magento OpenSource: https://magento.com/tech-resources/download
4. Mariadb Repositories: https://downloads.mariadb.org/mariadb/repositories/#mirror=yamagata-university
