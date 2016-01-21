# sub2rbl
**Implements iptables RBL blocking using ipset** - @robzr

Subscribe to RBL - minimalist OpenWRT (Chaos Calmer) script to download
and compile IP based and CIDR (net) based RBLs from various sources into
ipsets and insert rules into iptables which drops packets originating
from entries in RBLs.

The included config file (/etc/config/sub2rbl) includes a number of IP
based RBLs (primarily based on ssh brute force scanning) and Spamhaus
DROP/EDROP net based RBLs (based on hijacked IP ranges used by spammers
and cyber-criminals).

**Dependencies**

- ipset + kmod-ipt-ipset for basic operation
- curl + ca-certificates recommended for HTTPS RBLs (configured by default) -or-
- wget + openssl-util + ca-certificates is an alternative to curl (GNU wget)

sub2rbl will intelligently select best option between curl/wget.  To force the use
of one, or modify the behavior, use the uci option "webGetCmd" (read script for details)

**Logging**

sub2rbl logs to syslog, so use the logread command to view the log.
Optionally, use "-f stdout" to log to stdout, and "-l 2" or "-l 3"
to increase logging verbosity.

**Config**

sub2rbl runs out of the box with sane settings. Config options are 
stored in /etc/config/sub2rbl and are overridable at runtime with 
command line arguments.  sub2rbl -h for a list of arguments.

**Installation**

	opkg install ipset kmod-ipt-ipset curl ca-certificates
	wget -O /etc/config/sub2rbl http://cdn.rawgit.com/robzr/sub2rbl/master/config/sub2rbl
	wget -O /usr/sbin/sub2rbl http://cdn.rawgit.com/robzr/sub2rbl/master/sub2rbl
	chmod 755 /usr/sbin/sub2rbl
	echo '0 */6 * * * /usr/sbin/sub2rbl' >> /etc/crontabs/root
	/etc/init.d/cron enable
	# And to watch it in action, for the first run, try:
	sub2rbl -l 2 -f stdout

**Monitoring**

You can take a look at the packet counts (first column) to see how many connection attempts the sub2rbl sets have prevented.

	root@gw:~# iptables -nvL input_wan_rule
	Chain input_wan_rule (1 references)
	 pkts bytes target     prot opt in     out     source               destination         
	    0     0 DROP       all  --  *      *       0.0.0.0/0            0.0.0.0/0            match-set sub2rbl-net src
	  204 12572 DROP       all  --  *      *       0.0.0.0/0            0.0.0.0/0            match-set sub2rbl src
	 3390  389K bearDropper  all  --  *      *       0.0.0.0/0            0.0.0.0/0           

**TBD**

- Add elegant auto-hook to forward chain (ex: forwarding_wan_rule)
- package
- ipv6

Also see the sister project bearDropper for log based bans: http://github.com/robzr/bearDropper
