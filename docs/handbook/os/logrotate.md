# logrotate

**1）配置文件介绍**

Linux系统默认安装logrotate工具，它默认的配置文件在： 

> /etc/logrotate.conf
> /etc/logrotate.d/ 

logrotate.conf 才主要的配置文件，logrotate.d 是一个目录，该目录里的所有文件都会被主动的读入/etc/logrotate.conf中执行。 

另外，如果 /etc/logrotate.d/ 里面的文件中没有设定一些细节，则会以/etc/logrotate.conf这个文件的设定来作为默认值。 

Logrotate是基于CRON来运行的，其脚本是/etc/cron.daily/logrotate，日志轮转是系统自动完成的。实际运行时，Logrotate会调用配置文件/etc/logrotate.conf。可以在/etc/logrotate.d目录里放置自定义好的配置文件，用来覆盖Logrotate的缺省值。 

 cat /etc/cron.daily/logrotate #!/bin/sh  /usr/sbin/logrotate /etc/logrotate.conf >/dev/null 2>&1 EXITVALUE=?if [ EXITVALUE != 0 ]; then     /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]" fiexit 0



如果等不及cron自动执行日志轮转，想手动强制切割日志，需要加-f参数；不过正式执行前最好通过Debug选项来验证一下（-d参数），这对调试也很重要： 

 /usr/sbin/logrotate -f /etc/logrotate.d/nginx

 /usr/sbin/logrotate -d -f /etc/logrotate.d/nginx

**logrotate 命令格式：**

> logrotate [OPTION…] <configfile> 
>
> -d, –debug ：debug模式，测试配置文件是否有错误。
> -f, –force ：强制转储文件。
> -m, –mail=command ：压缩日志后，发送日志到指定邮箱。
> -s, –state=statefile ：使用指定的状态文件。
> -v, –verbose ：显示转储过程。 

根据日志切割设置进行操作，并显示详细信息： 

/usr/sbin/logrotate -v /etc/logrotate.conf 

/usr/sbin/logrotate -v /etc/logrotate.d/php 

根据日志切割设置进行执行，并显示详细信息,但是不进行具体操作，debug模式 

 /usr/sbin/logrotate -d /etc/logrotate.conf 

 /usr/sbin/logrotate -d /etc/logrotate.d/nginx 

查看各log文件的具体执行情况 

 cat /var/lib/logrotate.status 

**2）切割介绍**

比如以系统日志/var/log/message做切割来简单说明下： 

- 第一次执行完rotate(轮转)之后，原本的messages会变成messages.1，而且会制造一个空的messages给系统来储存日志；
- 第二次执行之后，messages.1会变成messages.2，而messages会变成messages.1，又造成一个空的messages来储存日志！

如果仅设定保留三个日志（即轮转3次）的话，那么执行第三次时，则 messages.3这个档案就会被删除，并由后面的较新的保存日志所取代！也就是会保存最新的几个日志。 

日志究竟轮换几次，这个是根据配置文件中的dateext 参数来判定的。 

看下logrotate.conf配置： 



 cat /etc/logrotate.conf# 底下的设定是 "logrotate 的默认值" ，如果別的文件设定了其他的值，# 就会以其它文件的设定为主weekly          //默认每一周执行一次rotate轮转工作 rotate 4       //保留多少个日志文件(轮转几次).默认保留四个.就是指定日志文件删除之前轮转的次数，0 指没有备份 create         //自动创建新的日志文件，新的日志文件具有和原来的文件相同的权限；因为日志被改名,因此要创建一个新的来继续存储之前的日志 dateext       //这个参数很重要！就是切割后的日志文件以当前日期为格式结尾，如xxx.log-20131216这样,如果注释掉,切割出来是按数字递增,即前面说的 xxx.log-1这种格式 compress      //是否通过gzip压缩转储以后的日志文件，如xxx.log-20131216.gz ；如果不需要压缩，注释掉就行   include /etc/logrotate.d # 将 /etc/logrotate.d/ 目录中的所有文件都加载进来  /var/log/wtmp {                 //仅针对 /var/log/wtmp 所设定的参数 monthly                    //每月一次切割,取代默认的一周 minsize 1M              //文件大小超过 1M 后才会切割 create 0664 root utmp            //指定新建的日志文件权限以及所属用户和组 rotate 1                    //只保留一个日志. }# 这个 wtmp 可记录用户登录系统及系统重启的时间# 因为有 minsize 的参数，因此不见得每个月一定会执行一次喔.要看文件大小。

