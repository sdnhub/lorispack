Loris for Docker Networking 2.0
===============================

This software package allows configuring the networking of individual
containers and isolating the network of container groups (i.e., pods).
The toolkit uses [Open vSwitch](http://openvswitch.org) to provide
connectivity to containers and creates VxLAN tunnels to allow containers
on different hosts to communicate with each other.
![docker-networking-2.0.png](https://bitbucket.org/repo/xGGgAk/images/3466984347-docker-networking-2.0.png)

# Running
After downloading the loris toolkit and adding that to the execution PATH,
you can run the following commands to manage the networks.

* Prepare host for container networking
```
#!shell
$ loris init
```

* Connect localhost to remote host
```
#!shell
$ loris patch <remote_host_ip>
```

* Connect container to global pod
```
#!shell
$ loris connect <container id/name> <desired_ip> <pod_number>
```

* Cleanup of host after container networking
```
#!shell
$ loris cleanup
```

# Support/discussion forum
(https://groups.google.com/forum/#!forum/lorispack)
