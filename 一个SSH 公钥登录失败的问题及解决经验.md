



`authorized_keys`的权限必须是600

```
[hadoop@master .ssh]$ ll
total 16
-rw-rw-r--. 1 hadoop hadoop  790 Oct 10 06:08 authorized_keys
-rw-------. 1 hadoop hadoop 1675 Oct 10 05:59 id_rsa
-rw-r--r--. 1 hadoop hadoop  395 Oct 10 05:59 id_rsa.pub
-rw-r--r--. 1 hadoop hadoop 1183 Oct  5 01:47 known_hosts
[hadoop@master .ssh]$ chmod 600 authorized_keys 
[hadoop@master .ssh]$ ll
total 16
-rw-------. 1 hadoop hadoop  790 Oct 10 06:08 authorized_keys
-rw-------. 1 hadoop hadoop 1675 Oct 10 05:59 id_rsa
-rw-r--r--. 1 hadoop hadoop  395 Oct 10 05:59 id_rsa.pub
-rw-r--r--. 1 hadoop hadoop 1183 Oct  5 01:47 known_hosts
[hadoop@master .ssh]$ ssh master
Last login: Tue Oct 10 06:07:26 2017 from master
```

