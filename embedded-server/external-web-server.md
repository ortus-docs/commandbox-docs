# External Web Server

You can place CommandBox downstream behind another external web server if you wish. Here is an overview of how to do that.

## Microsoft IIS

## Apache HTTP

## Nginx

```text
server {

  server_name  example.net www.example.net;

  root /app;

  index index.cfm index.html index.htm;

  proxy_redirect           off;
  proxy_set_header         X-Real-IP $remote_addr;
  proxy_set_header         X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header         Host $http_host;
  proxy_set_header         X-Forwarded-Proto $scheme;

  location ~ /WEB-INF/  { access_log off; log_not_found off; deny all; }
  location ~ /META-INF/  { access_log off; log_not_found off; deny all; }
  location ~ /META-INF/ { return 404; }
  location ~ /WEB-INF/ { return 404; }
  location ~ \.config$ { return 404; }
  location ~ /\. { return 404; } ## e.g. .htaccess, .gitignore etc.
  location ~ ~$ { return 404; }
  location ~ \.aspx?$ { return 404; } ## most likely hackers testing the site
  location ~ \.php$ { return 404; } 

  # this prevents hidden files (beginning with a period) from being served
  location ~ /\. {
    access_log off; log_not_found off; deny all;
  }

  # Do not log missing favicon.ico errors
  location ^/(favicon\.ico|apple-touch-icon.*\.png)$ { 
    access_log off; log_not_found off; 
  } 
  location = /robots.txt  { 
    access_log off; log_not_found off; 
  }

  location / {
    try_files $uri $uri/ @rewrites;
  }

  #set indexfileinurls=0 in Mura's settings.ini.cfm     
  location @rewrites {
    rewrite ^/(.*)? /index.cfm/$1 last;
    rewrite ^ /index.cfm last;
  }

  # Main Railo/Lucee proxy handler
  location ~ \.(cfm|cfml|cfc|jsp|cfr)(.*)$ {
    proxy_pass http://127.0.0.1:8080;
  }

  # Some basic cache-control for static files to be sent to the browser
  location ~* \.(?:jpg|jpeg|gif|png|ico|gz|svg|svgz|ttf|otf|woff|eot|mp4|ogg|ogv|webm|js|css)$ {
    expires 1M;
    #expires modified +90d;
    access_log off;
    add_header Pragma public;
    add_header Cache-Control "public, must-revalidate, proxy-revalidate";
  }

  listen [::]:443 ssl http2 ipv6only=on; # managed by Certbot
  listen 443 ssl http2; # managed by Certbot
  ssl_certificate /etc/letsencrypt/live/example.net/fullchain.pem; # managed by Certbot
  ssl_certificate_key /etc/letsencrypt/live/example.net/privkey.pem; # managed by Certbot
  include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {

  if ($host ~ ^[^.]+\.example\.net$) {
      return 301 https://$host$request_uri;
  } # managed by Certbot

  if ($host = example.net) {
      return 301 https://$host$request_uri;
  } # managed by Certbot


  listen 80;
  listen [::]:80;
  server_name  example.net www.example.net;
  return 404; # managed by Certbot

}
```

