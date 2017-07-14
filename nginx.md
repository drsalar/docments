install pcre

    sudo apt-get update
    sudo apt-get install libpcre3 libpcre3-dev
  
install nginx

    download [nginx](http://nginx.org/en/download.html)
    extract 
    ./configure
    make
    sudo make install
  
make sure nginx installed

    sudo /usr/local/nginx/sbin/nginx -t
    nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
    nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful

show the nginx process

    ps -ef | grep nginx
    root     19512  1654  0 13:29 ?        00:00:00 nginx: master process /usr/local/nginx/sbin/nginx
    nobody   19513 19512  0 13:29 ?        00:00:00 nginx: worker process
    horo     19515  2205  0 13:29 pts/17   00:00:00 grep --color=auto nginx

start nginx

    sudo /usr/local/nginx/sbin/nginx
    
link nginx
 
    type localhost in browser 
    
add third module to nginx

    ./configure --add-module=/path/to/module1/source
    make
    /usr/local/nginx/sbin/nginx -s stop
    cp objs/nginx /usr/local/nginx/sbin/nginx
    /usr/local/nginx/sbin/nginx
