## Wireless
* Reduce transmit power on wireless networks to 15dBm

`/etc/config/wireless`
```
config wifi-device '???'
	option txpower '15'
```
## DHCP
* Main dhcp options can be left as default:
```
config dnsmasq
	option domainneeded '1'
	option localise_queries '1'
	option rebind_protection '1'
	option rebind_localhost '1'
	option local '/lan/'
	option domain 'lan'
	option expandhosts '1'
	option authoritative '1'
	option readethers '1'
	option leasefile '/tmp/dhcp.leases'
	option resolvfile '/tmp/resolv.conf.auto'
	option nonwildcard '1'
	option localservice '1'
```
* dhcp pools - remove ra and dhcpv6 options, dhcp options to return pi-hole as DNS server and openwrt as secondary
```
config dhcp 'lan'
	option interface 'lan'
	option start '100'
	option limit '150'
	option leasetime '12h'
	list dhcp_option '6,192.168.1.2,192.168.1.1'
```
* Configure static leases
```
config host
	option name 'mypc'
	option dns '1'
	option mac '00:11:22:33:44:55'
```
## DNS
* Upstream DNS Servers - Cloudfare - 1.1.1.1, 1.0.0.1

`/etc/config/network`
```
config interface 'wan'
	option peerdns '0'
  	option dns '1.1.1.1 1.0.0.1'
```
## Guest WiFi
  `/etc/config/wireless`
  ```
  config wifi-iface
       option device '???'
       option mode 'ap'
       option network 'guest'
       option ssid 'Guest'
       option encryption 'psk2'
       option key 'GuestWifiKey'
       option isolate '1'
  ```  
  `/etc/config/network`
  ```
  config interface 'guest'
       option proto 'static'
       option ipaddr '192.168.2.1'
       option netmask '255.255.255.0'
  ```
  * Guest Wifi use OpenDNS Family shield for DNS - 208.67.222.123, 208.67.220.123
  
  `/etc/config/dhcp`
  ```
  config dhcp 'guest'
    	option interface 'guest'
    	option start '100'
    	option limit '150'
    	option leasetime '1h'
    	list dhcp_option '6,208.67.222.123,208.67.220.123'
  ```
  `/etc/config/firewall`
  ```
  config zone                                     
    	option name 'guest'                 
    	option network 'guest'
    	option input 'REJECT'        
    	option forward 'REJECT'             
    	option output 'ACCEPT'              
       
  config forwarding                               
    	option src 'guest'                  
   	option dest 'wan'
       
  # Allow DHCP Guest -> Router
  # DHCP communication uses UDP ports 67-68
  config rule
    	option name 'Allow-Guest-DHCP'
    	option src 'guest'
    	option dest_port '67-68'
    	option proto 'udp'
    	option target 'ACCEPT'
    
  # Don't allow access to the cable modem
  config rule
    	option name 'Deny-Guest-Cable-Modem'
    	option src 'guest'
    	option dest 'wan'
    	option dest_ip '192.168.100.1'
    	option family 'ipv4'
    	option proto 'all'
    	option target 'REJECT'
  ```
## Dynamic DNS 
* https://openwrt.org/docs/guide-user/base-system/ddns
## VPN
* Install Easy-RSA
```
opkg update
opkg install openvpn-easy-rsa
```
* OpenWRT has a good guide on creating the key pairs so will use this. EASYRSA_PKI is the PKI directory, EASYRSA_REQ_CN is the Certificate Authority Common Name. vpnserver is the servers common name and vpnclient is the client's common name.
```
# Configuration parameters
export EASYRSA_PKI="/etc/easy-rsa/pki"
export EASYRSA_REQ_CN="vpnca"
 
# Remove and re-initialize the PKI directory
easyrsa --batch init-pki
 
# Generate DH parameters
# May take a while to complete (~25m on WRT3200ACM)
easyrsa --batch gen-dh
 
# Create a new CA
easyrsa --batch build-ca nopass
 
# Generate a keypair and sign locally for vpnserver
easyrsa --batch build-server-full vpnserver nopass
 
# Generate a keypair and sign locally for vpnclient
easyrsa --batch build-client-full vpnclient nopass
```

https://openvpn.net/community-resources/how-to/#openvpn-quickstart
https://openwrt.org/docs/guide-user/services/vpn/openvpn/basic
https://github.com/OpenVPN/easy-rsa/blob/master/README.quickstart.md
https://wiki.gentoo.org/wiki/Create_a_Public_Key_Infrastructure_Using_the_easy-rsa_Scripts
https://wiki.archlinux.org/index.php/Easy-RSA
https://github.com/pivpn/easy-rsa/blob/master/doc/EasyRSA-Readme.md
https://community.openvpn.net/openvpn/wiki/EasyRSA3-OpenVPN-Howto
https://github.com/OpenVPN/easy-rsa/blob/master/easyrsa3/easyrsa
