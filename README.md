# homeassistant
Konfigurace na Turris Omnia
Install Docker
Install needed opkg packages on the Turris Omnia host
opkg install kmod-veth
opkg install kmod-ipt-extra
opkg install iptables-mod-extra
Install a new container (I created an arch linux container)
Open up the /srv/lxc/containername/config file for editing
Uncomment the line that enables nesting
# Uncomment the following line to support nesting containers:
lxc.include = /usr/share/lxc/config/nesting.conf
# (Be aware this has security implications)
Configure network to bridge to lan
(not shown: either set ip static on lxc container OS, or use DHCP)
# Network configuration
lxc.net.0.type = veth
lxc.net.0.link = br-lan
lxc.net.0.flags = up
lxc.net.0.name = eth0
lxc.net.0.hwaddr = 22:11:85:de:f9:fb
Add the following lines to open up access to the host, allowing docker to successfully start
raw.lxc: |-
lxc.mount.auto = cgroup:rw:force
lxc.cgroup.devices.allow = a
security.nesting: "true"
security.privileged: "true"
lxc.cap.drop =
Save and exit the config file

Start the lxc container

lxc-start -n <container_name>
Attach to the console of the lxc container
lxc-attach -n <container_name>
Install docker (arch, so pacman)
pacman -Syu
pacman -S docker
pacman -S lxc
Enable Docker
systemctl enable docker
systemctl start docker
