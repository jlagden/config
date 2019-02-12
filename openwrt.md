* Reduce transmit power on wireless networks
* https://openwrt.org/docs/guide-user/dns-request-hijacking
* **DHCP**
  * Main dhcp options can be left as default:
  ```
  config 'dnsmasq'
	  option domainneeded		'1'
	  option boguspriv		'1'
	  option filterwin2k		'0'
	  option localise_queries	'1'
	  option rebind_protection	'1'
	  option rebind_localhost	'1'
	  option local		'/lan/'
	  option domain		'lan'
	  option expandhosts		'1'
	  option nonegcache		'0'
	  option authoritative	'1'
	  option readethers		'1'
	  option leasefile		'/tmp/dhcp.leases'
	  option resolvfile		'/tmp/resolv.conf.auto'
	  option localservice		'1'
  ```
  * DHCP Pools - Remove ra and dhcpv6 options, dhcp options to return pi-hole as DNS server
  ```
  config dhcp 'lan'
	option interface	'lan'
	option start		'100'
	option limit		'150'
	option leasetime	'12h'
	list   dhcp_option	'6,192.168.1.2,192.168.1.1'
	
  config dhcp 'guest'
	option interface	'guest'
	option start		'100'
	option limit		'150'
	option leasetime	'2h'
	list   dhcp_option	'6,208.67.222.123,208.67.220.123'
  ```
  * Configure static leases
  ```
  config host
        option ip       '192.168.1.3'
        option mac      '00:11:22:33:44:55'
        option name     'mypc'
  ```
* Upstream DNS Servers
  * LAN- Cloudfare - 1.1.1.1, 1.0.0.1
  * Guest Wifi - OpenDNS Family shield - 208.67.222.123, 208.67.220.123
* dyndns https://openwrt.org/docs/guide-user/base-system/ddns
* openvpn
* collectd
  * https://openwrt.org/docs/guide-user/perf_and_log/statistic.collectd
  * https://openwrt.org/docs/guide-user/perf_and_log/statistic.rrdcollect
  * https://openwrt.org/docs/guide-user/perf_and_log/statistic.rrdtool
  * https://openwrt.org/docs/guide-user/perf_and_log/statistical.data.overview
* https://github.com/apollo-ng/luci-theme-darkmatter
