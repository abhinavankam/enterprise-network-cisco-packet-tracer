# Incident-04: Internet Connectivity Failure

## Incident Description
Internal users cannot access the internet or reach external servers (8.8.8.8, Internet Server).

## Symptoms Observed
- Internal PCs can ping internal gateway (192.168.1.1)
- Cannot ping 8.8.8.8 or external server (192.168.50.10)
- `show ip nat translations` shows no active translations
- Internet access is completely down

## Investigation Steps
1. Verified NAT configuration on EDGE-R1
2. Checked ACL used for NAT
3. Verified static route to ISP
4. Checked default gateway on routers
5. Verified `ip nat inside` and `ip nat outside` interfaces
6. Checked ISP connectivity

## Commands Used
#show ip nat translations

#show ip nat statistics

#show ip access-lists

#show ip route

#show running-config | include nat

#show interfaces serial0/0/0

#ping 8.8.8.8 source serial0/0/0


## Root Cause
NAT/PAT configuration was incomplete. The NAT ACL was not matching internal traffic, or the `ip nat inside/outside` interfaces were not correctly set on EDGE-R1.

## Resolution
1. Configured NAT ACL to match internal networks (10.0.0.0/8, 192.168.0.0/16)
2. Set `ip nat inside` on internal interfaces (Gig0/0, Gig0/1)
3. Set `ip nat outside` on external interface (Serial0/0/0)
4. Applied `ip nat inside source list 1 interface serial0/0/0 overload`
5. Configured default route to ISP (0.0.0.0 0.0.0.0 203.0.113.1)
6. Verified NAT is operational

## Validation
EDGE-R1# show ip nat translations

#Pro  Inside global     Inside local       Outside local  Outside global

#icmp 203.0.113.1:1024  192.168.1.10:1024  8.8.8.8:1024   8.8.8.8:1024

#icmp 203.0.113.1:1025  192.168.1.11:1024  8.8.8.8:1024   8.8.8.8:1024

PC> ping 8.8.8.8

#Reply from 8.8.8.8: bytes=32 time=2ms TTL=128

#Reply from 8.8.8.8: bytes=32 time=2ms TTL=128

PC> ping 192.168.50.10

#Reply from 192.168.50.10: bytes=32 time=1ms TTL=127

✅ Internet connectivity is restored. NAT translations are working.
