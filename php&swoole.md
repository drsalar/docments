# mac 下 php 使用 swoole

## 安装 pecl

```
$ curl -O https://pear.php.net/go-pear.phar
$ sudo php -d detect_unicode=0 go-pear.phar
```

安装过程

```
Below is a suggested file layout for your new PEAR installation.  To
change individual locations, type the number in front of the
directory.  Type 'all' to change all of them or simply press Enter to
accept these locations.

 1. Installation base ($prefix)                   : /usr
 2. Temporary directory for processing            : /tmp/pear/install
 3. Temporary directory for downloads             : /tmp/pear/install
 4. Binaries directory                            : /usr/bin
 5. PHP code directory ($php_dir)                 : /usr/share/pear
 6. Documentation directory                       : /usr/docs
 7. Data directory                                : /usr/data
 8. User-modifiable configuration files directory : /usr/cfg
 9. Public Web Files directory                    : /usr/www
10. System manual pages directory                 : /usr/man
11. Tests directory                               : /usr/tests
12. Name of configuration file                    : /private/etc/pear.conf

1-12, 'all' or Enter to continue: 1
```

输入 1，将安装根目录修改为 /usr/local/pear；

输入 4，将命令安装到 /usr/local/bin 目录；


## 安装 swoole 扩展

mac 自带的 php 在安装 swoole 的时候会出现错误:

```
WARNING: channel "pecl.php.net" has updated its protocols, use "pecl channel-update pecl.php.net" to update
downloading swoole-4.5.4.tgz ...
Starting to download swoole-4.5.4.tgz (1,563,795 bytes)
.....................................................................................................................................................................................................................................................................................................................done: 1,563,795 bytes
379 source files, building
running: phpize
grep: /usr/include/php/main/php.h: No such file or directory
grep: /usr/include/php/Zend/zend_modules.h: No such file or directory
grep: /usr/include/php/Zend/zend_extensions.h: No such file or directory
Configuring for:
PHP Api Version:
Zend Module Api No:
Zend Extension Api No:
Cannot find autoconf. Please check your autoconf installation and the
$PHP_AUTOCONF environment variable. Then, rerun this script.

ERROR: `phpize' failed
```

**解决办法**

1. 关闭 sip: 重启 mac ，按住 command+R 进入安全模式，打开终端输入

```
csrutil disable
```

2. 重启后重新挂载磁盘

```
sudo mount -uw /
```

3. 然后 将 include 目录链接到 /usr 下

```
sudo ln -s /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/usr/include /usr/include
```
4. 可以通过 sudo pecl install swoole 或者下载源码安装
5. 更新 php-ini
   
可能会遇到 

```
file included from /private/tmp/pear/install/swoole/php_swoole_cxx.h:19:
/private/tmp/pear/install/swoole/php_swoole.h:112:2: error: "Enable openssl support, require openssl library"
#error "Enable openssl support, require openssl library"
 ^
1 error generated.
make: *** [php_swoole.lo] Error 1
ERROR: `make' failed
```

在 pecl 安装过程中加上 openssl 库目录地址，或者修改源码 make.sh，在 ./configure 后加上目录地址
 
```
enable sockets supports? [no] : yes
enable openssl support? [no] : yes --with-openssl-dir=/usr/local/Cellar/openssl@1.1/1.1.1g/
enable http2 support? [no] : yes
enable mysqlnd support? [no] : yes
building in /private/tmp/pear/install/pear-build-rootz6Jg9v/swoole-4.5.4
```