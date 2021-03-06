


cd /usr/local/src

wget http://192.168.1.22/nginx/nginx-1.4.7.tar.gz 
wget http://192.168.1.22/nginx/nginx-mogilefs-module-1.0.5.tar.gz
wget http://192.168.1.22/nginx/nginx-requestkey-module-1.0.tar.gz
wget http://192.168.1.22/nginx/pcre-8.33.tar.gz

yum install -y zlib.x86_64 zlib-devel.x86_64 gzip.x86_64 pcre.x86_64 pcre-devel.x86_64

useradd -M nginx 
mkdir -p /data/logs/nginx

vim src/core/nginx.h
#define NGINX_VERSION "1.0_1.4.7"
#define NGINX_VER "UPLUS_SERVER/" NGINX_VERSION
#define NGINX_VAR "UPLUS_SERVER"
#define NGX_OLDPID_EXT ".oldbin"

vim src/http/ngx_http_header_filter_module.c
static char ngx_http_server_string[] = "Server: UPLUS_SERVER" CRLF;

./configure \
--prefix=/usr/local/nginx \
--user=nginx \
--group=nginx \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_stub_status_module 

make && make install


=======

yum install -y  binutils make cmake vim gcc gcc-devel
yum install -y pcre-devel.x86_64 pcre.x86_64
yum install -y openssl-devel.x86_64 openssl.x86_64
yum install -y gd-devel.x86_64 gd.x86_64
yum install -y GeoIP-devel.x86_64 GeoIP.x86_64
yum install -y libxml2-devel.x86_64 libxml2.x86_64 libxslt-devel.x86_64 libxslt.x86_64

vim src/core/nginx.h
...
#define nginx_version      1006002
#define NGINX_VERSION      "1.1_1.6.2"
#define NGINX_VER          "UPLUS_SERVER/" NGINX_VERSION

#define NGINX_VAR          "UPLUSE_SERVER"
#define NGX_OLDPID_EXT     ".oldbin"
...

vim src/http/ngx_http_header_filter_module.c
...
static char ngx_http_server_string[] = "Server: UPLUS_SERVER" CRLF;
...

cat > /etc/profile.d/nginx-env.sh <<EOF
export NGINX_HOME=/usr/local/nginx
export PATH=\${NGINX_HOME}/sbin:\$PATH
EOF
. /etc/profile
echo $PATH
which nginx

UPLUS_SERVER/1.1_1.6.2

./configure --prefix=/usr/local/nginx  --user=nginx --group=nginx --conf-path=/usr/local/nginx/conf/nginx.conf --error-log-path=/usr/local/nginx/logs/error.log --http-client-body-temp-path=/usr/local/nginx/body --http-fastcgi-temp-path=/usr/local/nginx/fastcgi --http-log-path=/usr/local/nginx/logs/access.log --http-proxy-temp-path=/usr/local/nginx/proxy --http-scgi-temp-path=/usr/local/nginx/scgi --http-uwsgi-temp-path=/usr/local/nginx/uwsgi --lock-path=/var/run/nginx.lock --pid-path=/var/run/nginx.pid --with-debug --with-http_addition_module --with-http_dav_module --with-http_geoip_module --with-http_gzip_static_module --with-http_image_filter_module --with-http_realip_module --with-http_stub_status_module --with-http_ssl_module --with-http_sub_module --with-http_xslt_module --with-ipv6 --with-sha1=/usr/include/openssl --with-md5=/usr/include/openssl --with-mail --with-mail_ssl_module --with-http_gunzip_module 

make && make install


./configure \
--prefix=/usr/local/nginx \
--user=nginx \
--group=nginx \
--with-http_ssl_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_stub_status_module \
--with-http_auth_request_module \
--with-mail \
--with-mail_ssl_module \
--with-file-aio \
--with-ipv6 \
--with-http_spdy_module \
--with-cc-opt='-O2 -g -pipe -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector \
--param=ssp-buffer-size=4 -m64 -mtune=generic'

