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

If you want to restrict connections to a given range of
IP addresses, then add a version of the following command 
to _/etc/sysconfig/iptables_ just after the similar command
that opens port 22.
I think I did the following to make this change persist across restarts,
but I'm not sure.

~~~
service iptables save
~~~

Reboot the system or run the following.

   service couchdb start

