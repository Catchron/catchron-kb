## LXC/LXD Networking

* When we list our containers to view the IPv4 Address assigned to them:<br>
<img src="https://i.imgur.com/RE3RGJv.gif" width="700"/><br>

* The IPs assigned to our containers are associated with the bridge we set up during [lxd init]():
<img src="https://i.imgur.com/QVZYYLn.gif" width="700"/><br>

* The LXC bridge you created during init (lxdbr0) will communicate with a veth (Virtual Ethernet Adapter) pair for each container you create. This veth pair will send packets to its other pair on the container itself<br>
<img src="https://i.imgur.com/I5uucWJ.gif" width="700"/><br>
<img src="https://i.imgur.com/BaCRPvT.gif" width="700"/><br>
In this example we can see our two containers and the two veth pair adapters that were created for them.
