rpm -ivh mpfr-2.4.1-6.el6.x86_64.rpm 

rpm -ivh ppl-0.10.2-11.el6.x86_64.rpm 

 rpm -ivh cpp-4.4.7-4.el6.x86_64.rpm 

rpm -ivh cloog-ppl-0.15.7-1.2.el6.x86_64.rpm 

 rpm -ivh libmpdclient2-2.1-1.el5.rf.x86_64.rpm 

rpm -ivh libgomp-4.4.7-4.el6.x86_64.rpm

 rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm 

gcc -v


```shell
[root@localhost ~]# cd gcc-4.4.7-4.el6.x86_64/
[root@localhost gcc-4.4.7-4.el6.x86_64]# ls
cloog-ppl-0.15.7-1.2.el6.x86_64.rpm  gcc-4.4.7-4.el6.x86_64.rpm      libmpdclient2-2.1-1.el5.rf.x86_64.rpm  mpfr-2.4.1-6.el6.x86_64.rpm
cpp-4.4.7-4.el6.x86_64.rpm           gmp-4.3.1-7.el6_2.2.x86_64.rpm  mpc-0.19-1.el6.rf.x86_64.rpm           ppl-0.10.2-11.el6.x86_64.rpm
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh mpfr-2.4.1-6.el6.x86_64.rpm 
warning: mpfr-2.4.1-6.el6.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
   1:mpfr                   ########################################### [100%
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh ppl-0.10.2-11.el6.x86_64.rpm 
warning: ppl-0.10.2-11.el6.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
   1:ppl                    ########################################### [100%]
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh cpp-4.4.7-4.el6.x86_64.rpm 
warning: cpp-4.4.7-4.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
   1:cpp                    ########################################### [100%]
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh cloog-ppl-0.15.7-1.2.el6.x86_64.rpm 
warning: cloog-ppl-0.15.7-1.2.el6.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
   1:cloog-ppl              ########################################### [100%]
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm 
warning: gcc-4.4.7-4.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
error: Failed dependencies:
	libgomp = 4.4.7-4.el6 is needed by gcc-4.4.7-4.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh libmpdclient2-2.1-1.el5.rf.x86_64.rpm 
warning: libmpdclient2-2.1-1.el5.rf.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 6b8d79e6: NOKEY
Preparing...                ########################################### [100%]
   1:libmpdclient2          ########################################### [100%]
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm 
warning: gcc-4.4.7-4.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
error: Failed dependencies:
	libgomp = 4.4.7-4.el6 is needed by gcc-4.4.7-4.el6.x86_64
	
	
	
	


[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh libgomp-4.4.7-17.el6.x86_64.rpm 
warning: libgomp-4.4.7-17.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 192a7d7d: NOKEY
Preparing...                ########################################### [100%]
	package libgomp-4.4.7-17.el6.x86_64 is already installed
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm 
warning: gcc-4.4.7-4.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
error: Failed dependencies:

	libgomp = 4.4.7-4.el6 is needed by gcc-4.4.7-4.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh libgomp-4.4.7-4.el6.x86_64.rpm 
warning: libgomp-4.4.7-4.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 192a7d7d: NOKEY
Preparing...                ########################################### [100%]
	package libgomp-4.4.7-17.el6.x86_64 (which is newer than libgomp-4.4.7-4.el6.x86_64) is already installed
	file /usr/lib64/libgomp.so.1.0.0 from install of libgomp-4.4.7-4.el6.x86_64 conflicts with file from package libgomp-4.4.7-17.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm 
warning: gcc-4.4.7-4.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
error: Failed dependencies:
	libgomp = 4.4.7-4.el6 is needed by gcc-4.4.7-4.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -e libgomp-4.4.7-17.el6.x86_64.rpm 
error: package libgomp-4.4.7-17.el6.x86_64.rpm is not installed
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh libgomp-4.4.7-4.el6.x86_64.rpm 
warning: libgomp-4.4.7-4.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 192a7d7d: NOKEY
Preparing...                ########################################### [100%]
	package libgomp-4.4.7-17.el6.x86_64 (which is newer than libgomp-4.4.7-4.el6.x86_64) is already installed
	file /usr/lib64/libgomp.so.1.0.0 from install of libgomp-4.4.7-4.el6.x86_64 conflicts with file from package libgomp-4.4.7-17.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -e libgomp
error: Failed dependencies:
	libgomp.so.1()(64bit) is needed by (installed) gettext-0.17-18.el6.x86_64
	libgomp.so.1(GOMP_1.0)(64bit) is needed by (installed) gettext-0.17-18.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh --replacefiles libgomp-4.4.7-4.el6.x86_64.rpm 
warning: libgomp-4.4.7-4.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 192a7d7d: NOKEY
Preparing...                ########################################### [100%]
	package libgomp-4.4.7-17.el6.x86_64 (which is newer than libgomp-4.4.7-4.el6.x86_64) is already installed
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm 
warning: gcc-4.4.7-4.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
error: Failed dependencies:
	libgomp = 4.4.7-4.el6 is needed by gcc-4.4.7-4.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh libgomp-4.4.7-4.el6.x86_64.rpm 
warning: libgomp-4.4.7-4.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 192a7d7d: NOKEY
Preparing...                ########################################### [100%]
	package libgomp-4.4.7-17.el6.x86_64 (which is newer than libgomp-4.4.7-4.el6.x86_64) is already installed
	file /usr/lib64/libgomp.so.1.0.0 from install of libgomp-4.4.7-4.el6.x86_64 conflicts with file from package libgomp-4.4.7-17.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -e libgomp
error: Failed dependencies:
	libgomp.so.1()(64bit) is needed by (installed) gettext-0.17-18.el6.x86_64
	libgomp.so.1(GOMP_1.0)(64bit) is needed by (installed) gettext-0.17-18.el6.x86_64
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -e -nodenp  libgomp
-nodenp: unknown option
[root@localhost gcc-4.4.7-4.el6.x86_64]# yum -y libgomp
Loaded plugins: product-id, refresh-packagekit, search-disabled-repos, security, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
No such command: libgomp. Please use /usr/bin/yum --help
[root@localhost gcc-4.4.7-4.el6.x86_64]# yum -y remove libgomp
Loaded plugins: product-id, refresh-packagekit, search-disabled-repos, security, subscription-manager
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
Setting up Remove Process
Resolving Dependencies
--> Running transaction check
---> Package libgomp.x86_64 0:4.4.7-17.el6 will be erased
--> Processing Dependency: libgomp.so.1()(64bit) for package: gettext-0.17-18.el6.x86_64
--> Processing Dependency: libgomp.so.1(GOMP_1.0)(64bit) for package: gettext-0.17-18.el6.x86_64
--> Running transaction check
---> Package gettext.x86_64 0:0.17-18.el6 will be erased
--> Processing Dependency: /usr/bin/msgfmt for package: redhat-lsb-core-4.0-7.el6.x86_64
--> Processing Dependency: /bin/gettext for package: redhat-lsb-core-4.0-7.el6.x86_64
--> Restarting Dependency Resolution with new changes.
--> Running transaction check
---> Package redhat-lsb-core.x86_64 0:4.0-7.el6 will be erased
--> Processing Dependency: redhat-lsb-core(x86-64) = 4.0 for package: redhat-lsb-printing-4.0-7.el6.x86_64
--> Processing Dependency: redhat-lsb-core(x86-64) = 4.0-7.el6 for package: redhat-lsb-4.0-7.el6.x86_64
--> Processing Dependency: redhat-lsb-core(x86-64) = 4.0 for package: redhat-lsb-graphics-4.0-7.el6.x86_64
--> Running transaction check
---> Package redhat-lsb.x86_64 0:4.0-7.el6 will be erased
--> Processing Dependency: redhat-lsb(x86-64) = 4.0-7.el6 for package: redhat-lsb-compat-4.0-7.el6.x86_64
---> Package redhat-lsb-graphics.x86_64 0:4.0-7.el6 will be erased
---> Package redhat-lsb-printing.x86_64 0:4.0-7.el6 will be erased
--> Running transaction check
---> Package redhat-lsb-compat.x86_64 0:4.0-7.el6 will be erased
--> Finished Dependency Resolution

Dependencies Resolved

======================================================================================================================================================
 Package                        Arch              Version                    Repository                                                          Size
======================================================================================================================================================
Removing:
 libgomp                        x86_64            4.4.7-17.el6               @anaconda-RedHatEnterpriseLinux-201604140956.x86_64/6.8            125 k
Removing for dependencies:
 gettext                        x86_64            0.17-18.el6                @anaconda-RedHatEnterpriseLinux-201604140956.x86_64/6.8            6.1 M
 redhat-lsb                     x86_64            4.0-7.el6                  @anaconda-RedHatEnterpriseLinux-201604140956.x86_64/6.8            0.0  
 redhat-lsb-compat              x86_64            4.0-7.el6                  @anaconda-RedHatEnterpriseLinux-201604140956.x86_64/6.8            0.0  
 redhat-lsb-core                x86_64            4.0-7.el6                  @anaconda-RedHatEnterpriseLinux-201604140956.x86_64/6.8             22 k
 redhat-lsb-graphics            x86_64            4.0-7.el6                  @anaconda-RedHatEnterpriseLinux-201604140956.x86_64/6.8            0.0  
 redhat-lsb-printing            x86_64            4.0-7.el6                  @anaconda-RedHatEnterpriseLinux-201604140956.x86_64/6.8            0.0  

Transaction Summary
======================================================================================================================================================
Remove        7 Package(s)

Installed size: 6.2 M
Downloading Packages:
Running rpm_check_debug
Running Transaction Test
Transaction Test Succeeded
Running Transaction
Warning: RPMDB altered outside of yum.
  Erasing    : redhat-lsb-compat-4.0-7.el6.x86_64                                                                                                 1/7 
  Erasing    : redhat-lsb-4.0-7.el6.x86_64                                                                                                        2/7 
  Erasing    : redhat-lsb-graphics-4.0-7.el6.x86_64                                                                                               3/7 
  Erasing    : redhat-lsb-printing-4.0-7.el6.x86_64                                                                                               4/7 
  Erasing    : redhat-lsb-core-4.0-7.el6.x86_64                                                                                                   5/7 
/var/tmp/rpm-tmp.S2Vz2P: line 1: lsb_release: command not found
  Erasing    : gettext-0.17-18.el6.x86_64                                                                                                         6/7 
  Erasing    : libgomp-4.4.7-17.el6.x86_64                                                                                                        7/7 
  Verifying  : redhat-lsb-4.0-7.el6.x86_64                                                                                                        1/7 
  Verifying  : redhat-lsb-graphics-4.0-7.el6.x86_64                                                                                               2/7 
  Verifying  : redhat-lsb-compat-4.0-7.el6.x86_64                                                                                                 3/7 
  Verifying  : redhat-lsb-printing-4.0-7.el6.x86_64                                                                                               4/7 
  Verifying  : redhat-lsb-core-4.0-7.el6.x86_64                                                                                                   5/7 
  Verifying  : libgomp-4.4.7-17.el6.x86_64                                                                                                        6/7 
  Verifying  : gettext-0.17-18.el6.x86_64                                                                                                         7/7 

Removed:
  libgomp.x86_64 0:4.4.7-17.el6                                                                                                                       

Dependency Removed:
  gettext.x86_64 0:0.17-18.el6                   redhat-lsb.x86_64 0:4.0-7.el6                      redhat-lsb-compat.x86_64 0:4.0-7.el6              
  redhat-lsb-core.x86_64 0:4.0-7.el6             redhat-lsb-graphics.x86_64 0:4.0-7.el6             redhat-lsb-printing.x86_64 0:4.0-7.el6            

Complete!
[root@localhost gcc-4.4.7-4.el6.x86_64]# ls
cloog-ppl-0.15.7-1.2.el6.x86_64.rpm    glibc-headers-2.12-1.192.el6.x86_64.rpm   libgomp-4.4.7-4.el6.x86_64.rpm         ppl-0.10.2-11.el6.x86_64.rpm
cpp-4.4.7-4.el6.x86_64.rpm             gmp-4.3.1-7.el6_2.2.x86_64.rpm            libmpdclient2-2.1-1.el5.rf.x86_64.rpm
gcc-4.4.7-4.el6.x86_64.rpm             kernel-headers-2.6.32-642.el6.x86_64.rpm  mpc-0.19-1.el6.rf.x86_64.rpm
glibc-devel-2.12-1.192.el6.x86_64.rpm  libgomp-4.4.7-17.el6.x86_64.rpm           mpfr-2.4.1-6.el6.x86_64.rpm
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh libgomp-4.4.7-4.el6.x86_64.rpm 
warning: libgomp-4.4.7-4.el6.x86_64.rpm: Header V4 DSA/SHA1 Signature, key ID 192a7d7d: NOKEY
Preparing...                ########################################### [100%]
   1:libgomp                ########################################### [100%]
[root@localhost gcc-4.4.7-4.el6.x86_64]# rpm -ivh gcc-4.4.7-4.el6.x86_64.rpm 
warning: gcc-4.4.7-4.el6.x86_64.rpm: Header V3 RSA/SHA1 Signature, key ID c105b9de: NOKEY
Preparing...                ########################################### [100%]
   1:gcc                    ########################################### [100%]
[root@localhost gcc-4.4.7-4.el6.x86_64]# gcc -v
Using built-in specs.
Target: x86_64-redhat-linux
Configured with: ../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-languages=c,c++,objc,obj-c++,java,fortran,ada --enable-java-awt=gtk --disable-dssi --with-java-home=/usr/lib/jvm/java-1.5.0-gcj-1.5.0.0/jre --enable-libgcj-multifile --enable-java-maintainer-mode --with-ecj-jar=/usr/share/java/eclipse-ecj.jar --disable-libjava-multilib --with-ppl --with-cloog --with-tune=generic --with-arch_32=i686 --build=x86_64-redhat-linux
Thread model: posix
gcc version 4.4.7 20120313 (Red Hat 4.4.7-4) (GCC) 
[root@localhost gcc-4.4.7-4.el6.x86_64]# 
```

