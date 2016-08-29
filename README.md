# Установка Oracle Databace 12c на Debian Jessie 8.5

## Уставливаем Debian в экспетрном режиме

`/tmp ~ 1 GB`.

1. ssh-сервер
2. стандартные системные утилиты
3. `sudo aptitude install build-essential`
4. `sudo aptitude install libaio1`
5. `sudo aptitude install x11-utils`

## Делаем симлинки
* `cd /bin`
* `sudo ln -s /usr/bin/awk`
* `sudo ln -s /usr/bin/basename`
* `sudo mkdir /usr/lib64`
* `cd /usr/lib64`
* `sudo ln -s /usr/lib/x86_64-linux-gnu/libpthread_nonshared.a`
* `sudo ln -s /usr/lib/x86_64-linux-gnu/libc_nonshared.a`

## Группы и пользователь
* `sudo groupadd -g 2001 oinstall`
* `sudo groupadd -g 2002 dba`
* `sudo groupadd -g 2003 oper`
* `sudo adduser --uid 2004 --gid 2001 oracle`
* `sudo usermod -a -G dba oracle`
* `sudo usermod -a -G oper oracle`

## /etc/sysctl.conf
* `kernel.shmmni = 4096`
* `kernel.shmmax = 4398046511104`
* `kernel.shmall = 1073741824`
* `kernel.sem = 250 32000 100 128`
* `fs.aio-max-nr = 1048576`
* `fs.file-max = 6815744`
* `net.ipv4.ip_local_port_range = 9000 65500`
* `net.core.rmem_default = 262144`
* `net.core.rmem_max = 4194304`
* `net.core.wmem_default = 262144`
* `net.core.wmem_max = 1048586`

`sudo /sbin/sysctl -p`

## /etc/security/limits.conf
* `oracle   soft   nproc    131072`
* `oracle   hard   nproc    131072`
* `oracle   soft   nofile   131072`
* `oracle   hard   nofile   131072`
* `oracle   soft   core     unlimited`
* `oracle   hard   core     unlimited`
* `oracle   soft   memlock  50000000`
* `oracle   hard   memlock  50000000`

## .profile
```
export TMP=/tmp
export TMPDIR=$TMP
 
export ORACLE_HOSTNAME=host.domain
export ORACLE_UNQNAME=orcl
export ORACLE_BASE=/db
export ORACLE_HOME=$ORACLE_BASE/home
export ORACLE_SID=orcl
 
export PATH=$ORACLE_HOME/bin:$PATH
 
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
```

## Каталог базы
* `sudo mkdir -p $ORACLE_HOME`
* `sudo chown -R oracle:oinstall $ORACLE_HOME`

## Установка
* `ssh -X oracle@host.domain`
* Распаковываем архив.
* `cd database`
* `./runInstaller`
* Следуем инструкциям.

## Автозапуск базы
* `/etc/oratab` -> `orcl:/db/home:Y`
* `oracledb` поместить в `/etc/init.d/`
* `sudo update-rc.d defaults`
