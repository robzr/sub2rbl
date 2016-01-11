# sub2rbl
**Implements iptables RBL blocking using ipset** - @robzr

Subscribe to RBL - minimalist OpenWRT (Chaos Calmer) script to download
and compile RBLs from various sources into an ipset and insert a rule
into iptables which drops packets originating from entries in RBLs.

The included config file (/etc/config/sub2rbl) has a few ssh-scan based 
RBLs which will work out of the box.

**Dependencies**
- ipset & kmod-ipt-ipset
- openssl-util (needed for https RBL URLs)

**Logging**

sub2rbl logs to syslog, so use the logread command to view the log.

**Config**

sub2rbl runs out of the box with sane settings. Config options are 
stored in /etc/config/sub2rbl and are overridable at runtime with 
command line arguments.  sub2rbl -h for a list of arguments.

**Installation**

	opkg install ipset kmod-ipt-ipset openssl-util
	wget -O /etc/config/sub2rbl https://raw.githubusercontent.com/robzr/sub2rbl/master/config/config
	wget -O /usr/sbin/sub2rbl https://raw.githubusercontent.com/robzr/sub2rbl/master/sub2rbl
	chmod 755 /usr/sbin/sub2rbl
	echo '0 */6 * * * /usr/bin/sub2rbl' >> /etc/crontabs/root

Also see the sister project bearDropper for log based bans: http://github.com/robzr/bearDropper

**TBD**
- Add more RBLs and RBL handling
- package
- ipv6
