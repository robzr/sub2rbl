# sub2rbl
**Implements iptables RBL blocking using ipset** - @robzr

Subscribe to RBL - minimalist OpenWRT (Chaos Calmer) script to download
and compile IP based and CIDR (net) based RBLs from various sources into
ipsets and insert rules into iptables which drops packets originating
from entries in RBLs.

The included config file (/etc/config/sub2rbl) includes a number of IP
based RBLs (primarily based on ssh brute force scanning) and Spamhaus
DROP/EDROP net based RBLs (based on hijacked IP ranges used by spammers
and cyber-criminals)

**Dependencies**
- curl (sadly busybox/wget did not work for all the RBLs I tried)
- ipset & kmod-ipt-ipset
- openssl-util & ca-certificates (needed for https RBL URLs)

**Logging**

sub2rbl logs to syslog, so use the logread command to view the log.
Optionally, use "-f stdout" to log to stdout, and "-l 2" or "-l 3"
to increase logging verbosity.

**Config**

sub2rbl runs out of the box with sane settings. Config options are 
stored in /etc/config/sub2rbl and are overridable at runtime with 
command line arguments.  sub2rbl -h for a list of arguments.

**Installation**

	opkg install ipset kmod-ipt-ipset openssl-util curl ca-certificates
	wget -O /etc/config/sub2rbl https://raw.githubusercontent.com/robzr/sub2rbl/master/config/sub2rbl
	wget -O /usr/sbin/sub2rbl https://raw.githubusercontent.com/robzr/sub2rbl/master/sub2rbl
	chmod 755 /usr/sbin/sub2rbl
	echo '0 */6 * * * /usr/sbin/sub2rbl' >> /etc/crontabs/root
	/etc/init.d/cron enable

**TBD**
- package
- ipv6

Also see the sister project bearDropper for log based bans: http://github.com/robzr/bearDropper
