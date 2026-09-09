# Incident-01: DHCP Failure

## Incident Description
PC in the network is unable to obtain an IP address via DHCP.

## Symptoms Observed
- PC shows APIPA address (169.254.x.x)
- `ipconfig` shows no DHCP lease
- Unable to ping gateway
- Network connectivity is not established

## Investigation Steps
1. Checked switch port configuration
2. Verified VLAN assignment on the access port
3. Checked DHCP server status
4. Verified DHCP pool configuration on the router
5. Checked if DHCP relay was needed

## Commands Used
#show vlan brief
show interfaces status
#show interfaces status
#show ip dhcp binding
#show ip dhcp pool
#show running-config | include dhcp
#debug ip dhcp server events

## Root Cause
PC switch port was assigned to the wrong VLAN. The DHCP server was not reachable on that VLAN, causing the PC to fail to obtain an IP address.

## Resolution
1. Changed switch port access VLAN to the correct VLAN (VLAN 10)
2. Verified port is in access mode
3. Confirmed PC received DHCP address
4. Verified connectivity to gateway

## Validation
PC> ipconfig

IP Address: 192.168.1.101
Subnet Mask: 255.255.255.0
#Default Gateway: 192.168.1.1
#DHCP Server: 192.168.1.1

#PC> ping 192.168.1.1
#Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

✅ PC successfully obtained DHCP address and can ping the gateway.
