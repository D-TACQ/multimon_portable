# multimon_portable
## Web status monitor for a fleet of ACQ400 systems on LAN

### Typical Screenshot:

![Screenshot](https://github.com/D-TACQ/multimon_portable/releases/download/v1.0.0/Screenshot.from.2023-04-07.16-58-31.png "Screenshot")

### A large fleet of units under test earlier:
https://github.com/D-TACQ/multimon_portable/releases/download/v1.0.0/Screenshot.from.2020-08-18.08-59-06.png

Prerequisites:
* python3 packages: flask requests xml2dict
* we assume there is a working DNS
* Multimon uses EPICS beacons to detect new devices, however we wanted to avoid needing to install EPICS on the host, and also to make the appropriate firewall entry, instead:
* Multimon needs to know the name of a "lighthouse" : a first ACQ400 system to get a TCP socket feed of all EPICS beacon data
* for initial testing, just run it, then connect a local web browser to localhost:5000/, also search @@TEST in index.html to reset the URL path



To run as service do:
```
      sudo cp multimon.service /etc/systemd/system/
      sudo systemctl daemon-reload
      sudo systemctl enable multimon
      sudo systemctl start multimon
```
Nginx config:
```
      location /multimon {
        proxy_pass http://127.0.0.1:5000/;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Prefix /multimon;
      }
```
