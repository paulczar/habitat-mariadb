# MariaDB habitat build

## Running in docker

First lets hab export it:

```
$ hab pkg export docker paulczar/mariadb
hab pkg export docker paulczar/mariadb
   hab-studio: Creating Studio at /tmp/hab-pkg-dockerize-ecRq/rootfs (baseimage)
> Using local package for paulczar/mariadb
> Using local package for core/bash/4.3.42/20160612075613 via paulczar/mariadb
...
...
...
Sending build context to Docker daemon 465.1 MB
Step 1 : FROM scratch
 ---> 
Step 2 : ENV export PATH=/hab/pkgs/paulczar/mariadb/10.1.14/20160704143920/bin:/hab/pkgs/core/bash/4.3.42/20160612075613/bin:/hab/pkgs/core/glibc/2.22/20160612063629/bin:/hab/pkgs/core/ncurses/6.0/20160612075116/bin:/hab/pkgs/paulczar/jemalloc/4.2.1/20160618180729/bin:/hab/pkgs/core/hab-sup/0.7.0/20160614232939/bin:/hab/pkgs/core/busybox-static/1.24.2/20160612081725/bin:/hab/pkgs/core/bzip2/1.0.6/20160612075040/bin:/hab/pkgs/core/glibc/2.22/20160612063629/bin:/hab/pkgs/core/openssl/1.0.2h/20160612081127/bin:/hab/pkgs/core/xz/5.2.2/20160612080402/bin:/hab/pkgs/core/busybox-static/1.24.2/20160612081725/bin:/hab/bin
 ---> Running in bdc8df7fb90e
 ---> 7109dbdd42a2
...
...
Removing intermediate container 27ae8abed3c9
Successfully built 5f174b248970

```

Now lets run it and watch it start up:

```
$ docker run -d --name db paulczar/mariadb
$ docker logs -f db
hab-sup(MN): Starting paulczar/mariadb
hab-sup(GS): Supervisor 172.17.0.5: 56460c5a-c702-4044-bfb0-89b5740f8943
...
...
mariadb(O): 2016-07-04 15:08:33 140630934144768 [Note] InnoDB: Dumping buffer pool(s) not yet started
mariadb(O): 2016-07-04 15:08:33 140631848871872 [Note] Server socket created on IP: '::'.
mariadb(O): 2016-07-04 15:08:33 140631848871872 [Note] Reading of all Master_info entries succeded
mariadb(O): 2016-07-04 15:08:33 140631848871872 [Note] Added new Master_info '' to hash table
mariadb(O): 2016-07-04 15:08:33 140631848871872 [Note] /hab/pkgs/paulczar/mariadb/10.1.14/20160704143920/bin/mysqld: ready for connections.
mariadb(O): Version: '10.1.14-MariaDB'  socket: '/hab/svc/mariadb/data/run/mysqld.sock'  port: 3306  MariaDB Server
```

and lets test that it works:

```
$ docker inspect db | grep IPAddress
            "IPAddress": "172.17.0.5",
$ docker run -ti paulczar/mariadb bash
hab-sup(SH): Starting your bashlike shell; enjoy!
/ # mysql -h 172.17.0.5 -uhabitat -phabitat -e "select now()"
+---------------------+
| now()               |
+---------------------+
| 2016-07-04 15:14:41 |
+---------------------+
```
