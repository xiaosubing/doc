



# nginx

## 下载

```shell
wget https://nginx.org/download/nginx-1.20.2.tar.gz
wget https://nginx.org/download/nginx-1.26.2.tar.gz 
```

## 编译安装

编译前先安装编译需要的环境

```shell
dnf install gcc make  pcre-devel  openssl-devel  libxml2-devel libxslt-devel   gd-devel  google-perftools-devel perl-devel
```



```shell
./configure  --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/lib/nginx/tmp/client_body --http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi --http-proxy-temp-path=/var/lib/nginx/tmp/proxy --http-scgi-temp-path=/var/lib/nginx/tmp/scgi --http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi --pid-path=/run/nginx.pid --lock-path=/run/lock/subsys/nginx --user=nginx --group=nginx --with-file-aio --with-ipv6 --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module=dynamic --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_degradation_module --with-http_slice_module --with-http_perl_module=dynamic --with-http_auth_request_module --with-mail=dynamic --with-mail_ssl_module --with-pcre --with-pcre-jit --with-stream=dynamic --with-stream_ssl_module --with-google_perftools_module --with-debug --with-cc-opt='-O2 -g -pipe -Wall -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fstack-protector-strong -grecord-gcc-switches -specs=/usr/lib/rpm/generic-hardened-cc1 -m64 -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fPIC -D_FORTIFY_SOURCE=2 -O2 -Wtrampolines -fsigned-char' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -specs=/usr/lib/rpm/generic-hardened-ld -Wl,-E -Wl,-z,relro -Wl,-z,now -Wl,-z,noexecstack'

# 第二步：
make 

# 第三布
make install 

```

提示：

```shell
./configure: error: C compiler cc is not found
# 解决
dnf install gcc -y 
```

提示：

```shell
./configure: error: the HTTP rewrite module requires the PCRE library.
# 解决 
dnf install pcre-devel -y
```

提示：

```shell
./configure: error: SSL modules require the OpenSSL library.
# 解决
dnf install openssl-devel 
```



提示

```shell
./configure: error: the HTTP XSLT module requires the libxml2/libxslt
# 解决
 dnf install libxml2-devel libxslt-devel
```

提示

```shell
./configure: error: the HTTP image filter module requires the GD library.

#解决：
dnf install gd-devel 
```

提示：

```shell
./configure: error: the Google perftools module requires the Google perftools
# 解决 
dnf install google-perftools-devel
```

提示

```shell
../../../../../src/http/modules/perl/ngx_http_perl_module.h:17:10: 致命错误：EXTERN.h：No such file or directory
# 解决
dnf install perl-devel
```



## 平滑升级

第一布

```shell
./configure  --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/lib/nginx/tmp/client_body --http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi --http-proxy-temp-path=/var/lib/nginx/tmp/proxy --http-scgi-temp-path=/var/lib/nginx/tmp/scgi --http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi --pid-path=/run/nginx.pid --lock-path=/run/lock/subsys/nginx --user=nginx --group=nginx --with-file-aio --with-ipv6 --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module=dynamic --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_degradation_module --with-http_slice_module --with-http_perl_module=dynamic --with-http_auth_request_module --with-mail=dynamic --with-mail_ssl_module --with-pcre --with-pcre-jit --with-stream=dynamic --with-stream_ssl_module --with-google_perftools_module --with-debug --with-cc-opt='-O2 -g -pipe -Wall -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fstack-protector-strong -grecord-gcc-switches -specs=/usr/lib/rpm/generic-hardened-cc1 -m64 -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fPIC -D_FORTIFY_SOURCE=2 -O2 -Wtrampolines -fsigned-char' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -specs=/usr/lib/rpm/generic-hardened-ld -Wl,-E -Wl,-z,relro -Wl,-z,now -Wl,-z,noexecstack'
```

第二步

```shell
make
```

第三步

```shell
make upgrade 
# 输出
[root@localhost new]# make upgrade
/usr/sbin/nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
kill -USR2 `cat /run/nginx.pid`
sleep 1
test -f /run/nginx.pid.oldbin
make: *** [Makefile:22：upgrade] 错误 1

# 然后复制输出，粘贴执行
cp -a /usr/sbin/nginx /usr/sbin/nginxbak
cp objs/nginx /usr/sbin/nginx
然后粘贴回车

```



## 认证

1、修改nginx.conf配置

```shell
auth_basic "提示输入验证的相关提示词";
auth_basic_user_file "/usr/local/nginx/passwd";
```

然后设置密码

```shell
dnf install httpd-tools -y 
```

安装完成后使用`htpasswd`命令进行设置密码

```shell
htpasswd -c -d /usr/local/nginx/passwd  user
```



## 虚拟机主机

虚拟主机一种有三种：

* 基于域名
* 基于端口
* 基于IP



基于域名， 相同的端口根据不同的域名跳转到不同的网站。

```shell
server {
	listen 80;
	server_name 域名;
	location / {
		root  html;
		index index.html index.htm;
	}
}

server {
	listen 80;
	server_name 域名2;
	location / {
		root 域名2;
		index index.html index.htm;
	}
}
```

基于端口，相同的域名根据不同的端口号跳转到不同的网站

```shell
server {
	listen 80;
	server_name 域名;
	localtion / {
		root html;
		index index.html;
	}
}

server {
	listen 90;
	server_name 域名;
	location / {
		root 90端口;
		index index.html;
	}
}
```

基于ip，

```shell
server {
	listen 1xx.xxx.xxx.xxx:80;
	location / {
	
	}
}
	
server {
	listen 1xx.xxx.xxx.xxx:80;
	location / {
	
	}
}
```





## nginx解析php

```shell
dnf install php -y 
# 如果没有安装php-fpm的话
dnf install php-fpm -y 
```

新建一个index.php

```shell
vi /usr/share/nginx/html/index.php
<?php
        echo "你好 Nginx + php";
?>
```

默认的php-fpm是socket的方式

```shell
vi /etc/php-fpm.d/www.conf
# 修改为127.0.0.1:9000
;listen = /run/php-fpm/www.sock
listen = 127.0.0.1:9000
```

修改nginx配置

```shell
# 这个在/etc/nginx/nginx.conf.default里面是有的，推荐奖/script这个修改为$document_root
        
        location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
```

然后重启服务

```shell
systemctl restart nginx 
systemctl restart php-fpm
```

测试

```shell
curl http://127.0.0.1/
你好 Nginx + php
```



## 配置https















