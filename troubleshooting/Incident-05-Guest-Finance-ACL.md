# Incident-05: Guest-Finance ACL Issue

## Incident Description
Guest VLAN users can access Finance VLAN devices, which violates the company's security policy requiring complete isolation of Guest network from sensitive Finance data.

## Symptoms Observed
- Guest users can successfully ping Finance VLAN devices
- Guest users can access Finance network shares
- Security policy requires Guest VLAN to be completely isolated
- Finance VLAN contains sensitive financial data

## Investigation Steps
1. Checked existing ACL configurations on CORE-R1
2. Verified ACL applied to correct interface
3. Tested access from Guest to Finance
4. Reviewed security requirements
5. Checked ACL order and logic

## Commands Used
#show access-lists

#show ip interface brief

#show running-config | include access-group

#show ip access-lists 110

#show interfaces gig0/0.30

## Root Cause
The Extended ACL to block Guest-to-Finance traffic was either missing, incorrectly configured with wrong source/destination networks, or not applied to the correct router subinterface.

## Resolution
1. Created Extended ACL 110 to deny Guest (VLAN 30: 192.168.30.0/24) to Finance (VLAN 20: 192.168.20.0/24)
2. Allowed other necessary traffic (Guest to Internet, DNS, DHCP)
3. Applied ACL 110 to Guest subinterface inbound (Gig0/0.30)
4. Verified ACL is active and working

## Validation
Guest-PC> ping 192.168.20.10

#Request timed out.

#Request timed out.

#Request timed out.

Guest-PC> ping 192.168.10.10

#Reply from 192.168.10.10: bytes=32 time=3ms TTL=255

CORE-R1# show access-lists 110

#Extended IP access list 110

#10 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255

#20 permit ip 192.168.30.0 0.0.0.255 any

#30 permit ip any any

CORE-R1# show ip interface gig0/0.30

#GigabitEthernet0/0.30 is up, line protocol is up

#Inbound access list is 110

//Guest to Finance traffic is now blocked. Security policy is enforced//
