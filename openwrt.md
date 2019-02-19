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
* DNS request hijacking (Google likes to do their own thing)

`/etc/config/firewall`
```
config redirect
      option name 'DNS LAN redirect'
      option src 'lan'
      option src_dport '53'
      option dest_ip '192.168.1.2'
      option target 'DNAT'
      option proto 'udp'
      option dest 'wan'
```
## Guest WiFi
  `/etc/config/wireless`
  ```
  config wifi-iface
       option device     '???'
       option mode       'ap'
       option network    'guest'
       option ssid 'Guest Wifi'
       option encryption 'psk2+ccmp'
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
    option interface	'guest'
    option start		'100'
    option limit		'150'
    option leasetime	'1h'
    list   dhcp_option	'6,208.67.222.123,208.67.220.123'
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
    option name 'Guest-DHCP'
    option src 'guest'
    option src_port '67-68'
    option dest_port '67-68'
    option proto 'udp'
    option target 'ACCEPT'
    
  # Don't allow access to the cable modem
  config rule
    option name 'Guest-Block-Cable-Modem'
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
## Themes
* https://github.com/apollo-ng/luci-theme-darkmatter
