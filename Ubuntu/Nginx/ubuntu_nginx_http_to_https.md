## Ubuntu Nginx http to https

* 80 Port에 대한 Server 설정과 443 Port에 대한 Server 설정을 분리
* 80 Port는 301 Redirect 설정

### 1. listen 80 port

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        return 301 https://$host$request_uri;
}
```

---

### 2. listen 443 port

```
server {
        listen 443 default_server;
        listen [::]:443 default_server;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name [Your Domain];

        location / {
        	proxy_pass		http://127.0.0.1:8080;
			...
        }
}

```

