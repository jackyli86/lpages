**Docker 守护进程代理**

- 一键启用
```bash
#!/bin/bash
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf > /dev/null <<EOF
[Service]
Environment="HTTP_PROXY=http://10.10.30.80:7890"
Environment="HTTPS_PROXY=http://10.10.30.80:7890"
Environment="NO_PROXY=localhost,127.0.0.1"
EOF
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart docker

```

- 一键关闭
```bash
#!/bin/bash
sudo rm -f /etc/systemd/system/docker.service.d/http-proxy.conf
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl restart docker

```