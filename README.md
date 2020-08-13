# Hortonworks Data Platform v2.6.5 Related Configuration

_Rev. 2020/08/13_


## Add sandbox-hdp and sandbox-proxy as a systemd service 

+ Create two system daemon service files for sandbox-hdp and sandbox-proxy respectively. 
``` sh
sudo touch /etc/systemd/system/hdp.service
sudo touch /etc/systemd/system/hdp-proxy.service
```

+ Add the following to the hdp.service file. Note sandbox-hdp requires docker service to be enabled and started.
``` sh
[Unit]
Description=HDP Sandbox 2.6.5
After=docker.service
Requires=docker.service

[Service]
Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin"
ExecStart=/usr/bin/docker start -a sandbox-hdp
ExecStop=/usr/bin/docker stop sandbox-hdp

[Install]
WantedBy=multi-user.target
```

+ Add the following to the hdp-proxy.service file. Note that hdp-proxy requires both docker and hdp.service as prerequisites. 
``` sh
[Unit]
Description=HDP Sandbox Proxy
After=docker.service
Requires=docker.service
After=hdp.service
Requires=hdp.service

[Service]
Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin"
ExecStart=/usr/bin/docker start -a sandbox-proxy
ExecStop=/usr/bin/docker stop sandbox-proxy

[Install]
WantedBy=multi-user.target
```

+ Add both of them as systemd services and enable startup boot. 
``` sh
sudo systemctl daemon-reload
sudo systemctl <start|stop|status> hdp
sudo systemctl <start|stop|status> hdp-proxy
# make them automatically start when boot
sudo systemctl enable hdp
sudo systemctl enable hdp-proxy
```
