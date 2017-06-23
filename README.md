Unifi Controler on a FreeNas Jail


=======

Source: https://wiki.mattrude.com/FreeNAS/Unifi_Controller_Install


ToC

+ [Create jail](#createjail)
+ [Install Unifi](#installunifi)
+ [Setting up](#settingup)
+ [Update](#update)



Notes
-----


**Create a jail <a name="#createjail">**





**Install UniFi Controler <a name="#installunifi">**



```
jls
jexec 18 tcsh
pkg update
pkg upgrade
pkg install -y openjdk8
```

Configure openjdk8 to work within the jail. 
```
echo "fdesc   /dev/fd         fdescfs         rw      0       0" > /etc/fstab
echo "proc    /proc           procfs          rw      0       0" >> /etc/fstab
```


Install MongoDB
```
pkg install -y mongodb
echo 'mongod_enable="YES"' >> /etc/rc.conf
```


Install Unifi Controller
```
setenv BATCH yes
portsnap fetch extract
portsnap fetch update
cd /usr/ports/net-mgmt/unifi5/
make install clean
echo 'unifi_enable="YES"' >> /etc/rc.conf
service unifi start
```


**Setting up UniFi Controler <a name="#settingup">**

First go to the IP address of your Unifi host, ie: https://<IP-Address>:8443 You will be prompted to accept an Invalid SSL Certificate before accessing the new site.


**Updating UniFi Controler <a name="#update">**

Check https://svnweb.freebsd.org/ports/head/net-mgmt/unifi5/Makefile?view=co


```
portsnap fetch update && cd /usr/ports/net-mgmt/unifi5/ && make deinstall reinstall clean && \
service unifi restart
```