# add-module nginx-rtmp-module
./configure \
--prefix=/usr/local/nginx \
--user=nginx \
--group=nginx \
--with-http_ssl_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_stub_status_module \
--with-http_auth_request_module \
--with-mail \
--with-mail_ssl_module \
--with-file-aio \
--with-ipv6 \
--with-http_spdy_module \
--with-cc-opt='-O2 -g -pipe -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector \
--param=ssp-buffer-size=4 -m64 -mtune=generic' \
--add-module=../nginx-rtmp-module-1.1.7 \
--with-pcre=../pcre-8.33

================================================================================

./configure  \
--prefix=/usr/local/nginx  \
--user=nginx  \
--group=nginx  \
--with-http_ssl_module  \
--with-http_realip_module  \
--with-http_addition_module  \
--with-http_sub_module  \
--with-http_dav_module  \
--with-http_flv_module  \
--with-http_mp4_module  \
--with-http_gunzip_module  \
--with-http_gzip_static_module  \
--with-http_random_index_module  \
--with-http_secure_link_module  \
--with-http_stub_status_module  \
--with-http_auth_request_module  \
--with-mail  \
--with-mail_ssl_module  \
--with-file-aio  \
--with-ipv6  \
--with-http_spdy_module  \
--with-cc-opt='-O2 -g -pipe -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic'  \
--with-http_image_filter_module \
--with-pcre=../pcre-8.36  \
--add-module=../echo-nginx-module-0.58 \
--add-module=../nginx-rtmp-module-1.1.7  \
--add-module=../ngx_cache_purge-2.3 \
--add-module=../ngx_devel_kit-0.2.19 \
--add-module=../lua-nginx-module-0.10.0 \
--add-module=../ngx_pagespeed-release-1.9.32.11-beta  \
--add-module=../redis2-nginx-module-0.12 \
--add-module=../sqlite-http-basic-auth-nginx-module-master 


make && make install

#(?????????)--add-module=../ngx_image_thumb \
#(?????????)--add-module=../nginx-requestkey-module \

wget http://uplus.file.youja.cn/nginx/pcre-8.36.tar.gz

--------------------------

--with-http_image_filter_module # images_filter
yum install gd-devel # gcc automake autoconf m4
apt-get install libgd2-xpm libgd2-xpm-dev # build-essential m4 autoconf automake make libcurl-dev libgd2-dev libpcre-dev 

conf: 
image_filter test; #?????????????????????
image_filter rotate 90|180|270; #????????????
image_filter size; #???????????????ID3???????????????
resize [width] [height]; #???????????????
image_filter crop [width] [height]; #??????????????????????????????
image_filter_buffer; #??????????????????????????????????????????1M
image_filter_jpeg_quality; #??????jpeg???????????????????????????
image_filter_transparency; #????????????gif???palette-based???png???????????????????????????????????????????????????

--------------------------

--add-module=../ngx_pagespeed-release-1.10.33.1-beta  \ 
# google optimze image
cd /usr/local/src
NPS_VERSION=1.10.33.1
wget https://github.com/pagespeed/ngx_pagespeed/archive/release-${NPS_VERSION}-beta.zip -O release-${NPS_VERSION}-beta.zip
unzip release-${NPS_VERSION}-beta.zip
cd ngx_pagespeed-release-${NPS_VERSION}-beta/
wget https://dl.google.com/dl/page-speed/psol/${NPS_VERSION}.tar.gz
tar -xzvf ${NPS_VERSION}.tar.gz

--------------------------

