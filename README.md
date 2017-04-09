```

https://ru.wikipedia.org/wiki/DRBD

# install deps
yum install gcc make automake autoconf libxslt libxslt-devel flex rpm-build kernel-devel -y
yum install help2man pygobject2 pandoc gcc-c++ -y

# compile and build rpms
cd /root && mkdir -p rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS}

git clone git://git.linbit.com/drbd-9.0.git && cd drbd-9* && make tarball && make kmp-rpm && cd ..
git clone git://git.linbit.com/drbd-utils.git && cd drbd-u* && ./autogen.sh && ./configure && make tarball && make rpm && cd ..
git clone git://git.linbit.com/drbdmanage.git && cd drbdm* && make all && make rpm && cd ..
git clone git://git.linbit.com/drbdmanage-docker-volume.git && cd drbdmanage-d* && make doc && make all && make rpm && ..

rpm -Uvh --replacefiles drbd-utils-*.rpm kmod-drbd-*.rpm drbdmanage-*.rpm 
# or
rpm -Uvh ./*.rpm

systemctl enable docker-drbdmanage-plugin.socket
systemctl start docker-drbdmanage-plugin.socket
systemctl status docker-drbdmanage-plugin.socket
systemctl restart docker


# for happy drbd need open this ports (f*king drbd developers dont tell this in docs)
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 6999 -j ACCEPT
iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 6996 -j ACCEPT


http://xmodulo.com/manage-lvm-volumes-centos-rhel-7-system-storage-manager.html

ssm add -p drbdpool /dev/sdb

# init on node with smaller disk
drbdmanage init 10.22.240.96
drbdmanage init 10.22.240.69
drbdmanage uninit

# to get add node string
drbdmanage new-node centgate96.localdomain 172.22.240.96

# AND DONT do this! ;)
systemctl enable drbd



```
