Loris for Docker Networking 2.0
===============================
Docker networking is fast evolving. There are many options today for
using Linux bridge, port mapping, Open vSwitch for this purpose. See
our [presentation](http://www.slideshare.net/lorispack/docker-networking-101)
on the different options available, and especially
how to network containers across multiple hosts.

We found the need to have a comprehensive mechanism to network all
applications across hosts with isolation through overlay networking.
The *lorispack* software is an early attempt at providing this framework,
without using Spanning Tree Protocol (STP) that can have potential
instability when container network ports flap.

The general idea of *lorispack* is to create a parallel network
dedicated for inter-container communication across hosts, while
leveraging the Linux bridge to provide the basic management connectivity
for the containers over the *eth0* interface. 
The toolkit uses [Open vSwitch](http://openvswitch.org) to provide
connectivity to containers and creates GRE or VxLAN tunnels to allow containers
on different hosts to communicate with each other.

The secondary goal of *lorispack* is to perform *microsegmentation*,
whereby we isolate the network of each pod (i.e., groups of
containers that are allowed to reach each other) from each other. 
This is also referred to as network virtualization.

![docker-networking-2.0.png](http://sdnhub.org/wp-content/uploads/2015/03/docker_vxlan_networking-596x365.png)

# Running
After downloading the loris toolkit and adding that to the execution PATH,
you can run the following commands to manage the networks.

* Install Open vSwitch
```
$ sudo apt-get -y install openvswitch-switch
$ sudo ovs-vsctl --version
```
   Verify that the OpenvSwitch version is greater than 2.0

* Prepare host for container networking
```
$ loris init
```
   This command could simply be added to the /etc/rc.local of your host.
   It creates the Open vSwitch bridges and patch ports.

* Connect localhost to remote hosts in cluster
```
$ loris cluster <list of remote_host_ips>
```
   This command needs to be run on every hosts today. But, we will soon
   update the script to allow executing this globally from a single host.

* Connect container to global pod
```
$ loris connect <container id/name> <desired_ip/mask> <optional:pod_number>
```
   This creates a new interface eth1 for the container. This is private
   network for cross-container communication and cannot be used to access
   external networks. Container MAC address within a pod needs to be unique.

   Instead of specifying the IP, it is possible to say "dhcp"
   and have the system pick IP for that interface from DHCP.

   If pod number is not specified, the system assigns the container
   interface to a global pod. The MAC address needs to be unique within
   this global pod for reachability.

   Once you connect the containers, you will notice that the containers
   will not be able to communicate across pods and will be restricted to
   communicating within their pod.

* Optionally, you can cleanup host configs after container networking
```
$ loris cleanup
```

# Support/discussion forum
http://sdnhub.org/forums/forum/solution-suite/lorispack/