--add-module=../ngx_image_thumb # ngx_image_thumb??????????????????
image on/off ???????????????????????????,????????????
image_backend on/off ???????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
image_backend_server ?????????????????????
image_output on/off ????????????????????????????????????????????? ??????off
image_jpeg_quality 75 ??????JPEG??????????????? ?????????75
image_water on/off ????????????????????????
image_water_type 0/1 ???????????? 0:???????????? 1:????????????
image_water_min 300 300 ???????????? 300 ?????? 300 ????????????????????????
image_water_pos 0-9 ???????????? ?????????9 0???????????????,1???????????????,2???????????????,3???????????????,4???????????????,5???????????????,6???????????????,7???????????????,8???????????????,9???????????????
image_water_file ????????????(jpg/png/gif),?????????????????????????????????????????????
image_water_transparent ???????????????,??????20
image_water_text ???????????? "Power By Vampire"
image_water_font_size ???????????? ?????? 5
image_water_font ??????????????????????????????
image_water_color ??????????????????,?????? #000000

--------------------------

--add-module=../nginx-rtmp-module-1.1.7  \ # rtmp
#RTMP
wget http://uplus.file.youja.cn/nginx/nginx-rtmp-module-1.1.7.tar.gz


--------------------------

--add-module=../ngx_cache_purge
#??????????????????
git clone https://github.com/FRiCKLE/ngx_cache_purge.git

proxy_cache_path /dev/shm levels=1:2 keys_zone=jcache:128m inactive=1d max_size=256m;  
server {
    listen      80;

    location ~* ^/purge(/\S+)$ {
        proxy_cache_purge jcache $1;
    }

    location ~* .*\.(jpg|png|gif|css|js)$ {
        proxy_cache jcache;
        proxy_cache_valid 200 302 30m;
        proxy_cache_key $uri;
        proxy_set_header Host "www.baidu.com";
        proxy_pass http://www.baidu.com;
    }

    location / {
        proxy_set_header Host "www.youja.cn";
        proxy_pass http://www.baidu.com;
    }
}

http://domain
http://domain/purge/test.png

--------------------------

--add-module=../nginx-requestkey-module
#key???????????? ??????????????????????????????????????????????????????module?????????
https://github.com/miyanaga/nginx-requestkey-module.git

???????????????????????????????????????Nginx-accesskey-2.03.tar.gz 
????????? ??????conf???????????????$HTTP_ACCESSKEY_MODULE????????????"ngx_http_accesskey_module",????????????nginx;
accesskey
??????: accesskey [on|off]
??????: accesskey off
????????????: main, server, location
?????? access-key ?????????
accesskey_arg
??????: accesskey_arg "??????"
??????: accesskey "key"
????????????: main, server, location
URL????????? access key ???GET?????????
accesskey_hashmethod
??????: accesskey_hashmethod [md5|sha1]
??????: accesskey_hashmethod md5???????????? md5 ?????????
????????????: main, server, location
??? MD5 ?????? SHA1 ?????? access key???
accesskey_signature
??????: accesskey_signature "??????"
??????: accesskey_signature "$remote_addr"
?????????: main, server, location

location / {
   default_type text/plain; 
   accesskey             on;
   accesskey_hashmethod  md5;
   accesskey_arg         "key";
   accesskey_signature   "jesse$uri";
   return 200 "OK";
}

--------------------------

--add-module=../redis2-nginx-module-0.12
#??????redis
https://github.com/openresty/redis2-nginx-module.git

upstream redis_pool{
    server 127.0.0.1:6379;
}

server {
    listen      80;

    location ~* "^/(\w+)$" {
        redis2_query $1;
        redis2_pass redis_pool;
    }

    location ~* "^/(\w+)/(\w+)$" {
        redis2_query $1 $2;
        redis2_pass redis_pool;
    }

    location ~* "^/(\w+)/(\w+)/(\w+)$" {
        redis2_query $1 $2 $3;
        redis2_pass redis_pool;
    }

    location ~* "^/(\w+)/(\w+)/(\w+)/(\w+)$" {
        redis2_query $1 $2 $3 $4;
        redis2_pass redis_pool;
    }
    location ~* "^/(\w+)/(\w+)/(\w+)/(\w+)/(\w+)$" {
        redis2_query $1 $2 $3 $4 $5;
        redis2_pass redis_pool;
    }

    location / {
        default_type text/plain;
        return 200 "Not found $uri";
    }
}

