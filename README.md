# Install-Apache-Tomcat-Server-on-VPS-Ubuntu-20.04

Instructions on configuring Ubuntu 20.04 VPS server to run java web.
![](https://user-images.githubusercontent.com/61933071/116807627-5f5ae500-ab5e-11eb-856d-6be41ed44ae7.png)
<p>Step 0: Update APT</p>

```markdown
First, as always, update your APT.
$ sudo apt update
```
<p>Step 1: Install java</p>
<p>Step 2: Install Tomcat</p>

```
- Check for Tomcat in Repository
$ sudo apt-cache search tomcat
- Download and install Tomcat
$ sudo apt install tomcat9 tomcat9-admin
- When the download is finished, it will install the Apache Tomcat Server, which will start up automatically. For verification, type the following ss command, which will show you the 8080 open port number, the default open port reserved for Apache Tomcat Server.
$ ss -ltn
- Change Tomcat Settings
$ sudo systemctl enable tomcat9
- Create User
$ sudo nano /etc/tomcat9/tomcat-users.xml
- Add Tagged Lines
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="tomcat" password="my-password" roles="standard,admin-gui,manager-gui"/>
- Restart Tomcat
$ sudo systemctl restart tomcat9
//$ sudo systemctl start tomcat9
```

<p>Step 3: Install proxy</p>

```
$ sudo apt-get install nginx
$ sudo systemctl stop nginx.service
$ sudo systemctl start nginx.service
$ sudo systemctl enable nginx.service
$ sudo nano /etc/nginx/sites-available/example


server {
	listen 80;
	listen [::]:80;
	server_name  public ip or domain name;



	proxy_redirect           off;
	proxy_set_header         X-Real-IP $remote_addr;
	proxy_set_header         X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_set_header         Host $http_host;

	location / {
			proxy_pass http://127.0.0.1:8080;
		}
}
$ sudo ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/
$ sudo systemctl restart nginx.service
```
    
<p>Step 4: Install mysql</p>

```
$ sudo apt install mysql-server
$ sudo mysql_secure_installation
$ sudo systemctl status mysql
$ sudo systemctl start mysql
$ sudo systemctl stop mysql
$ sudo mysql -u root -h localhost -p
//Then set password for user root in mysql
```
<p>Fix error: 413 Request Entity Too Large on Nginx</p>

```markdown
$ sudo nano /etc/nginx/nginx.conf

http {
    ...
    # Set value 'client_max_body_size'
    client_max_body_size 100M;
    ...
}


$ sudo nginx -s reload
```
