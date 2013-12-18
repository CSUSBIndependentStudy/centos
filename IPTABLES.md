Iptables is the name of a service on the system that does packet filtering.
This is also referred to as a firewall.

After the base install, port 22 is open by default,
which allows you to access the system through ssh.

Suppose you want to open port 5000.  The following will do this.

    iptables -A INPUT -p tcp --dport 5000 -j ACCEPT
    service iptables save
    
The first command opens the port; the second command saves the
current state of iptables to be reloaded when the system restarts.

To accept connections to port 5984 from a limited range of IP addresses,
use the following.

    iptables -A INPUT -p tcp -m iprange --src-range 139.182.148.49-139.182.148.154 --dport 5984 -j ACCEPT