curl http://domain/set/one/first
curl http://domain/get/one

--------------------------

--add-module=../sqlite-http-basic-auth-nginx-module-master
https://github.com/kunal/sqlite-http-basic-auth-nginx-module.git
#sqlite??????
location / {
        auth_sqlite_basic "Restricted Sqlite";
        auth_sqlite_basic_database_file  /dev/shm/sqlite3.db; # Sqlite DB file
        auth_sqlite_basic_database_table  auth_table; # Sqlite DB table
        auth_sqlite_basic_table_user_column  user; # User column
        auth_sqlite_basic_table_passwd_column  password; # Password column
   }

--------------------------


Nginx + Lua

wget http://luajit.org/download/LuaJIT-2.0.4.tar.gz
tar xvzf LuaJIT-2.0.4.tar.gz
cd LuaJIT-2.0.4
make && make install

cat > /etc/profile.d/lua-env.sh << EOF
export LUAJIT_LIB=/usr/local/lib
export LUAJIT_INC=/usr/local/include/luajit-2.0

EOF

wget -O lua-nginx-module-0.10.0.tar.gz https://codeload.github.com/openresty/lua-nginx-module/tar.gz/v0.10.0
wget -O ngx_devel_kit-0.2.19.tar.gz https://codeload.github.com/simpl/ngx_devel_kit/tar.gz/v0.2.19

tar xvzf lua-nginx-module-0.10.0.tar.gz
tar xvzf ngx_devel_kit-0.2.19.tar.gz 

--add-module=../ngx_devel_kit-0.2.19 \
--add-module=../lua-nginx-module-0.10.0

cannot open shared object file: No such file or directory  ????????????

ldd /usr/local/nginx/sbin/nginx

echo "/usr/local/lib" > /etc/ld.so.conf.d/usr_local_lib.conf

location /lua {
    set $test "hello, world.";
    content_by_lua '
        ngx.header.content_type = "text/plain";
        ngx.say(ngx.var.test);
    ';
}

--------------------------

echo-nginx-module

--add-module=../echo-nginx-module-0.58

wget -O echo-nginx-module-0.58.tar.gz https://codeload.github.com/openresty/echo-nginx-module/tar.gz/v0.58

location /echo {
    default_type text/plain;
    echo "Hello, jesse";

}

--------------------------

-- 2016-04-26 1.10.0 support stream

./configure --prefix=/usr/local/nginx --user=nginx --group=nginx --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --with-http_auth_request_module --with-mail --with-mail_ssl_module --with-file-aio --with-ipv6 --with-cc-opt='-O2 -g -pipe -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic' --with-pcre=../pcre-8.38 --with-http_image_filter_module --add-module=../nginx-rtmp-module-1.1.7 --add-module=../ngx_cache_purge --with-stream


stream {
    upstream test_tcp {
        server 127.0.0.1:11111 weight=5;
        server 127.0.0.1:11112 max_fails=3 fail_timeout=30s;
    }

    upstream test_udp {
        server 127.0.0.1:11111 weight=5;
        server 127.0.0.1:11112 max_fails=3 fail_timeout=30s;
    }

    server {
       listen 12345;
       proxy_connect_timeout 1s;
       proxy_timeout 3s;
       proxy_responses 1;
       proxy_pass test_tcp;
   }

    server {
       listen 12345 udp;
       proxy_connect_timeout 1s;
       proxy_timeout 3s;
       proxy_responses 1;
       proxy_pass test_tcp;
    }
}

-- 2016-08-30 lua


cat > /etc/profile.d/openresty-env.sh <<EOF
export OPENRESTY_HOME=/usr/local/openresty
export PATH=\${OPENRESTY_HOME}/bin:\${OPENRESTY_HOME}/luajit/bin:\$PATH
EOF


https://openresty.org/download/ngx_openresty-1.9.7.2.tar.gz

https://openresty.org/download/openresty-1.11.2.1.tar.gz





