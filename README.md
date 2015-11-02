This is the README file for ppp-2.4, a package which implements the
Point-to-Point Protocol (PPP) to provide Internet connections over
serial lines with few enhancements for parameters

Debian original patch was discussed here https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=458646
But this patch only allows you to configure a static interface name like pppoe or pptp.

In the case of the server which manages the PPP incomming connections I was facing with
the problem to make a difference between the technology I'm using in the concentrator.

So for this I wanted to rename the interfaces like pppoes0, pppoes1, pptp2, pptp3, l2tp4 etc.
Also a good application of this is to use the specific interface name to separate iptables rules
by the technology used.

A good example is VyOS which renames the interfaces by tehnology used.

I've tried the method of VyOS but I've faced with a problem when Radius plugin sends the updates
to the Radius server: it couldn't find the interfaces.

Because of this also pppd itselfs had some issues because couldn't find the interface.
We have ifname variable in pppd which contain the interface name. After the renaming in scripts
in ip-pre-up.d or ip-up.d this variable contains an old value of the interface like ppp0, ppp1 etc.
The plugins uses this variable to find the interface statistics for example.

I've decided to change the original Debian patch and add my contribution to create an enhanced version
of the patch.

This patch is takes a parameter ifname in format pppoesN, pptpN or l2tpN for example where N will be replaced
in the core of pppd with the unit of the ppp interfaces.

For example /etc/ppp/pptpd-options:

require-pap
login
lcp-echo-interval 10
lcp-echo-failure 2
name pptpd
refuse-pap
refuse-chap
refuse-mschap
require-mschap-v2
require-mppe-128
proxyarp
nodefaultroute
debug
kdebug 3
dump
lock
nobsdcomp
novj
novjccomp
nologfd
ifname pptpN


As you can see adding pptpN parameter will generate pptp0, pptp1 interface in Linux.
Also if you don't put the character 'N' the interface will be renamed only 'pptp' or how do you want.

Hope it will helps my patch.
