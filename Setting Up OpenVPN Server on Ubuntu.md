# Setting Up OpenVPN Server on Ubuntu
1. Installing and configuring an OpenVPN server manually is not a simple task from my experience. That’s the reason, we will be using a script that lets you set up your own secure OpenVPN server in a matter of seconds.

Before downloading and running the script, note that the script will auto-detect your server’s private IP address. But you need to take note of your server public IP address especially if it is running behind NAT.

To find out your server’s public IP address, run the following 
```
curl ifconfig.me
```
2. Now download the installer script using the curl command-line tool, then make it executable using the chmod command as follows.
```
sudo curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
```
```
sudo chmod +x openvpn-install.sh
```
3. Next, run the executable installer script as shown.
```
sudo bash openvpn-install.sh
```
OutPut
```
Welcome to the OpenVPN installer!
The git repository is available at: https://github.com/angristan/openvpn-install

I need to ask you a few questions before starting the setup.
You can leave the default options and just press enter if you are ok with them.

I need to know the IPv4 address of the network interface you want OpenVPN listening to.
Unless your server is behind NAT, it should be your public IPv4 address.
IP address: 20.216.13.253

Checking for IPv6 connectivity...

Your host does not appear to have IPv6 connectivity.

Do you want to enable IPv6 support (NAT)? [y/n]: n

What port do you want OpenVPN to listen to?
   1) Default: 1194
   2) Custom
   3) Random [49152-65535]
Port choice [1-3]: 1

What protocol do you want OpenVPN to use?
UDP is faster. Unless it is not available, you shouldn't use TCP.
   1) UDP
   2) TCP
Protocol [1-2]: 1

What DNS resolvers do you want to use with the VPN?
   1) Current system resolvers (from /etc/resolv.conf)
   2) Self-hosted DNS Resolver (Unbound)
   3) Cloudflare (Anycast: worldwide)
   4) Quad9 (Anycast: worldwide)
   5) Quad9 uncensored (Anycast: worldwide)
   6) FDN (France)
   7) DNS.WATCH (Germany)
   8) OpenDNS (Anycast: worldwide)
   9) Google (Anycast: worldwide)
   10) Yandex Basic (Russia)
   11) AdGuard DNS (Anycast: worldwide)
   12) NextDNS (Anycast: worldwide)
   13) Custom
DNS [1-12]: 11

Do you want to use compression? It is not recommended since the VORACLE attack makes use of it.
Enable compression? [y/n]: n

Do you want to customize encryption settings?
Unless you know what you're doing, you should stick with the default parameters provided by the script.
Note that whatever you choose, all the choices presented in the script are safe. (Unlike OpenVPN's defaults)
See https://github.com/angristan/openvpn-install#security-and-encryption to learn more.

Customize encryption settings? [y/n]: n

Okay, that was all I needed. We are ready to setup your OpenVPN server now.


Tell me a name for the client.
The name must consist of alphanumeric character. It may also include an underscore or a dash.
Client name: ranavpn

Do you want to protect the configuration file with a password?
(e.g. encrypt the private key with a password)
   1) Add a passwordless client
   2) Use a password for the client
Select an option [1-2]: 2
⚠️ You will be asked for the client password below ⚠️

Note: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/vars
Using SSL: openssl OpenSSL 1.1.1f  31 Mar 2020
Generating an EC private key
writing new private key to '/etc/openvpn/easy-rsa/pki/easy-rsa-48658.qIF9Tb/tmp.DaVC3w'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
Using configuration from /etc/openvpn/easy-rsa/pki/easy-rsa-48658.qIF9Tb/tmp.vEUsfo
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'ranavpn'
Certificate is to be certified until Oct 13 10:35:55 2024 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

Client ranavpn added.

The configuration file has been written to /home/rana/ranavpn.ovpn.
Download the .ovpn file and import it in your OpenVPN client.

```
4. Once the VPN installation process is complete, a client configuration file will be written under the current working directory. This is the file you will use to configure your OpenVPN client as described in the next section.
```
The configuration file has been written to "/home/rana/ranavpn.ovpn"
Download the .ovpn file and import it in your OpenVPN client.
```
5. Next, confirm that the OpenVPN service is up and running by checking its status using the following systemctl command.
```
sudo systemctl start openvpn.service 
sudo systemctl enable openvpn.service 
sudo systemctl status openvpn
```
6. Also, confirm that the OpenVPN daemon is listening on the port you instructed the script to use, using the ss command as shown.

```
sudo ss -tupln | grep openvpn
```
OutPut
```
udp     UNCONN   0        0                0.0.0.0:1194          0.0.0.0:*       users:(("openvpn",pid=48524,fd=7))  
```
7. If you check your network interfaces, a new interface has been created for a VPN tunnel, you can confirm this by using IP command.

```
ip ad
```
