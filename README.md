# wireguard-example

see https://www.wireguard.com/quickstart/

and https://gist.github.com/chrisswanda/88ade75fc463dcf964c6411d1e9b20f4

use gen.sh to generate public and private keys in the current directory


/etc/wg0.conf on a server (with static IP):

```
## wg0.conf on server

[Interface]
# server
Address = 10.88.88.1/24 # you pick the ip. subnet mask of /24 gives ~200 addresses, you can change that if you want
ListenPort = 51820 # you pick this
PrivateKey = 1234123412341234123412341234123412341234123= # you use wg to generate this
SaveConfig = false

[Peer]
# laptop
PublicKey = 8765876587658765876587658765876587658765876= # you use wg and the 5678 private key to generate this
AllowedIPs = 10.88.88.2/32 # you pick the ip, it has to match the subnet mask selected above. here /32 is mandatory
PersistentKeepalive = 5 # necessary because laptop is roaming and generally behind NAT
```



/etc/wg0.conf on a laptop (roaming IP):

```
## wg0.conf on laptop

[Interface]
# laptop
Address = 10.88.88.2/24 # has to match the laptop allowed ips above, and the subnet has to match above as well
PrivateKey = 5678567856785678567856785678567856785678567= # you use wg to generate this
SaveConfig = false

[Peer]
# server
PublicKey = 4321432143214321432143214321432143214321432= # you use wg and the 1234 private key to generate this
AllowedIPs = 10.88.88.1/32 # has to match the server address above. here /32 is mandatory
Endpoint = 1.1.1.1:51820 # ip should be the actual ip of the server (curl zx2c4.com/ip); port should be the ListenPort above
PersistentKeepalive = 5
```

don't forget to open port 51820 for UDP on your cloud server!