由这个文件的设定可以知道/etc/logrotate.d其实就是由/etc/logrotate.conf 所规划出来的目录，虽然可以将所有的配置都写入 /etc/logrotate.conf ，但是这样一来这个文件就实在是太复杂了，尤其是当使用很多的服务在系统上面时， 每个服务都要去修改 /etc/logrotate.conf 的设定也似乎不太合理了。 

所以，如果独立出来一个目录，那么每个要切割日志的服务， 就可以独自成为一个文件，并且放置到 /etc/logrotate.d/ 当中。 

其他重要参数说明： 

compress                                   通过gzip 压缩转储以后的日志 nocompress                                不做gzip压缩处理 copytruncate                              用于还在打开中的日志文件，把当前日志备份并截断；是先拷贝再清空的方式，拷贝和清空之间有一个时间差，可能会丢失部分日志数据。 nocopytruncate                           备份日志文件不过不截断 create mode owner group             轮转时指定创建新文件的属性，如create 0777 nobody nobody nocreate                                    不建立新的日志文件 delaycompress                           和compress 一起使用时，转储的日志文件到下一次转储时才压缩 nodelaycompress                        覆盖 delaycompress 选项，转储同时压缩。 missingok                                 如果日志丢失，不报错继续滚动下一个日志 errors address                           专储时的错误信息发送到指定的Email 地址 ifempty                                    即使日志文件为空文件也做轮转，这个是logrotate的缺省选项。 notifempty                               当日志文件为空时，不进行轮转 mail address                             把转储的日志文件发送到指定的E-mail 地址 nomail                                     转储时不发送日志文件 olddir directory                         转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统 noolddir                                   转储后的日志文件和当前日志文件放在同一个目录下 sharedscripts                           运行postrotate脚本，作用是在所有日志都轮转后统一执行一次脚本。如果没有配置这个，那么每个日志轮转后都会执行一次脚本 prerotate                                 在logrotate转储之前需要执行的指令，例如修改文件的属性等动作；必须独立成行 postrotate                               在logrotate转储之后需要执行的指令，例如重新启动 (kill -HUP) 某个服务！必须独立成行 daily                                       指定转储周期为每天 weekly                                    指定转储周期为每周 monthly                                  指定转储周期为每月 rotate count                            指定日志文件删除之前转储的次数，0 指没有备份，5 指保留5 个备份 dateext                                  使用当期日期作为命名格式 dateformat .%s                       配合dateext使用，紧跟在下一行出现，定义文件切割后的文件名，必须配合dateext使用，只支持 %Y %m %d %s 这四个参数 size(或minsize) log-size            当日志文件到达指定的大小时才转储，log-size能指定bytes(缺省)及KB (sizek)或MB(sizem).   当日志文件 >= log-size 的时候就转储。 以下为合法格式：（其他格式的单位大小写没有试过） size = 5 或 size 5 （>= 5 个字节就转储） size = 100k 或 size 100k size = 100M 或 size 100M 

小示例：下面一个切割nginx日志的配置 

 vim /etc/logrotate.d/nginx /usr/local/nginx/logs/*.log { dailyrotate 7 missingoknotifemptydateextsharedscriptspostrotate    if [ -f /usr/local/nginx/logs/nginx.pid ]; then         kill -USR1 cat /usr/local/nginx/logs/nginx.pid     fiendscript}

## touch命令 

touch命令就是用来修改文件访问与修改时间的。 

例1：如果文件abc不存在，则创建一个访问与修改时间为当前时间的空文件abc，如果存在，只修改时间。 

touch abc

例2：修改文件的访问与修改时间为当前时间，如果文件不存在，那么不创建文件 

touch -c abc f2

例3：只更新文件的修改时间 

touch -m abc

例4：只更新文件的访问时间 

touch -a abc

例5：指定时间更新 

格式：[[CC]YY]MMDDhhmm[.ss] 

touch -t  201405291752 abc

touch -m -t  201405291752 abc

touch -a -t  201405291752 abc

touch -t  201405291752.14 abc

touch -t  1405291752.14 abc

touch -t  05291752 abc

例6：参考其它文件(aaa)更新abc 

touch -r aaa abc

touch -m -r aaa abc

touch -a -r aaa abc

## 验证修改结果 

```bash
 # stat abc
  File: "abc"
  Size: 0               Blocks: 0          IO Block: 4096   普通空文件
Device: 802h/2050d      Inode: 410262      Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2014-05-29 17:52:00.000000000 +0800
Modify: 2014-05-29 17:52:00.000000000 +0800
Change: 2014-05-29 17:54:18.076993319 +0800
Access：访问时间
Modify：最后修改时间
Change：状态修改时间，比如权限等
```

