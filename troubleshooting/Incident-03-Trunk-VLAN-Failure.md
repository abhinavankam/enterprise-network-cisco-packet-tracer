# Incident-03: Trunk VLAN Failure

## Incident Description
VLANs are not passing across the trunk link between SW1 and SW2, causing connectivity issues for devices connected to SW2.

## Symptoms Observed
- PCs connected to SW2 cannot reach their gateway
- `show interfaces trunk` shows no VLANs allowed or trunk not operational
- VLANs exist on both switches but traffic is not passing
- PC to PC communication across switches fails

## Investigation Steps
1. Checked trunk interface status on both switches
2. Verified allowed VLANs on trunk
3. Checked for native VLAN mismatch
4. Verified VLAN database on both switches
5. Checked encapsulation type

## Commands Used
#show interfaces trunk

#show interfaces gig0/1 switchport

#show vlan brief

#show running-config interface gig0/1

#show interfaces gig0/1

## Root Cause
Trunk configuration was missing the `switchport trunk allowed vlan` command, or the native VLAN did not match on both sides of the trunk link.

## Resolution
1. Configured trunk mode on both switch interfaces
2. Allowed required VLANs (10, 20, 30, 99)
3. Set native VLAN to 99 on both sides
4. Verified trunk is operational
5. Tested traffic across the trunk

## Validation
SW2# show interfaces trunk

#Port Mode Encapsulation Status Native vlan

#Gi0/1 on 802.1q trunking 99

#Vlans allowed on trunk:

#1,10,20,30,99

#Vlans allowed and active in management domain:

#10,20,30,99

PC1> ping 192.168.20.10

#Reply from 192.168.20.10: bytes=32 time=2ms TTL=255

✅ Trunk is now operational and all VLANs are passing traffic.
