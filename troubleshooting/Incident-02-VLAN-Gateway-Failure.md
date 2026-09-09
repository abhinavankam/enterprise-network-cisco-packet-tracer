# Incident-02: VLAN Gateway Failure

## Incident Description
Devices in VLAN 20 cannot communicate with devices in VLAN 10 or reach their gateway.

## Symptoms Observed
- PCs in same VLAN (VLAN 20) can ping each other
- Cannot ping gateway IP (192.168.20.1)
- Inter-VLAN communication is failing
- `show ip route` shows missing VLAN 20 network

## Investigation Steps
1. Verified VLANs exist on switch configuration
2. Checked trunk ports between switches
3. Verified router subinterface configuration
4. Checked routing table on CORE-R1
5. Verified interface status on router

## Commands Used
show vlan brief
show interfaces trunk
show ip interface brief
show ip route
show running-config interface gig0/0.20
show interfaces gig0/0.20


## Root Cause
Router subinterface for VLAN 20 was either not configured or administratively shut down. The gateway IP was not active, preventing VLAN 20 devices from routing to other networks.

## Resolution
1. Configured subinterface for VLAN 20 on CORE-R1
2. Assigned correct IP address (192.168.20.1/24)
3. Enabled 802.1Q encapsulation for VLAN 20
4. Brought interface up with `no shutdown`
5. Verified subinterface is active

## Validation
PC1> ping 192.168.20.1
Reply from 192.168.20.1: bytes=32 time<1ms TTL=255

PC1> ping 192.168.10.1
Reply from 192.168.10.1: bytes=32 time=1ms TTL=254

✅ Gateway is now reachable and inter-VLAN routing is working.
