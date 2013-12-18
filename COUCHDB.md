Warning: the following process fails to download software securely.

~~~
yum install wget
wget ftp://mirror.cs.princeton.edu/pub/mirrors/fedora-epel/6/i386/epel-release-6-8.noarch.rpm
rpm -ivh epel-release-6-8.noarch.rpm
yum install couchdb
chkconfig --add couchdb
chkconfig couchdb on
~~~

Edit _/etc/couchdb/local.ini_ and do the following.

- Set bind_address to the ip address clients will use to reach the server.
- In the [admins] section, add an admin user.

Configure iptables to restrict connections to a given range of ip addresses.

~~~
iptables -A INPUT -p tcp -m iprange --src-range 139.182.148.49-139.182.148.154 --dport 5984 -j ACCEPT
service iptables save
~~~

Reboot the system or run the following.

   service couchdb start

