# network
## proxy
sudo nano /etc/environment
```
http_proxy="http://127.0.0.1:7890"
https_proxy="http://127.0.0.1:7890"
ftp_proxy="http://127.0.0.1:7890"
all_proxy="socks5://127.0.0.1:7891"
no_proxy="localhost,127.0.0.1,::1,192.168.0.0/16,localaddress,.localdomain.com"

HTTP_PROXY="http://127.0.0.1:7890"
HTTPS_PROXY="http://127.0.0.1:7890"
FTP_PROXY="http://127.0.0.1:7890"
ALL_PROXY="socks5://127.0.0.1:7891"
NO_PROXY="localhost,127.0.0.1,::1,192.168.0.0/16,localaddress,.localdomain.com"
```
## chrome
--proxy-server=127.0.0.1:7890