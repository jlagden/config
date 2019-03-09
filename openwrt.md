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
```
config service 'myddns_ipv4'
	option interface 'wan'
	option ip_source 'network'
	option ip_network 'wan'
	option service_name 'afraid.org-keyauth'
	option enabled '1'
	option lookup_host '???'
	option password '???'
```
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


```
config openvpn 'vpn'
	option enabled '1'
	option verb '3'
	option user 'nobody'
	option group 'nogroup'
	option dev 'tun'
	option port '1194'
	option proto 'udp4'
	option server '10.8.0.0 255.255.255.0'
	option topology 'subnet'
	option persist_tun '1'
	option persist_key '1'
	list push 'route 192.168.1.0 255.255.255.0'
	list push 'dhcp-option DNS 192.168.1.2'
	list push 'dhcp-option DNS 192.168.1.1'
	list push 'redirect-gateway def1'
	list push 'persist-tun'
	list push 'persist-key'
	option keepalive '10 120'
	option ca '/etc/openvpn/ca.crt'
	option cert '/etc/openvpn/vpn-server.crt'
	option key '/etc/openvpn/vpn-server.key'
	option dh '/etc/openvpn/dh.pem'
	option tls_crypt '/etc/openvpn/tc.key'
	option cipher 'AES-256-CBC'
	option fragment '1400'
```
```
config rule
        option name 'Allow-OpenVPN-Inbound'
        option target 'ACCEPT'
        option src 'wan'
        option proto 'udp'
        option dest_port '1194'

config zone 'vpn'
        option name 'vpn'
        option network 'vpn'
        option input 'ACCEPT'
        option forward 'REJECT'
        option output 'ACCEPT'
        option masq '1'

config forwarding 'vpn_forwarding_lan_in'
        option src 'vpn'
        option dest 'lan'

config forwarding 'vpn_forwarding_lan_out'
        option src 'lan'
        option dest 'vpn'

config forwarding 'vpn_forwarding_wan'
        option src 'vpn'
        option dest 'wan'
```

