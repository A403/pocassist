+ 下载Nginx和最新的releases  
Nginx: http://nginx.org/download/nginx-1.19.10.zip  
releases: https://github.com/jweny/pocassist/releases
+ 解压Nginx并修改配置文件
![image](https://user-images.githubusercontent.com/59276674/118127693-9bc6f480-b42c-11eb-96e1-0ba29ecd6465.png)  
把bulid文件放在和nginx.exe放在同级目录，修改配置如下（仅供参考）：  
```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
	upstream pocassistAPI {
        server 127.0.0.1:1231;
    }
    server {
        listen       8888;
        server_name  localhost;

        location / {
            root build;
            index  index.html index.htm;
        }
		location /api/ {
            proxy_pass http://pocassistAPI/api/;
        }
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```
+ 安装Mysql数据
步骤略过，选择server only,然后一路next
+ 创建并导入数据库
mysql -uroot -ppassword
create databases pocassist;
source D:\pocassist.sql
+ 修改配置文件
```dbConfig:
  host: "127.0.0.1"
  password: "you_mysql_passoword"
  port: "3306"
  user: "root"
  database: "pocassist"
  timeout: "3s"
  ```
  + 启动Nginx
  cd nginx
  start nginx.exe
+ enjoy
