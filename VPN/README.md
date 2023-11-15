# Site to Site VPN setup on Amazon Linux 2023 using Libreswan

### Following steps needs to be performed on the Customer side of the VPN server for connecting to AWS VGW. This guide assumes that you have already configured AWS side of the network and created Site-to-Site VPN connection in AWS. You also need to download the VPN configuration file before you proceed.

The default repository for Amazon Linux 2023 does not include openswan.
Also, openswan is already a non-maintained package and should not be used even if available.
https://docs.aws.amazon.com/linux/al2023/release-notes/all-packages-AL2023.1.html

It is recommended to use libreswan, which is created by a fork of openswan.
To install libreswan, please add the fedora repository with the following command.

1. Add repository for libreswan package
```
sudo vi /etc/yum.repos.d/fedora.repo
```

The repositories to be added are as follows.
```
[fedora]
name=Fedora 36 - $basearch
#baseurl=http://download.example/pub/fedora/linux/releases/36/Everything/$basearch/os/
metalink=https://mirrors.fedoraproject.org/metalink?repo=fedora-36&arch=$basearch
enabled=0
countme=1
metadata_expire=7d
repo_gpgcheck=0
type=rpm
gpgcheck=1
gpgkey=https://getfedora.org/static/fedora.gpg
skip_if_unavailable=False
```
2. Once added, execute the following command.
```
sudo dnf --enablerepo=fedora install libreswan
```
3.   Open /etc/sysctl.conf and ensure that its values match the following:
```
net.ipv4.ip_forward = 1
net.ipv4.conf.default.rp_filter = 0
net.ipv4.conf.default.accept_source_route = 0
```
2. Apply the changes in step 1 by executing the command:
```
sudo sysctl -p
```
4. Open /etc/ipsec.conf and look for the line below. Ensure that the # in front of the line has been removed, then save and exit the file.
```
 #include /etc/ipsec.d/*.conf
```
5. Next all the steps are provided in the VPN Configuration file that you download from AWS. Make sure to replace the right values as described in the following section. Before you proceed with the following steps, download the VPN configuration file.
   
7. Create a new file at /etc/ipsec.d/aws.conf if doesn't already exist, and then open it. Append the following configuration to the end in the file.
> [!IMPORTANT]
> Replace fields like left, leftid, leftsubnet, right, rightsubnet with corresponding values. Left means on-premises customer side of the network and right means AWS side of the network.  
> **Remove auth=esp if it’s there**  
> **phase2alg=aes_gcm**  
> **ike=aes256-sha1**  
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
	phase2alg=aes_gcm
	ike=aes256-sha1
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
7. Start the ipsec service
```
sudo systemctl start ipsec.service
```
