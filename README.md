# Complete installation of X-UI panel
- [How to fully install the XUI panel.](https://github.com/MR-Amoori/Complete_installation_of_X-UI_panel/blob/Master/README.md#install-x-ui-panel "How to fully install the XUI panel.")
- [Get an SSL certificate for the domain.](https://github.com/MR-Amoori/Complete_installation_of_X-UI_panel/blob/Master/README.md#download-and-install-the-acme-script-for-getting-a-free-ssl-certificate "Get an SSL certificate for the domain.")
- [Install Google BBR.](https://github.com/MR-Amoori/Complete_installation_of_X-UI_panel/blob/Master/README.md#install-google-bbr "Install Google BBR.")
- [IR domains filter and Iran IPs filter to reduce the possibility of being filtered.](https://github.com/MR-Amoori/Complete_installation_of_X-UI_panel/blob/Master/README.md#routing-ir-domains-filter-and-iran-ips-filter-to-reduce-the-possibility-of-being-filtered "IR domains filter and Iran IPs filter to reduce the possibility of being filtered.")
- [Installing WordPress on the server.](https://github.com/MR-Amoori/Complete_installation_of_X-UI_panel/blob/Master/README.md#install-wordpress-on-ubunto "Installing WordPress on the server.")

------------


#  Install X-UI Panel

#### server up to date:

    apt update && apt upgrade -y


#### Also install curl and socat:



    apt install curl socat -y




#### Download and install the Acme script for getting a free SSL certificate:



    curl https://get.acme.sh | sh




#### Set the default provider to Let’s Encrypt:



    ~/.acme.sh/acme.sh --set-default-ca --server letsencrypt




#### Register your account for a free SSL certificate:



    ~/.acme.sh/acme.sh --register-account -m name@gmail.com




### Obtain an SSL certificate:



    ~/.acme.sh/acme.sh --issue -d sub.host.top --standalone




#### Therefore install the certificate and key to a permanent location:



    ~/.acme.sh/acme.sh --installcert -d sub.host.top --key-file /root/private.key --fullchain-file /root/cert.crt




### Run the X-UI Install Script:

###### Chinese:


    bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)

###### English & Persian:

    bash <(curl -Ls https://raw.githubusercontent.com/NidukaAkalanka/x-ui-english/master/install.sh)
    
###### English & Persian (Update & Custom Version):
    
    bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)

###### Username: admin
###### Password: admin
###### Port: 54321




### Install google BBR:

###### type >  x-ui  > select 15 



------------



## Enable HTTPS on Panel:

#### Panel settings:

> Panel certificate public key file path: ' /root/cert.crt '
 Panel certificate key file path: ' /root/private.key '




## Routing (IR domains filter and Iran IPs filter to reduce the possibility of being filtered):

 

    sudo -i
    cd /usr/local/x-ui/bin
    wget https://github.com/bootmortis/iran-hosted-domains/releases/latest/download/iran.dat
    wget https://github.com/v2fly/domain-list-community/releases/latest/download/dlc.dat




### Replace the config in x-ui panel settings:

```json
{
  "api": {
    "services": [
      "HandlerService",
      "LoggerService",
      "StatsService"
    ],
    "tag": "api"
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "port": 62789,
      "protocol": "dokodemo-door",
      "settings": {
        "address": "127.0.0.1"
      },
      "tag": "api"
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
  "policy": {
    "system": {
      "statsInboundDownlink": true,
      "statsInboundUplink": true
    }
  },
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api",
        "type": "field"
      },
      {
        "ip": [
          "geoip:private",
          "geoip:ir"
        ],
        "outboundTag": "blocked",
        "type": "field"
      },
      {
        "outboundTag": "blocked",
        "protocol": [
          "bittorrent"
        ],
        "type": "field"
      },
      {
        "outboundTag": "blocked",
          "domain": [
            "regexp:.*\\.ir$",
            "ext:iran.dat:ir",
            "ext:iran.dat:other",
            "geosite:category-ir-gov",
            "geosite:category-ir-news",
            "geosite:category-ir-bank",
            "geosite:category-ir-tech",
            "geosite:category-ir-travel",
            "geosite:category-ir-shopping",
            "geosite:category-ir-insurance",
            "geosite:category-ir-scholar",
            "snapp", "digikala","tapsi", "blogfa", "bank", "sb24.com", "sheypoor.com", "tebyan.net", "beytoote.com", "telewebion.com", "Film2movie.ws", "Setare.com", "Filimo.com", "Torob.com", "Tgju.org", "Sarzamindownload.com", "downloadha.com", "P30download.com", "Sanjesh.org"
          ],
        "type": "field"
      }
    ]
  },
  "stats": {}
}

```

### Open the required ports and close the other ports

``` 
ufw enable -y
ufw allow 22/tcp
ufw allow 2053/tcp
ufw allow 443/tcp
ufw allow 2087/tcp
```

------------



## Install wordpress on ubunto:

###### Update & Upgrade Server : 

``` 
apt update -y && apt upgrade -y 
```

###### Install Apache : 

```
apt install apache2 -y
```

###### Install MySQL : 

```
apt install mariadb-server mariadb-client -y
```

###### SecureMariaDB :

```
mysql_secure_installation
```

>Change Root Password : N

>Remove anonymous users: Y

>Disallow root login remotely: N

>Remove test database: Y

>Reload privilege table now? : Y


###### Install PHP :

```
apt install php php-mysql -y
```

###### Create DataBase : 

```
mysql -u root -p
```

```
CREATE DATABASE wordpress_db;
```

###### Create Database User:  

```
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'password';
```

###### User Permissions : 

```
GRANT ALL ON wordpress_db.* TO 'wp_user'@'localhost' IDENTIFIED BY 'password';
```

###### Exit of Database : 

```
FLUSH PRIVILEGES;
Exit;
```

###### Install WP CMS : 

```
cd /tmp && wget https://wordpress.org/latest.tar.gz
```

###### Unzip WP : 

```
tar -xvf latest.tar.gz
```

###### Change Dir of WP :  

```
cp -R wordpress /var/www/html/
```

###### Run to change the ownership : 

```
chown -R www-data:www-data /var/www/html/wordpress/
```

###### change permission to WP folder : 

```
chmod -R 755 /var/www/html/wordpress/
```

###### Create Upload Dir: 

```
mkdir /var/www/html/wordpress/wp-content/uploads
```

###### Change Permissions of Upload Folder: 

```
chown -R www-data:www-data /var/www/html/wordpress/wp-content/uploads/
```

###### Move the WordPress directory to /var/www/html and changes in the config file

```
cd / && cd /var/www/html
rm index.html
cd wordpress
mv * /var/www/html
cd ..
rm -rf wordpress
mv wp-config-sample.php wp-config.php
nano wp-config.php
```

###### Install WP by UI: 

```
https://server-ip-address-or-domain/wordpress
```


