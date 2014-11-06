Loris for Docker Networking 2.0
===============================
Docker networking is fast evolving. There are many options today for
using Linux bridge, port mapping, Open vSwitch for this purpose. We
found the need to have a comprehensive mechanism to network all
applications across hosts with isolation through overlay networking.
The loris software is an earlier attempt at provide this framework.

The loris software package allows configuring the networking of individual
containers and isolating the network of container groups (i.e., pods).
The toolkit uses [Open vSwitch](http://openvswitch.org) to provide
connectivity to containers and creates GRE or VxLAN tunnels to allow containers
on different hosts to communicate with each other.
![docker-networking-2.0.png](http://lorispack.io/wp-content/uploads/2014/11/docker_vxlan_networking.png)

# Running
After downloading the loris toolkit and adding that to the execution PATH,
you can run the following commands to manage the networks.

* Prepare host for container networking
```
$ loris init
```
This command could simply be added to the /etc/rc.local of your host

* Connect localhost to remote hosts in cluster
```
$ loris cluster <list of remote_host_ips>
```
This command needs to be run on every hosts today. But, soon
we update code to allow entering this one a single host.

* Connect container to global pod
```
$ loris connect <container id/name> <desired_ip/mask> <pod_number>
```
This creates a new interface eth1 for the container. This is private
for cross-contain communication and cannot be used to access external
networks.

* Optionally, you can cleanup host configs after container networking
```
$ loris cleanup
```

# Support/discussion forum
(https://groups.google.com/forum/#!forum/lorispack)
