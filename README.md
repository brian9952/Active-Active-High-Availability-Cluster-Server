# Active-Active-High-Availability-Cluster-Server

Active Active High Availability CLuster Server applying Load Balancer and High Availability to Server

<h3>Step:<br><br>
1. Provide 4 mini-pc Raspberry Pi<br>
2. Configure Raspi 1 and Raspi 2 as The Master Node.<br>
3. Configure Raspi 3 and Raspi 4 as The Slave Node.<br>
4. In Slave Node Install Apache2 </h3>
<br>We used Linux Ubuntu as the OS of the Raspberry Pi,
So the command line is: 

```
sudo apt install apache2
```
 
<h3>5. In Slave Node do config at apache2.conf</h3>
After you install the apache2 software, do
<br>

```
sudo vim /etc/apache2/apache2.conf
```

<br>
after open apache2.conf, insert this statement:
<br>

```
#LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined 
LogFormat "%{X-Forwarded-For}i %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
```

<h3>6. In Slave Node do config at default </h3>
You can find default.conf at 

```
vi /etc/apache2/sites-available/default
```

after you open the default.conf, insert this statement:

```
SetEnvIf Request_URI "^/check\.txt$" dontlog
CustomLog /var/log/apache2/access.log combined env=!dontlog
```

<h3>7. Restart the configuration </h3>

```
sudo systemctl restart apache2
#check_status
sudo systemctl status apache2
```
<h3>8. create the file check.txt </h3>

```
touch /var/www/check.txt
```

<h3>9. Then in Master Node, at Raspi node 1 and Raspi node 2 install haproxy</h3>

```
sudo apt install haproxy
```

<h3>10. edit haproxy.cfg at </h3>

```
vim /etc/haproxy/haproxy.cfg
```

insert this code

```

