# le-scripts
My set of lets encrypt scripts and templates

1. First install letsencrypt:

```
sudo -s
apt-get update
apt-get upgrade -y
apt-get install -y git bc
git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt
```

2. Put files into appropriate directories
3. Change DNS for your new site
4. Ensure that /etc/nginx/sites-enabled/default serves /usr/share/nginx/html
5. create certificate:
```
cd /opt/letsencrypt
./letsencrypt-auto certonly -a webroot -d www.mynewsite.cz -d mynewsite.cz --webroot-path=/usr/share/nginx/html
```

6. create WWW server:
```
cd /etc/nginx/sites-available
cp template mynewsite.cz
cd ../sites-enabled
```
and replace !domainhere! with mynewsite.cz

```
cd ../sites-enabled
ln -s ../sites-available/mynewsite.cz
service nginx reload
```

7. create LE renewal config:
```
cd /usr/local/etc
cp le-template le-mynewsite.cz
```

and replace !domainhere! with mynewsite.cz

8. create renewal scripts:
```
cd /usr/local/sbin
cp le-renew-template le-renew-mynewsite.cz
chmod +x le-renew-mynewsite.cz    #if necessary
./le-renew-mynewsite.cz           #just for test

nano le-renew.sh              #add your le-renew-mynewsite.cz
```

9. edit crontab (just once!)
```
crontab -e
```

paste this into last line of crontab:
```
30 2 * * 1 /usr/local/sbin/le-renew.sh >> /var/log/le-renewal.log
```

10. you are done! :-)
