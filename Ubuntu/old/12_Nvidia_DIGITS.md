   - [Task](#task)
   - [Useful links](#useful)
   - [Install DIGITS](#install)
   - [Start DIGITS](#start)
   - [Start DIGITS automatically](#autostart)

---
### <a name="task" />Task
   - Nvidia DIGITS configuration and administration.

---
### <a name="useful" />Useful Nvidia DIGITS links

   - [Getting Started With The DIGITS Container](https://docs.nvidia.com/deeplearning/digits/digits-container-getting-started/index.html)
   - [GitHub getting started](https://github.com/NVIDIA/DIGITS/blob/master/docs/GettingStarted.md)
   - []()

---
### <a name="install" />Install DIGITS

Before running the application, use the ```docker pull``` command
to ensure an up-to-date image is installed.

```shell
sudo docker pull nvidia/digits:latest
# or
sudo docker pull nvcr.io/nvidia/digits:18.04
```

---
### <a name="start" />Start DIGITS

```shell
# Run DIGITS with Nvidia Docker
sudo nvidia-docker run -v /hdd_purple:/data/ -p 5000:5000 nvidia/digits:latest
```

---
### <a name="autostart" />Start DIGITS automatically

Create ```/etc/systemd/system/mydigits.service``` containing:

```shell
[Unit]
Description=Autostart NVIDIA DIGITS
Requires=docker.service
After=docker.service

[Service]
User=pavlenko
Group=docker
RemainAfterExit=true
ExecStart=/bin/sh -c "/usr/bin/nvidia-docker run -v /hdd_purple:/data/ -p 5000:5000 nvidia/digits:latest &"

[Install]
WantedBy=multi-user.target

```

And then run:

```shell
# Start the service and enable autostart after reboot
sudo systemctl daemon-reload
sudo systemctl enable mydigits.service
sudo systemctl start mydigits
systemctl is-active mydigits

# To debug use
#sudo systemctl status mydigits.service
```
