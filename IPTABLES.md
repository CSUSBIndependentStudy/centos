Iptables is the name of a service on the system that does packet filtering.
This is also referred to as a firewall.

After the base install, port 22 is open by default,
which allows you to access the system through ssh.

Suppose you want to open port 5000.  The following will do this.

    iptables -A INPUT -p tcp --dport 5000 -j ACCEPT
    service iptables save
    
The first command opens the port; the second command saves the
current state of iptables to be reloaded when the system restarts.

