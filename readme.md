# How to compile brotli modules from source

Download nginx source code, make sure that you download the same version which is present in your server
```sh
wget https://nginx.org/download/nginx-$(nginx -v 2>&1 | cut -d / -f2).tar.gz 
```

Expand the tarball
```sh
tar zvxf nginx-1.19.8.tar.gz
```

Download the source code of Brotli modules

```sh
git clone --recursive https://github.com/google/ngx_brotli.git 
```

Now compile

```sh
cd nginx-1.19.8
sudo ./configure --with-compat --add-dynamic-module=../ngx_brotli
sudo make modules
```

Now copy those brotli modules to `/etc/nginx/modules/` directory 

```sh
cp objs/ngx_http_brotli_filter_module.so /etc/nginx/modules/
cp objs/ngx_http_brotli_static_module.so /etc/nginx//modules/
```

Now load them in `/etc/nginx/nginx.conf/' - add the following two lines at the top of your nginx configuration

```sh
load_module "modules/ngx_http_brotli_filter_module.so";
load_module "modules/ngx_http_brotli_static_module.so";
```

# How to use the precompiled brotli modules from this repo?
You can download the precompiled brotli modules from this repo. If you are running nginx-1.19.8, then download the brotli modules from that folder. Match the version number of nginx that is installed in your server, that's all 
Now copy those brotli modules to `/etc/nginx/modules/` directory 

```sh
cp ngx_http_brotli_filter_module.so /etc/nginx/modules/
cp ngx_http_brotli_static_module.so /etc/nginx//modules/
```

Now load them in `/etc/nginx/nginx.conf/' - add the following two lines at the top of your nginx configuration

```
load_module "modules/ngx_http_brotli_filter_module.so";
load_module "modules/ngx_http_brotli_static_module.so";
```

# Enable support for brotli

And add the following highlighted lines at the end of the http {} section in the configuration file

```
http { 
  ## 
  # Basic Settings 
  ## 
  . . . . 
  brotli on; 
  brotli_comp_level 6; 
  brotli_static on; 
  brotli_types application/atom+xml application/javascript application/json application/rss+xml 
  application/vnd.ms-fontobject 
  application/x-font-opentype application/x-font-truetype application/x-font-ttf application/x-javascript 
  application/xhtml+xml application/xml 
  font/eot font/opentype font/otf font/truetype 
  image/svg+xml image/vnd.microsoft.icon 
  image/x-icon image/x-win-bitmap text/css 
  text/javascript text/plain text/xml; 
}
```

Finally, restart Nginx 

```sh
sudo systemctl restart nginx
```

# Troubleshooting
If you are compiling brotli modules on your own server, during the confiruation step you may get the error that PCRE is missing. If so, just install it like this 

```sh
sudo apt install libpcre3 libpcre3-dev
```

If ZLib is missing, install it like this

```sh
sudo apt install zlib1g zlib1g-dev
```
