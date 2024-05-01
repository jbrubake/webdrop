# nginx

## Ubuntu 20.04

```sh
$ sudo apt install nginx
$ sudo systemctl enable --now nginx
$ sudo chmod 660 /var/log/nginx/access.log
$ sudo usermod -aG adm <user>
```

