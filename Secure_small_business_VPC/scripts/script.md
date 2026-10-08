# Start up script

```bash
  #!/bin/bash
  dnf install -y httpd
  sed -i 's/^Listen 80$/Listen 8080/' /etc/httpd/conf/httpd.conf
  echo "<h1>Desert Bloom Dental - Internal App Server</h1>" > /var/www/html/index.html
  systemctl enable --now httpd
```
