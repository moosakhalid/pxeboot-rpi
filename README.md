#####Instructions , these instructions are for RPi Ubuntu 14.04 distro Jessie

1) apt-get install isc-dhcp-server tftp-hpa lftp

#Fetch a netboot installer from ubuntu(http://cdimage.ubuntu.com/netboot/)  archives for serving to PXE boot clients, in this case I'm fetching
#installer for Ubuntu distro Yakkety Yak, you can set up yours as you prefer


2) lftp -c "open archive.ubuntu.com/ubuntu/dists/yakkety/main/installer-amd64/current/images/; mirror netboot"

move the contents of netboot dir to /var/lib/tftpboot or your tftp directory as specified in /etc/default/tftp-hpa

3) mv /netboot/* /var/lib/tftpboot

Backup your /etc/dhcp/dhcpd.conf and /etc/default/tftp-hpa

4) >> cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.orig 
   >> cp /etc/default/tftp-hpa /etc/default/tftp-hpa-orig


5) copy paste the contents dhcpd.conf and tftp-hpa files in the repo and edit dhcpd.conf according to you network

  - the next-server parameter is the dhcp server telling pxe client where to look for tftp-server, if it is the same 
    server as the one running the dhcp server than give it's IP here.
    
  - the filename parameter tells the client the name of file to get for starting the installer it's by default 
    "pxelinux.0" unless you have changed or gotten a modified netboot installer
    
  - option-domain-name-servers  specificies the DNS nameservers for your network
  
  - authoritative =  it allows your DHCP server to send a DHCPNACK to misconfigured clients. An example of a 
   misconfigured client would be a computer that was physically moved to another subnet without releasing its old lease.  - option routers provides your routers IP address
   
  - Provide the IP range and network and subnet mask according to your network


6) service isc-dhcp-server start && service tftp-hpa start

** Important = Ensure no other DHCP server is running in the LAN except the one on your server.


