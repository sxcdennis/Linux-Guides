# VLAN
VLAN = Virutal Local Area Network.
Essentially what that is, is being able to logically split up different networks with switch. What that means is, you can have one switch manage different LAN's.

# linux
To add a VLAN device using the IP command.

1. `ip link add link device1 name vlandevice type vland id vlanID`

### Example:

`ip link add link eth0 name eth0.5 type vlan id 5`

device1 = eth0  vlandevice= eth0.5 vlan id =5

2. `ip link`

3. `ip -d link show vlandevice`

You need to activate and add an IP address to vlan link, type:

4. `ip addr add ipaddr/subnet brd 192.168.1.255 dev eth0.5`

5.  `ip link set dev eth0.5 up`

How can I remove VLAN ID 5?

`ip link set dev eth0.5 down
ip link delete eth0.5`  
