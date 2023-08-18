1. Open /etc/sysctl.conf and ensure that its values match the following:
```
	net.ipv4.ip_forward = 1
	net.ipv4.conf.default.rp_filter = 0
	net.ipv4.conf.default.accept_source_route = 0
```
2. Apply the changes in step 1 by executing the command:
```
	sysctl -p
```
4. Open /etc/ipsec.conf and look for the line below. Ensure that the # in front of the line has been removed, then save and exit the file.
```
 #include /etc/ipsec.d/*.conf
```
5. Create a new file at /etc/ipsec.d/aws.conf if doesn't already exist, and then open it. Append the following configuration to the end in the file.
> [!IMPORTANT]
> Remove auth=esp if it’s there and replace fields like left, leftid, leftsubnet, right, rightsubnet with corresponding values. Left means on-premises customer side of the network and right means AWS side of the network.
```
conn Tunnel1	
	authby=secret
	auto=start
	left=%defaultroute
	leftid=x.x.x.x
	right=x.x.x.x
	type=tunnel
	ikelifetime=8h
	keylife=1h
	phase2alg=aes128-sha1;modp1024
	ike=aes128-sha1;modp1024
	keyingtries=%forever
	keyexchange=ike
	leftsubnet=<LOCAL NETWORK>
	rightsubnet=<REMOTE NETWORK>
	dpddelay=10
	dpdtimeout=30
	dpdaction=restart_by_peer
```
6. Create a new file at /etc/ipsec.d/aws.secrets if it doesn't already exist, and append this line to the file (be mindful of the spacing!):
```
x.x.x.x x.x.x.x: PSK “xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```
