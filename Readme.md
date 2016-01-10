# sub2rbl
**Synchronizes RBLs to iptables** --Rob Zwissler @robzr

Subscribe to RBL - minimalist OpenWRT (Chaos Calmer) script to download
and compile RBLs from various sources into an ipset and insert an
iptables rule which drops packets originating from entries in RBLs.

The included config file (/etc/config/sub2rbl) has a few ssh-scan based 
RBLs which will work out of the box.

**Installation**

	echo '0 */6 * * * /usr/bin/sub2rbl' >> /etc/crontabs/root

**Dependencies**
 - ipset & kmod-ipt-ipset
 - openssl (if HTTPS based RBLs are used)

Also see the sister project bearDropper for log based bans: http://github.com/robzr/bearDropper

