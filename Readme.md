# sub2rbl
--Rob Zwissler @robzr

**Synchronizes RBLs to iptables**

Subscribe to RBL - minimalist OpenWRT (Chaos Calmer) script to download
and compile RBLs from various sources into an ipset and insert an
iptables rule which drops packets originating from entries in RBLs.

The included config file (/etc/config/sub2rbl) has a few ssh-scan based 
RBLs which will work out of the box.

Recommended installation is to run via a cron entry, which can be setup
as follows:

	echo '0 */6 * * * /usr/bin/sub2rbl' >> /etc/crontabs/root

Dependencies include ipset, and openssl (if HTTPS based RBLs are used).

