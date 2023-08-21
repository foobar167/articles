   - [Task](#task)
   1. [Request for Grid certificate](#request)
      - [Links to read](#links)
      - 1.1. Install OpenSSL utility
      - 1.2. Download configuration file `globus-user-ssl.conf`
      - 1.3. Generate a request for Grid certificate
      - 1.4. Obtain and verify `usercert_request.pem` and `userkey.pem`
      - 1.5. Send request for certificate to the local request authority
      - 1.6. Check the certificate
   2. [Install GSI openssh client](#install)
   3. [Login via GSI SSH client](#login)
   4. [Server management](#manage)
      - [How to transfer files via `gsiscp` and `gsisftp`](#transfer-files)
      - [How-to manage resources](#resource-management)

---
### <a name="task" />Task
   1. Create request for personal Grid certificate.
   2. Install GSI SSH client for **Ubuntu 18.04**.
   3. Connect via GSI SSH to the server.
   4. SURFsara server resource management.

---
### <a name="request" />1. Request for Grid certificate

   * Tested on Windows 10, but should work on Linux too.
   * Country: Belarus
   * Institution Name: United Institute of Informatics Problems ([UIIP NASB](http://uiip.bas-net.by/eng)).
   * Local authority for Belarus. Website http://ca.grid.by doesn't function for now (2019.04.05).
   * The authority member for Belarus is BYGCA, Alexander Shahk
   ([Шах Александр Геннадьевич](http://uiip.bas-net.by/struct/orti.php?sphrase_id=238098),
   e-mail: shag at uiip dot bas-net dot by).
   * SEE-GRID Certification Authority: [Nikos Nikoloutsakos](https://see-grid-ca.hellasgrid.gr/about/)
   (e-mail: nikoloutsa at admin dot grnet dot gr, Greece, GRNET)

----
#### <a name="links" />Links to read
Request creation:
   - [Configuration file](http://uiip.bas-net.by/ca/misc)
   - [Certification center UIIP NASB](http://uiip.bas-net.by/ca)
   - [BYGCA.pem certificate for Belarus](https://github.com/dCache/dcache-docker/blob/master/dcache/dcache/etc/grid-security/certificates/BYGCA.pem)
   - [Contents of current OSG CACert Distribution (version 1.78IGTFNEW)](https://repo.opensciencegrid.org/cadist)
   - [Операционный центр национальной грид-сети Республики Беларусь](http://noc.grid.basnet.by/index.php?n=Main.Ca)
   - [EUGridPMA Membership](https://www.eugridpma.org/members/membership)
   - [SEE-GRID Certification Authority](https://see-grid-ca.hellasgrid.gr/about)
   - [Certificate request generation via the command line and without configuration file](https://see-grid-ca.hellasgrid.gr/documents/certificate-requests)
   - [Небольшой рассказ про X.509](http://ca.grid.kiae.ru/RDIG/info/x509.html)

Installation of GSI SSH client:
   * [Install GSI openssh client (Ubuntu and Debian)](https://www.lsc-group.phys.uwm.edu/lscdatagrid/doc/installgsiopensshclient.html)

---
&zwnj;1.1. Install OpenSSL utility.
   - for Windows: [openssl-0.9.8h-1-setup.exe](https://sourceforge.net/projects/gnuwin32/)
     via http://gnuwin32.sourceforge.net/packages/openssl.htm
   - for Linux: openssl package included into your version of operating system

---
&zwnj;1.2. Download configuration file `globus-user-ssl.conf` from
[certification center UIIP NASB](http://uiip.bas-net.by/ca/misc).
Edit config file and change `default_bits` from 1024 to 2048.

```shell script
#
# Belarusian Grid Certification Authority
# Configuration for user certificate requests
#

[ req ]
default_bits		= 2048
default_keyfile 	= userkey.pem
distinguished_name	= req_distinguished_name

[ req_distinguished_name ]
0.domainComponent		= DomainComponent (by)
0.domainComponent_default	= by
1.domainComponent		= DomainComponent (grid)
1.domainComponent_default	= grid
organizationName		= Domain of your organization (e.g. uiip.bas-net.by)
commonName                       = Name (e.g., Francysk Skaryna)
commonName_max                   = 64
```

---
&zwnj;1.3. Generate a request for Grid certificate:
```shell script
# Run OpenSSL utility and generate file: usercert_request.pem
# Enter and remember your pass phrase for the file: userkey.pem

openssl req -new -config globus-user-ssl.conf -out usercert_request.pem -sha256

    WARNING: can't open config file: /usr/local/ssl/openssl.cnf
    Generating a 2048 bit RSA private key
    ....................................................+++
    .........................+++
    unable to write 'random state'
    writing new private key to 'userkey.pem'
    Enter PEM pass phrase:
    Verifying - Enter PEM pass phrase:
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    DomainComponent (by) [by]:by
    DomainComponent (grid) [grid]:grid
    Domain of your organization (e.g. uiip.bas-net.by) []:uiip.bas-net.by
    Name (e.g., Francysk Skaryna) []:Foo Bar
```

---
&zwnj;1.4. Two files are created: `usercert_request.pem` (a request for certificate) and
`userkey.pem` (an encrypted private key, hide it).

Verify created request PEM file:
```shell script
# Verify request
openssl req -in usercert_request.pem -noout -text

    WARNING: can't open config file: /usr/local/ssl/openssl.cnf
    Unable to load config info from /usr/local/ssl/openssl.cnf
    Certificate Request:
        Data:
            Version: 0 (0x0)
            Subject: DC=by, DC=grid, O=uiip.bas-net.by, CN=Foo Bar
            Subject Public Key Info:
                Public Key Algorithm: rsaEncryption
                    Public-Key: (2048 bit)
                    Modulus:
                        00:91:5f:2c:83:70:b9:e5:be:38:94:b6:28:f8:2f:
                        58:9c:b4:a3:73:ca:61:4b:89:bb:91:08:b6:23:bd:
                        9f:dd:a4:00:cc:20:09:3d:82:0c:7c:a8:49:54:6c:
                        99:c1:63:02:84:c5:ce:1b:7c:d7:3c:58:23:7b:13:
                        45:10:d9:2d:fa:31:09:79:5d:9d:c8:98:48:e2:0b:
                        60:54:6c:6b:00:3d:2a:59:29:ec:1d:1f:2e:cc:ae:
                        05:47:ea:82:48:fd:63:73:6d:13:ec:4b:f7:4d:00:
                        32:7c:79:2a:e7:64:f3:7b:47:ce:d0:97:92:6c:f0:
                        84:a3:ae:6b:51:82:a6:ee:a6:55:50:d1:5d:1b:c7:
                        7f:ad:e6:1d:e8:f7:5c:80:c8:a4:99:f5:ed:df:bf:
                        3b:d7:cc:c2:16:68:03:23:38:69:74:b4:a3:65:3a:
                        c4:86:72:64:f6:0e:89:f1:42:c7:1a:09:f4:24:bc:
                        87:a0:e4:97:30:6f:79:c5:22:91:b7:08:33:b3:0a:
                        b6:7a:e6:e1:d0:f7:e1:46:12:04:ab:f8:aa:60:c9:
                        7d:e5:53:69:f4:a4:1c:fe:98:0b:12:1c:d3:ac:b2:
                        93:23:be:6f:72:d7:a3:f6:58:0c:e7:6c:46:e9:bc:
                        50:6a:78:67:c3:c9:7c:59:ae:36:c0:08:e3:80:f4:
                        a5:2f
                    Exponent: 65537 (0x10001)
            Attributes:
                a0:00
        Signature Algorithm: sha256WithRSAEncryption
             7a:54:85:b0:e3:01:dd:a8:31:92:2c:63:0b:fb:86:b5:a6:9f:
             7d:b0:1a:8b:80:da:8d:ff:6f:a9:1a:b6:59:83:88:1d:7e:bb:
             74:e2:38:34:82:82:a6:4a:26:65:82:d0:70:6e:94:89:31:ff:
             cc:60:5d:cf:f8:82:48:a2:1c:0e:f5:ce:13:49:23:86:93:0c:
             01:4b:8f:bc:b1:98:bd:20:fd:22:16:89:64:4b:f1:c9:0c:1e:
             26:39:64:24:75:22:7c:8a:8b:63:29:d9:96:d9:bd:a7:04:bf:
             5c:aa:41:84:6c:96:65:c0:e7:d3:d9:72:c1:10:26:8a:d2:c4:
             65:af:3e:06:61:71:d8:2a:aa:5a:0e:2d:9b:43:39:86:1c:82:
             84:7b:39:89:74:cd:60:e8:85:59:1f:c7:3c:3b:ba:54:5a:9f:
             30:50:77:ca:b6:b9:dc:e4:f7:86:6e:29:2a:30:98:0a:c3:ef:
             b0:96:09:f3:42:7d:52:0e:4e:46:75:42:e8:c9:76:ca:d9:a1:
             9a:26:fe:a7:2a:c3:37:65:3d:d2:c0:a6:8c:25:a6:6d:37:89:
             4a:c9:96:48:83:8e:cb:eb:26:9c:6e:5c:89:cb:a9:a1:f5:80:
             6b:43:35:47:ef:e8:e5:34:ed:50:6b:ee:72:79:25:a9:e7:1b:
             eb:8c:70:f6
```

Field "Subject" should be like this: `DC=by, DC=grid, O=uiip.bas-net.by, CN=Foo Bar`

---
&zwnj;1.5. Send request for certificate (file `usercert_request.pem`)
to the local request authority with the following information:
```text
Foo Bar personal info:
1. Firstname Lastname: Foo Bar
2. Institution Name: United Institute of Informatics Problems (UIIP NASB)
3. Address: Surganova 6, 220012 Minsk, Belarus
4. Subdivision: Laboratory of Informatics
5. Position/Role: Software Engineer
6. E-mail: foobar167@gmail.com
7. Phone number: Office +375-17-225-12-34, Mobile: +375-29-225-12-34
8. Certificate request file "userkey.pem" is attached to the letter.

Send additional information for LDAP entries:
FooBar_usercert.pem, DN: CN=Foo Bar,O=uiip.bas-net.by,DC=grid,DC=by; Mr. Foo Bar, foobar167@gmail.com, +375-29-123-34-67, BY

For SURFsara Cartesius server system administrators
send IP-address from which you're going to login to the SURFsara Cartesius server.
They'll add this IP-address to their whitelist.
```

Do not send file `userkey.pem`. This is your private, secret key.

From the local request authority you should receive `usercert.pem` file with personal certificate.

---
&zwnj;1.6. Check the certificate

After receiving your certificate via e-mail (filename `Surname_usercert.pem`) verify it:
```shell script
# Have to be OK. Any other output means verification is wrong.
openssl verify -verbose -CAfile BYGCA.pem Surname_usercert.pem

    WARNING: can't open config file: /usr/local/ssl/openssl.cnf
    Surname_usercert.pem: OK
```
where file [BYGCA.pem](https://github.com/dCache/dcache-docker/blob/master/dcache/dcache/etc/grid-security/certificates/BYGCA.pem)
is a certificate authority (CA) file to verify your certificates for Belarus.

You can download other certificates for other countries and institutions from
[this repository](https://github.com/dCache/dcache-docker/tree/master/dcache/dcache/etc/grid-security/certificates).
Path: `dcache-docker/dcache/dcache/etc/grid-security/certificates/`.

---
### <a name="install" />2. Install GSI openssh client

<b>Note:</b> Your IP-address should be whitelisted for the gsi-ssh on the SURFsara server.
Login only from whitelisted IP-address.

GSI SSH client installation:

```shell script
mkdir -p ~/Documents/Install/Globus_Toolkit
cd ~/Documents/Install/Globus_Toolkit

# Download Globus Tollkit package and add it to repository.
wget http://toolkit.globus.org/ftppub/gt6/installers/repo/globus-toolkit-repo_latest_all.deb
sudo dpkg -i globus-toolkit-repo_latest_all.deb

# This should install gsissh, gsiscp and gsisftp.
sudo apt install gsi-openssh-clients

# Install globus-proxy-utils
apt search globus-proxy-utils
sudo apt install globus-proxy-utils

# Download and install the OSG X509 trusted certificates bundle,
# which is required to access the LSC clusters.
wget http://software.ligo.org/gridtools/debian/pool/main/o/osg-ca-certs/osg-ca-certs-1.79NEW-0.deb

# This will add the certificates to a
# /etc/grid-security/certificates directory
sudo dpkg -i osg-ca-certs-1.79NEW-0.deb
```

---
### <a name="login" />3. Login via GSI SSH client

You must have certificate `usercert.pem` and
encrypted private key `userkey.pem` to configure your GSI SSH client:

```shell script
# Login to the computer with whitelisted IP-address, i.e. DeepLab3.

# Create .globus/certificates directory in your $HOME
mkdir -p ~/.globus/certificates

# Move certificate and encrypted private key. Rename them.
# to the ~/.globus directory
mv Surname_usercert.pem ~/.globus/usercert.pem
mv Surname_userkey.pem ~/.globus/userkey.pem

# Change permissions to read only
cd ~/.globus/
chmod ugo-rwx usercert.pem userkey.pem
chmod u+r usercert.pem userkey.pem
ls -hal

# Copy all trusted certificates bundle from /etc/grid-security/certificates
# to your ~/.globus/certificates/ directory
cp /etc/grid-security/certificates/* ~/.globus/certificates/
```

Configuration of GSI SSH client is finished. Then to connect to the server
you should create proxy and use `gsissh` command.

```shell script
# Create proxy connection for 12 hours. Must enter GRID pass phrase.
grid-proxy-init -debug -verify

    User Cert File: /home/username/.globus/usercert.pem
    User Key File: /home/username/.globus/userkey.pem
    
    Trusted CA Cert Dir: /home/username/.globus/certificates
    
    Output File: /tmp/x509up_u1003
    Your identity: /DC=by/DC=grid/O=uiip.bas-net.by/CN=Name Surname
    Enter GRID pass phrase for this identity:
    Creating proxy ...+++
    ................................+++
     Done
    Proxy Verify OK
    Your proxy is valid until: Fri Apr  5 08:47:41 2019

# Now try to connect via GSI SSH
gsissh int1-bb.cartesius.surfsara.nl -p 2222
                               SURFsara
                           Cartesius System

   Welcome to SURFsara,
   Some text here... doesn't matter.
   
   Consult https://userinfo.surfsara.nl/systems/cartesius
   for information on system usage.
Last login: Thu Apr  4 20:06:42 2019 from 80.94.171.57
********************************************************************************
* Some text here... doesn't matter.                                            *
********************************************************************************
* Questions? Please e-mail to helpdesk <at> surfsara <dot> nl                  *
******************************************* last modified:  02-04-2019 10:30 ***
-bash-4.2$ 
```

---
### <a name="manage" />4. Server management

You can [install Anaconda virtual environment](05_Virtual_environments.md/#anaconda)
or [use EasyBuild framework](05_Virtual_environments.md/#easy-build) that allows you to
manage (scientific) software on High Performance Computing (HPC) systems in an efficient way.

#### <a name="transfer-files" />How to transfer files via `gsiscp` and `gsisftp`

Links to read:
   * [SSH/TransferFiles](https://help.ubuntu.com/community/SSH/TransferFiles)
   * [10 SCP Commands to Transfer Files/Folders in Linux](https://www.tecmint.com/scp-commands-examples)
   * [12 scp command examples to transfer files on Linux](https://www.binarytides.com/linux-scp-command)
   * [How to Use Linux SFTP Command to Transfer Files](https://linuxize.com/post/how-to-use-linux-sftp-command-to-transfer-files)
   * [Linux sftp command](https://www.computerhope.com/unix/sftp.htm)

```shell script
# Download from the URL
export DIR=Downloads/mydata  # set directory environment variable
mkdir -p ~/"$DIR"  # make directory
cd ~/"$DIR"        # goto this directory
wget https://storage.googleapis.com/laurencemoroney-blog.appspot.com/validation-horse-or-human.zip
unzip validation-horse-or-human.zip -d validation-horse-or-human

# Upload from DeepLab3 to SURFsara.
grid-proxy-init -debug -verify  # initialize proxy for 12 hours
# Make directory on remote host
gsissh int1-bb.cartesius.surfsara.nl -p 2222 "mkdir -p ~/"$DIR

# -C parameter to compress files on the go
# -r parameter for recursion, to upload directory recursively
# -v parameter for verbosity
# Port: -P 2222
# File to upload: validation-horse-or-human.zip
# Remote host: int1-bb.cartesius.surfsara.nl
# Destination directory: ~/Downloads/mydata
# If you upload file twice, it will rewrite the old file without warning.
gsiscp -Cr -P 2222 validation-horse-or-human.zip int1-bb.cartesius.surfsara.nl:~/$DIR

# Upload from SURFsara to DeepLab3. You must enter your 'username' and password.
export DIR=Downloads/mydata/data1  # set directory environment variable
ssh username@80.94.171.57 -p 2222 "mkdir -p ~/"$DIR  # make directory remotely
scp -Cr -P 2222 validation-horse-or-human.zip username@80.94.171.57:~/$DIR

# SFTP connection from DeepLab3 to SURFsara.
grid-proxy-init -debug -verify  # initialize proxy for 12 hours
gsisftp -C -P 2222 int1-bb.cartesius.surfsara.nl  # login

# 'sftp>' command line prompt should appear.
sftp>
help  # type 'help' for available commands
lls -hal
lcd ~/Downloads/mydata/data1  # change local directory
lpwd
ls -hal
cd Downloads/mydata  # change remote directory
pwd
mkdir data2  # make remote directory
cd data2
put -r validation-horse-or-human.zip  # upload files recursively from DeepLab3 to SURFsara
bye  # or exit

# SFTP connection from SURFsara to DeepLab3. You must enter your 'username' and password.
sftp -C -P 2222 username@80.94.171.57
lcd ~/Downloads/mydata
cd Downloads/mydata
mkdir data3
ls -hal
cd data3
put -r validation-horse-or-human.zip  # upload files recursively from SURFsara to DeepLab3
exit
```

#### <a name="resource-management" />How-to manage resources

Links to read:
   * [Getting started with Cartesius](https://userinfo.surfsara.nl/systems/cartesius/getting-started)
   * Resource management [SURFsara Cartesius batch usage](https://userinfo.surfsara.nl/systems/cartesius/usage/batch-usage).
   * [Cartesius file systems](https://userinfo.surfsara.nl/systems/cartesius/filesystems).
   * Unfortunately GSI users **can not** login at [portal](https://portal.surfsara.nl)
   * [SLURM official website](https://slurm.schedmd.com). SLURM is the scheduling system used on Cartesius.
   * Track resources and [budget (core hours)](https://userinfo.surfsara.nl/systems/cartesius/getting-started#budget).
   * One has to have a basic knowledge of [shell programming](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html).

```text
We have 150 000 System Billing Units (SBUs)
1 SBU == 1 core for 1 hour on a fat and thin nodes
1 SBU == 1 core for 20 minutes on a GPU node
3 SBU == 1 core for 60 minutes on a GPU node
1 GPU node == 16 CPU cores and 2 NVIDIA Tesla K40m GPUs
1 fat/thin node == 16 CPU cores without GPU

Calculating on 1 GPU node for 1 hour is equal to
16 cores * 3 SBU = 48 SBU/hour

150 000 SBU == 130 days of calculations on 1 GPU node
```

Use resources carefully.

```shell script
# Hours are trackable on the system like this:
budget-overview  # get a more dynamic and accurate overview of the current state of your account's budget
accinfo          # view the budget, user list, and other details
accuse           # view the daily/monthly usage per user/account
myquota          # check the disk quota
```

Note that the information given by `accuse` and `accinfo` is only up-to-date
for the situation at the end of the previous day.
I.e.: the day before the commands are issued.

[You have 200 GiB](https://userinfo.surfsara.nl/systems/cartesius/getting-started#filesystems)
on the `$HOME` directory and 8 TiB on the `Scratch` file system, but only for 14 days.
Keep in mind that any file on `scratch` folder than 14 days will be **automatically removed**.
If necessary we can request for additional space in the `Projects` file system
or `/projects/0/<project_name>` directory.

```shell script
myquota  # check the disk quota

# 200 GiB on the $HOME directory
# 8 TiB on the /scratch/shared directory for 14 days only
df -h /scratch/shared/
mkdir /scratch/shared/paulenka
cp validation-horse-or-human.zip /scratch/shared/paulenka/
ls -hal /scratch-shared/paulenka  # the same as /scratch/shared/

echo $TMPDIR  # /scratch-local/pr1d1005
# You can also use /scratch-local for 14 days
mkdir /scratch-local/paulenka
cp validation-horse-or-human.zip /scratch-local/paulenka/
```

Activate conda `DL3` virtual environment:
```shell script
module load Anaconda3/5.3.0  # load installed Anaconda
source activate DL3          # activate DL3 virtual environment
```

Create simple SLURM batch file `~/Downloads/example.slurm`:
```shell script
mkdir -p ~/Downloads/
touch ~/Downloads/example.slurm
# Make file executable, otherwise it will not run with 'srun' command
chmod u+x ~/Downloads/example.slurm
# Open 'nano' editor
nano ~/Downloads/example.slurm
```

Contents of `example.slurm` batch file:

```shell script
#!/bin/bash
#SBATCH -t 30:00
#SBATCH -N 1
#SBATCH -p gpu

module load Anaconda3/5.3.0  # load installed Anaconda
source activate DL3          # activate DL3 virtual environment

echo "Start of job at `date`"
python3 -c 'import tensorflow as tf; print(tf.__version__)'
echo "End of job at `date`"

conda deactivate  # deactivate DL3 virtual environment

```

where:
   * `-t` is the maximum wall clock time. If the job runs longer, it will **get terminated**.
   * `-N` is the number of nodes used for your calculations. `-N 1` is equal 48 SBUs per hour.
   * `-p` is the partition. Use `gpu_short` for for test and debug runs and `gpu` for production
   runs on GPU nodes. Show available partitions list with the command
   `sinfo` or `scontrol show partition`.

Run simple Slurm batch file `example.slurm`.

```shell script
srun -N 1 -t 30:00 -p gpu ~/Downloads/example.slurm  # run job in foreground mode
    Start of job at Thu May  2 12:35:54 CEST 2019
    1.13.1
    End of job at Thu May  2 12:35:57 CEST 2019

sbatch ~/Downloads/example.slurm   # run job in background mode
    Submitted batch job 6279513

cat ~/Downloads/slurm-6279513.out  # view script output
    Start of job at Thu May  2 12:33:37 CEST 2019
    1.13.1
    End of job at Thu May  2 12:33:41 CEST 2019
```

By default, SLURM willl redirect all output to a file called `<jobid>.out`,
which will be placed in the directory from where you submitted your job.
This file contains both the standard output and error streams, combined.
If you want to send these two streams to different files, you can achieve this
by setting certain `sbatch` parameters.

Control your jobs:

```shell script
squeue -u $USER     # check the current state of the queue
scancel -j <jobid>  # remove one of your jobs from the queue
```
