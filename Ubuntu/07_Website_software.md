   - [Task](#task)
   - [Links](#links)
   - [Allow user permissions](#permissions)
      - [Relative articles](#relative-articles)
      - [Allow to execute `sudo` commands](#allow-execute)
      - [Allow to write in the system folder](#allow-write)
         - [Group `webmasters`](#webmasters)
         - [Group `xray`](#xray)
         - [Group `technology-sg`](#technology-sg)
   - [Autorun service](#autorun)
   - [Backup and delete deprecated user accounts](#accounts)
   - [Configure `nginx`](#configure)
   - [Correctly delete `nginx`](#nginx)
   - [File compressor library](#file-compressor)
   - [Install Certbot for `nginx`](#certbot)
   - [ORY Kratos deployment](#kratos)
   - [Packages for the website](#website)
   - [SQLite 3](#sqlite)
   - [Work with Nginx server and its API](#work)

---
## <a name="task" />Task

   - Install software for the website https://image.org.by

---
## <a name="links" />Links

   - [Valery Malyshev project link](https://github.com/MalyshevValery/Generator_Back)
   - [Alexander Yaroshevich project link](https://github.com/Vozf/slide_analysis_api)

---
## <a name="permissions" />Allow user permissions

:exclamation: **For trusted users only.** :exclamation:

### <a name="relative-articles" />Relative articles

   * [Ubuntu Docs - Sudoers](https://help.ubuntu.com/community/Sudoers)
   * [How To Edit the Sudoers File on Ubuntu and CentOS](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos)
   * [Take Control of your Linux | sudoers file: How to with Examples](https://www.garron.me/en/linux/visudo-command-sudoers-file-sudo-default-editor.html)
   * [FilePermissionsACLs](https://help.ubuntu.com/community/FilePermissionsACLs)
   * [Nagayoshi answer on StackOverflow](https://unix.stackexchange.com/a/584507/323648)
   * [My answer on AskUbuntu.com](https://askubuntu.com/a/1098707/672237)

### <a name="allow-execute" />Allow to execute `sudo` commands

Allow to execute `sudo` commands without granting root permissions.

Create and edit file with `visudo` editor
in the directory `/etc/sudoers.d/`.

```shell script
# Create and edit file for 'website' group
sudo visudo -f /etc/sudoers.d/website

# Write into the file /etc/sudoers.d/website

# Create alias for WEBMASTERS group
User_Alias WEBMASTERS = username, webmaster, malyshevvalery

# Create commands alias to start, stop and restart some services and view BIOS
Cmnd_Alias START1   = /bin/systemctl start nginx,              \
                      /bin/systemctl start generator,          \
                      /bin/systemctl start slide_analysis_api, \
                      /bin/systemctl start segmentation,       \
                      /bin/systemctl start demoxray,           \
                      /bin/systemctl start adversarial

Cmnd_Alias STOP1    = /bin/systemctl stop nginx,              \
                      /bin/systemctl stop generator,          \
                      /bin/systemctl stop slide_analysis_api, \
                      /bin/systemctl stop segmentation,       \
                      /bin/systemctl stop demoxray,           \
                      /bin/systemctl stop adversarial

Cmnd_Alias RESTART1 = /bin/systemctl restart nginx,              \
                      /bin/systemctl restart generator,          \
                      /bin/systemctl restart slide_analysis_api, \
                      /bin/systemctl restart segmentation,       \
                      /bin/systemctl restart demoxray,           \
                      /bin/systemctl restart adversarial

Cmnd_Alias STATUS1  = /bin/systemctl status nginx,              \
                      /bin/systemctl status generator,          \
                      /bin/systemctl status slide_analysis_api, \
                      /bin/systemctl status segmentation,       \
                      /bin/systemctl status demoxray,           \
                      /bin/systemctl status adversarial

Cmnd_Alias ENABLE1  = /bin/systemctl enable nginx,              \
                      /bin/systemctl enable generator,          \
                      /bin/systemctl enable slide_analysis_api, \
                      /bin/systemctl enable segmentation,       \
                      /bin/systemctl enable demoxray,           \
                      /bin/systemctl enable adversarial

Cmnd_Alias DISABLE1 = /bin/systemctl disable nginx,              \
                      /bin/systemctl disable generator,          \
                      /bin/systemctl disable slide_analysis_api, \
                      /bin/systemctl disable segmentation,       \
                      /bin/systemctl disable demoxray,           \
                      /bin/systemctl disable adversarial

Cmnd_Alias START2   = /usr/sbin/service nginx start,              \
                      /usr/sbin/service generator start,          \
                      /usr/sbin/service slide_analysis_api start, \
                      /usr/sbin/service segmentation start,       \
                      /usr/sbin/service demoxray start,           \
                      /usr/sbin/service adversarial start

Cmnd_Alias STOP2    = /usr/sbin/service nginx stop,              \
                      /usr/sbin/service generator stop,          \
                      /usr/sbin/service slide_analysis_api stop, \
                      /usr/sbin/service segmentation stop,       \
                      /usr/sbin/service demoxray stop,           \
                      /usr/sbin/service adversarial stop

Cmnd_Alias RESTART2 = /usr/sbin/service nginx restart,              \
                      /usr/sbin/service generator restart,          \
                      /usr/sbin/service slide_analysis_api restart, \
                      /usr/sbin/service segmentation restart,       \
                      /usr/sbin/service demoxray restart,           \
                      /usr/sbin/service adversarial restart

Cmnd_Alias STATUS2  = /usr/sbin/service nginx status,              \
                      /usr/sbin/service generator status,          \
                      /usr/sbin/service slide_analysis_api status, \
                      /usr/sbin/service segmentation status,       \
                      /usr/sbin/service demoxray status,           \
                      /usr/sbin/service adversarial status

Cmnd_Alias ENABLE2  = /usr/sbin/service nginx enable,              \
                      /usr/sbin/service generator enable,          \
                      /usr/sbin/service slide_analysis_api enable, \
                      /usr/sbin/service segmentation enable,       \
                      /usr/sbin/service demoxray enable,           \
                      /usr/sbin/service adversarial enable

Cmnd_Alias DISABLE2 = /usr/sbin/service nginx disable,              \
                      /usr/sbin/service generator disable,          \
                      /usr/sbin/service slide_analysis_api disable, \
                      /usr/sbin/service segmentation disable,       \
                      /usr/sbin/service demoxray disable,           \
                      /usr/sbin/sercice adversarial disable

Cmnd_Alias FUSER1   = /bin/fuser 3000/tcp, /bin/fuser -k 3000/tcp
Cmnd_Alias FUSER2   = /bin/fuser 4000/tcp, /bin/fuser -k 4000/tcp
Cmnd_Alias FUSER3   = /bin/fuser 8080/tcp, /bin/fuser -k 8080/tcp
Cmnd_Alias FUSER4   = /bin/fuser 8081/tcp, /bin/fuser -k 8081/tcp
Cmnd_Alias FUSER5   = /bin/fuser 8083/tcp, /bin/fuser -k 8083/tcp
Cmnd_Alias FUSER6   = /bin/fuser 443/tcp,  /bin/fuser -k 443/tcp

Cmnd_Alias STATUS   = /bin/systemctl status
Cmnd_Alias DAEMON   = /bin/systemctl daemon-reload
Cmnd_Alias LDCONFIG = /sbin/ldconfig
Cmnd_Alias BIOS     = /usr/sbin/dmidecode -t bios

# Allow members of WEBMASTERS to restart some services and view BIOS
WEBMASTERS ALL = START1, STOP1, RESTART1, STATUS1, ENABLE1, DISABLE1, \
                 START2, STOP2, RESTART2, STATUS2, ENABLE2, DISABLE2, \
                 FUSER1, FUSER2, FUSER3, FUSER4, FUSER5, FUSER6,      \
                 STATUS, DAEMON, LDCONFIG, BIOS

```

Check it or edit broken configuration file:

```shell script
# To edit broken configuration file
pkexec visudo -f /etc/sudoers.d/website

# Check if it works — view BIOS as 'username' (for root)
sudo -u username sudo dmidecode -t bios    # should work
sudo -u username sudo dmidecode -t memory  # should NOT work

# Check under "username" account
sudo dmidecode -t bios    # should work
sudo dmidecode -t memory  # should NOT work

sudo service nginx restart
sudo systemctl restart nginx
sudo service slide_analysis_api start

sudo fuser 3000/tcp  # view port 3000/tcp
```

### <a name="allow-write" />Allow to write in the system folder

#### <a name="webmasters" />Group `webmasters`

Prepare `webmasters` group:

```shell script
# Check 'webmasters' group doen't exist
cat /etc/group | grep webmasters
# Create 'webmasters' group
sudo addgroup webmasters
# Add users to 'webmasters' group
sudo usermod -a -G webmasters username
sudo usermod -a -G webmasters malyshevvalery
sudo usermod -a -G webmasters webmaster

# Group assignment changes won't take effect
# until the users log out and back in.
```

For `webmasters` group give write permission to directories:

```shell script
# /etc/systemd/system — to start services automatically
# /etc/nginx — for Nginx
# /etc/letsencrypt — for Certbot

# List ACLs
getfacl /etc/nginx/
getfacl /etc/systemd/system
getfacl /etc/letsencrypt

    getfacl: Removing leading '/' from absolute path names
    # file: etc/systemd/system
    # owner: root
    # group: root
    user::rwx
    group::r-x
    other::r-x

# Add 'webmasters' group to an ACL
sudo setfacl -R -m g:webmasters:rwx /etc/nginx
sudo setfacl -R -m g:webmasters:rwx /etc/systemd/system
sudo setfacl -R -m g:webmasters:rx /etc/letsencrypt

# Check
getfacl /etc/nginx
getfacl /etc/systemd/system
getfacl /etc/letsencrypt

    getfacl: Removing leading '/' from absolute path names
    # file: etc/systemd/system
    # owner: root
    # group: root
    user::rwx
    group::r-x
    group:webmasters:rwx
    mask::rwx
    other::r-x

sudo -u username touch /etc/systemd/system/test.txt  # should work
sudo -u username touch /etc/systemd/test.txt  # Permission denied

# Remove entry
sudo setfacl -R -x u:username,g:groupname  /dir/path/
setfacl -b /dir/path  # remove all extended ACL entries
setfacl --help  # for more information
```

Give read permission to files in the directory `/var/log/nginx`.

```shell script
# There is read permission to the directory `/var/log/nginx` itself.
# But the owner of files in this directory is `www-data` and the group is `adm`.
ls -hal /var/log/nginx
    total 560K
    drwxr-xr-x  2 root     adm    4.0K Aug  7 00:12 .
    drwxrwxr-x 14 root     syslog 4.0K Aug  7 00:12 ..
    -rw-r-----  1 www-data adm    122K Aug  7 10:15 access.log
    -rw-r-----  1 www-data adm     26K Aug  5 23:58 access.log.2.gz
    -rw-r-----  1 www-data adm     12K Aug  7 10:09 error.log
    -rw-r-----  1 www-data adm     808 Aug  5 10:32 error.log.2.gz

# So add user to the `adm` group to read files in the directory `/var/log/nginx`.
# Add users to `adm` group.
cat /etc/group | grep adm
sudo usermod -a -G adm username
sudo usermod -a -G adm malyshevvalery
sudo usermod -a -G adm webmaster
cat /etc/group | grep adm
```

#### <a name="xray" />Group `xray`

Prepare `xray` group. Group assignment changes won't take effect until the users log out and back in.

```shell script
# Check `xray` group doen't exist
cat /etc/group | grep xray
# Create `xray` group
sudo addgroup xray
# Add users to `xray` group
sudo usermod -a -G xray skliff13
sudo usermod -a -G xray sergeko
sudo usermod -a -G xray malyshevvalery
sudo usermod -a -G xray pavlenko
sudo usermod -a -G xray ahmed
```

Give write permission for `xray` group:

```shell script
# Recursively remove "other" from read, write and execute directory
sudo chmod -R o-rwx /hdd_purple/PTD_Xray/

# Remove "other" executable recursively from files (not directories)
chmod -R o-x+X /some/folder/name

# Add `xray` group to an ACL
sudo setfacl -R -m g:xray:rwx /hdd_purple/PTD_Xray/

# Change group owner
sudo chgrp -R xray /hdd_purple/PTD_Xray/

# Check it by creating test file
sudo -u pavlenko touch /hdd_purple/PTD_Xray/xray_datasets/test.txt
```

#### <a name="technology-sg" />Group `technology-sg`

```shell script
# Recursively remove "other" from read, write and execute directory
sudo chmod -R o-rwx /hdd_purple/data_technology_sg/

sudo setfacl -R -m g:technology-sg:rwx /hdd_purple/data_technology_sg
sudo chgrp -R technology-sg /hdd_purple/data_technology_sg
```

---
## <a name="autorun" />Autorun service

See [how-to run scripts on startup](02_How-tos.md/#autorun).

There are two services for autorun: `slide_analysis_api` and `generator`.

Config file `cat /etc/systemd/system/slide_analysis_api.service`
[link on GitHub](https://github.com/Vozf/slide_analysis_api):

```text
[Unit]
Description=uWSGI instance to serve slide_analysis_api
After=network.target

[Service]
User=malyshevvalery
Group=www-data
WorkingDirectory=/home/malyshevvalery/Slide_Analysis
Environment="PATH=/home/malyshevvalery/Slide_Analysis"
ExecStart=/home/malyshevvalery/Slide_Analysis/venv/bin/uwsgi --ini /home/malyshevvalery/Slide_Analysis/slide_analysis_api.ini

[Install]
WantedBy=multi-user.target
```

Config file `cat /etc/systemd/system/generator.service`
[link on GitHub](https://github.com/MalyshevValery/Generator_Back/blob/master/generator.service):

```text
[Unit]
Description=uWSGI instance to serve image factory
After=network.target

[Service]
User=malyshevvalery
Group=webmasters

WorkingDirectory=/home/malyshevvalery/Generator_Back
Environment="PATH=/home/malyshevvalery/Generator_Back/venv/bin"
ExecStart=/home/malyshevvalery/Generator_Back/venv/bin/uwsgi --ini uwsgi.ini

[Install]
WantedBy=multi-user.target
```

---
## <a name="accounts" />Backup and delete deprecated user accounts

Remember to backup and delete obsolede and unnecessary user accounts.

```shell script
# Find all directories owned by a particular user
sudo find / -type d -user vozman
# Find all files owned by a particular user. If necessary.
#sudo find / -type f -user vozman

# Disable user accounts
sudo usermod -L vozman
sudo usermod -L romanroskach
#sudo usermod -U tempuser  # enable user account

# Delete users from `webmasters` group
sudo deluser vozman webmasters
sudo deluser romanroskach webmasters

# If error:
#     userdel: user NAME is currently used by process 1234
sudo killall -u username  # kill all user processes
#sudo kill -9 1864  # kill the process
#htop -u username  # list processes by user name

# Delete users
sudo deluser vozman
sudo deluser romanroskach

# Backup the whole directory and delete it from $HOME
du -sh /home/username 2> /dev/null  # show directory size
sudo tar -zcvf /hdd_barracuda1/pavlenko_user_accounts_backups/vozman_2019.12.31_backup.tar.gz /home/vozman
sudo tar -zcvf /hdd_barracuda1/pavlenko_user_accounts_backups/romanroskach_2019.12.31_backup.tar.gz /home/romanroskach
sudo rm -r /home/vozman  # use with caution!
sudo rm -r /home/romanroskach  # use with caution!

# Check
cat /etc/group | grep 'vozman\|romanroskach'
cat /etc/passwd | grep 'vozman\|romanroskach'
ls /home | grep 'vozman\|romanroskach'
```

---
## <a name="configure" />Configure `nginx`

Add configuration files to directories
`/etc/nginx/sites-enabled` and `/etc/nginx/sites-available`.

Open ports 80 for HTTP and 3000 for API

```shell script
# Open ports. Allow incoming TCP and UDP packets.
sudo ufw allow 80    # HTTP
sudo ufw allow 443   # HTTPS
sudo ufw allow 2222  # SSH
sudo ufw allow 3000  # API slide_analysis_api
sudo ufw allow 8080  # HTTP
sudo ufw allow 8081  # API slide_analysis_api
sudo ufw allow 8083  # API slide_analysis_api

# Delete existing rule.
# Simply prefix the original rule with "delete".
# sudo ufw delete allow 8083

# Check if it works

# Check the status of ufw
sudo ufw status
```

Website API works via 4000 port for localhost (not via IP).
With the help of `nginx` 3000 port redirected to 4000.

To check website works locally, enter in your browser's URL
field `http://localhost` or `http://127.0.0.1`.

To check website works globally, enter in your browser's URL
field http://image.org.by or via HTTPS https://image.org.by
Website main page should open.

---
## <a name="nginx" />Correctly delete `nginx`

[Original answer here](https://askubuntu.com/questions/235347/what-is-the-best-way-to-uninstall-nginx)

```shell script
# Removes all but config files
sudo apt remove nginx nginx-common
# Removes everything
sudo apt purge nginx nginx-common
# After using any of the above commands, use this
# in order to remove dependencies used by nginx
# which are no longer required
sudo apt autoremove
## rm -rf /etc/nginx  # to remove the conf files too
```

---
## <a name="file-compressor" />File compressor library

Install high-quality block-sorting file compressor library - development

```shell script
sudo apt install libbz2-dev
```

---
## <a name="certbot" />Install Certbot for `nginx`

### Ubuntu 22.04
[Link to Certbot installation instruction](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx)

```shell script
# Add Certbot PPA
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt update

# Install Certbot
sudo apt install certbot python-certbot-nginx

# Choose how you'd like to run Certbot
sudo certbot --nginx

# Enter the following information:
    Enter email address (used for urgent renewal and security notices): malyshevalery at gmail.com
    
    Which names would you like to activate HTTPS for?
    1: myo.fr.to
    2: apibioimage.hopto.org
    Select the appropriate numbers separated by commas and/or spaces, or leave input
    blank to select all options shown (Enter 'c' to cancel): 2
    
    Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
    1: No redirect - Make no further changes to the webserver configuration.
    2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
    new sites, or if you're confident your site works on HTTPS. You can undo this
    change by editing your web server's configuration.
    Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2

# Test automatic renewal
sudo certbot renew --dry-run
```

### Ubuntu 24.04
[How to Install Nginx Web Server on Ubuntu 24.04](https://docs.vultr.com/how-to-install-nginx-web-server-on-ubuntu-24-04)
```shell
sudo apt update
sudo apt install nginx -y
sudo nginx -version
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl status nginx

sudo snap install --classic certbot
sudo certbot --version
sudo certbot --nginx -d app.example.com --agree-tos
```

To confirm that your site is set up properly, visit https://image.org.by via HTTPS.

You should add read permission to `webmasters` group and
make a secure backup of `/etc/letsencrypt` folder.

---
## <a name="kratos" />ORY Kratos deployment

[ORY Kratos](https://www.ory.sh/kratos/docs/) is an API-first Identity and
User Management system that is built according to cloud architecture best practices.

Links:
   * [How To Install and Use PostgreSQL on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04)
   * [NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions)
   * [How To Install Node.js on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)

Install PostgreSQL
```shell script
sudo apt update
sudo apt install postgresql postgresql-contrib
# Login to default "postgres" user
sudo su - postgres
# Login into the prompt interface
psql
# Give the "postgres" a password
\password postgres
# Exit from "postgres" user
\q
exit
# Change password for the user
passwd postgres
# Restart the server
sudo service postgresql restart

# Check login without "sudo"
su - postgres
```

Install nodejs using NodeSource PPA.
The NodeSource `nodejs` package contains both the `node` binary and `npm`,
so you don’t need to install `npm` separately.
```shell script
# Remove previous version v8.10.0,
# which was installed via "sudo apt install nodejs"
nodejs --version
sudo apt remove nodejs  # retain config files
#sudo apt purge nodejs  # delete config files
sudo apt autoremove

# Install Node.js using NodeSource PPA
cd ~/Downloads
# Version 13.x is not officially supported anymore
#curl -sL https://deb.nodesource.com/setup_13.x -o nodesource_setup.sh
curl -sL https://deb.nodesource.com/setup_15.x -o nodesource_setup.sh
cat nodesource_setup.sh  # take a look
sudo bash nodesource_setup.sh
sudo apt install nodejs
nodejs --version  # v15.1.0
npm --version  # v7.0.8

# Install development tools to build native addons
sudo apt install gcc g++ make

# Install the Yarn package manager
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
```

---
## <a name="website" />Packages for the website

Install packages for https://image.org.by

Github repository with installation instructions https://github.com/Vozf/slide_analysis_api

```shell script
sudo apt install git
sudo apt install python3
sudo apt install python3-pip
sudo apt install python3-tk
sudo apt install openslide-tools
sudo apt install libsm6
sudo apt install libxext6

# Python 2 and 3 wrappers for reading whole slide image files
sudo apt install python-openslide
sudo apt install python3-openslide

# Flask micro web framework
sudo apt install python-flask
sudo apt install python3-flask

# nginx [engine x] is an HTTP and reverse proxy server,
# a mail proxy server, and a generic TCP/UDP proxy server,
# originally written by Igor Sysoev.
sudo apt install nginx
```

Install [GDCM and Pydicom](https://github.com/HealthplusAI/python3-gdcm)

```shell script
git clone --branch master https://github.com/HealthplusAI/python3-gdcm.git && \
cd python3-gdcm && \
sudo dpkg -i build_1-1_amd64.deb && \
sudo apt-get install -f

# Not necessary for whole the system
cp /usr/local/lib/gdcm.py /usr/local/lib/python3.6/site-packages/.
cp /usr/local/lib/gdcmswig.py /usr/local/lib/python3.6/site-packages/.
cp /usr/local/lib/_gdcmswig.so /usr/local/lib/python3.6/site-packages/.
cp /usr/local/lib/libgdcm* /usr/local/lib/python3.6/site-packages/.
```

Install ["libvips" package](https://zoomadmin.com/HowToInstall/UbuntuPackage/libvips).
VIPS is an image processing system. It is good with large images
(images larger than the amount of RAM in your machine),
and for working with colour. It can perform many image manipulation
tasks much faster than other packages.

```shell script
sudo apt-get update -y
sudo apt-get install -y libvips
```

---
## <a name="sqlite" />SQLite 3

Install SQLite 3 development files:

```shell script
sudo apt install libsqlite3-dev
```

---
## <a name="work" />Work with Nginx server and its API

- [Ubuntu Linux: Start / Restart / Stop Nginx Web Server](https://www.cyberciti.biz/faq/nginx-restart-ubuntu-linux-command)

```shell script
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl status nginx
sudo systemctl status nginx.service
# or
sudo service nginx stop
sudo service nginx start
sudo service nginx restart
sudo service nginx status
# or
sudo nginx -s reload
# or
systemctl is-active nginx
systemctl is-active slide_analysis_api

# Check for the syntax error in config file
sudo nginx -t
# Check for nginx server log files
sudo tail -f /var/log/nginx/error.log
# or
sudo journalctl -xe

# There is API for Nginx server
sudo systemctl start slide_analysis_api
```

Two services: `nginx` and `slide_analysis_api`, must start
automatically after computer reboots.
Service `nginx` start automatically after software installation
through SysV (`man update-rc.d`).
Service `slide_analysis_api` is using config file
`/etc/systemd/system/slide_analysis_api.service` for autorun.
See [how-to run scripts on start up](02_How-tos.md/#autorun) for more details.

```shell script
# Add slide_analysis_api to autorun
sudo systemctl daemon-reload
sudo systemctl enable slide_analysis_api.service
```

Check it: reboot and wait for 3-5 minutes for service to start.
You should see the images on the website.
