#### ubuntu vsftpd configuration
---

_package installation_

```bash
sudo apt update
sudo apt install vsftpd -y
```
_ftp user creation_

```bash

useradd -m -s /bin/false user-a
sudo passwd user-a
suseradd -m -s /bin/false user-b
sudo passwd user-b
```

_user document root create_

|users| document root|
|---|---|
|user-a|/var/www/api.fourtimes.ml|
|user-b|/var/www/ai.fourtimes.ml|

```bash

```
_configuration changes_

backup the original file  for future issues.

```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.original
```

adjust the configuration according to the needs

`sudo vim /etc/vsftpd.conf`

```conf
# sudo vim /etc/vsftpd.conf
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_file=/var/log/vsftpd.log
xferlog_std_format=NO
idle_session_timeout=600
data_connection_timeout=120
ascii_upload_enable=YES
ascii_download_enable=YES
ftpd_banner=Welcome to fourtimes.ml FTP service.
chroot_local_user=NO
chroot_list_enable=YES
user_config_dir=/etc/vsftpd/users
allow_writeable_chroot=YES
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
chroot_list_file=/etc/vsftpd.chroot_list

```


create vsftpd users list

```bash
sudo mkdir -p /etc/vsftpd/users

sudo mkdir -p /var/www/api.fourtimes.ml
sudo chown -R user-a:user-a /var/www/api.fourtimes.ml
echo "local_root=/var/www/api.fourtimes.ml" | sudo tee  /etc/vsftpd/users/user-a

sudo mkdir -p /var/www/ai.fourtimes.ml
sudo chown -R user-b:user-b /var/www/ai.fourtimes.ml
echo "local_root=/var/www/ai.fourtimes.ml" | sudo tee /etc/vsftpd/users/user-b
```

_restrict the users at root level_

```bash
echo "user-a" | tee  /etc/vsftpd.chroot_list
echo "user-b" | tee -a /etc/vsftpd.chroot_list
```


_restart the service_

```bash

systemctl restart vsftpd
systemctl status vsftpd

```
 If the /home directory does not appear, check your user shell type, delete your user, then create a new user with the adduser command.

