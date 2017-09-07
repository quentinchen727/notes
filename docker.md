# Docker Swarm
Configure proxy for each node to be able to download images.
## Docker Swarm Key Concept
1. node: a docker engine instance participating in the swarm
    - manager node: can be configured to run manager tasks only
    - worker node
2. service: definition of the tasks to execute on the manager or worker nodes
    - global service: one task for the service on each node
    - replicated service: manager distributes a specific number of replica tasks among nodes based on the set scale
3. task: carries a container and the commands to run inside the containter, atomic scheduling unit.
### Load balancing
The swarm manager uses `ingress load balancing` to expose the service to the swarm.
Swarm manager can automatically assign the service a `PublishedPort` or you can configure it.
Swarm mode has a internal DNS component that automatically assign each serive in the swarm a DNS entry, in
round-robin manner(?). Manager uses internal load balancing to distribute requests among services within the cluster
based upon DNS name of the service.
External: PublishedPort;
Internal: DNS name on internal DNS server

## Swarm Tutorial
1. Create a swarm
docker swarm init --advertise-add <MANAGER_IP>
docker info // view current state of the swarm
docker node ls  // view informatio about nodes
2. join a swarm
docker swarm join-token worker // print out command to join
docker swarm join --token blabla...
3. deploy a service to the swarm
docker service create --replicas 1 --name foo alpine ping docker.com
docker service inspect foo
docker service inspect --pretty foo
dokcer service ps foo
4. scale the service in the swarm
docker service scale <service_id>=<number>
5. Delete the serivce running on the swarm
docker service rm <service_id>
6. apply rolling updates to a service
docker service create --replicas 3 --name redis --update-delay 10s redis:3.0.6
docker service update --image reids:3.0.7 reids
7. drain a node on the swarm
docker node update --availability drain <node_name_or_id>
docker node update --availability active <node_name_or_id>
8. use swarm mode routing mesh
In order to use the ingress network in the swarm, you need to have the following ports open between
swarm nodes before you enable swarm mode:
    - Port 7946: TCP/UDP for container network discovery
    - Port 4789: UDP for container ingress network
    1. publish a port for a service:
        docker service create --name <service-name> --publish <PublishedPort>:<Target-port> <image>
        docker service update --publish-add <PublishedPort>:<Target-port> <service>


## HOw nodes work
external load balancer: HAProxy

1. Manager nodes: an N manager cluster will tolerate the loss of at most (N-1)/2
                  A maximum of seven manager nodes for a swarm.
    Using a `raft` implementation, the manager maintain a consistent internal state of the entire swarm and all the services running on it.
2. Work nodes: exectute containers.
    By default, all managers are also workers.
    availability: Drain, Active.
    docker node update --availability drain <node_name>
    docker node promote // promote to a manager

## How services work
service: an image for a microservice.
    when you create a service, you specify image and command,
    and options: the published port,
                 an overlay network for the service to connect to other services in the swarm;
                 CPU and memory limits and reservations;
                 a rolling update policy;
                 the number of replicas;
    A container is an isolated process and each task is analogous to a "slot" where the scheduler puts a container.
    A task is a one-directional mechanism: assigned-->prepared-->runnign,tec.
    replicas mode: deployed based on scale number;
    global mode: one on each node

## Manager swarm security with public key infrastructure[PKI]

## node status:
    1. active
    2. pause
    3. drain
    service status: pending, stopped??

## PKI in docker
By default, the manager node generates a new root Certificate Authority(CA) along with a key pair. Root certificate are self signed.

dockerd -D // to turn on debug
netstat -tulpn | grep docker  // check listening port for docker
nmap -p2337 <ip_addr>  // check availability of port on ip_addr

## Deploy services to a swarm
### Publish a service's port
docker service create --name my_web --replicas 3 --publish 8080:80 nginx
docker service create --mode global --publish mode=host,target=80,published=8080 // map port to each local node.
### connect the service to an overlay network
docker network create --drive overlay my-network
docker network create --network my-network nginx
docker network update --network-add my-network my-web
                      --network-rm
### control service scale and placement
docker service create --name m-web --replicas 3 --mode global alpine top
You can create constraint for service to run on some specific nodes.
specify service placement preference -placement-pref 'spread=node.labels.datacenter'
--placement-pref spread=engine.labels.<labelname>

###configure a service's update service
--rollback

### Give a service access to volumes or bind mounts
Performance-wise, you should avoid writing important data directly into a container's writable layer, instad using data volumes or bind mounts.
1. data volumes:
docker service create --mount src=<volume name>,dst=<container_path>  //using existing volumes
--mount type=volume,src=?,dst=>,volume-drive=?,volume-opt=?  //create volume on the fly
2. bind volumes:
They are file system paths from the host where the scheduler deploys the container for the task.
--mount type=bind,src=<host path>,dst=<container-path>,readonly
If you bind mount a host path into your service's containers, the path must exist on every swarm node.

## Store configuration data using Docker configs
docker config create homepage index.html
docker service create --config src=homepage,target='/foo/foo.html'
docker service update --config-rm site.conf --config-add srouce=foo.conf,target=/etc/foo.conf

## Manager swarm service networks
two different kinds of traffic:
1. control and managerment plane traffic, alway encrypted.
2. application data plane traffic, including container traffic and traffic to and from external clients.
Three networks:
1. Overlay: manage communications among the Docker Daemons participating in the swarm.
2. Ingress: a special overlay network that facilitates load balancing among a service's node. When any swarm mode node receives a request on a published port, it hands that request off to a module called IPVS, which keeps track of all the IP addresses participating in that service, slects one of them, and routes the requests to it, over the ingress network.
3. docker_gwbridge: a bridge network that connects the overlay networks to an individual Daemon's physical network. It is created automatically when you initialize or join a swarm.
Firewall considertions:
    1. Port 7946: TCP/UDP for container network discovery.
    2. Port 4789 UDP for container overlay network.
create an overlay network:
    docker network create --drive overlay my-network
    The network's subnet and gateway are dynamically configured when a service connects to the network for the first time.
Customize an overlay network:
    docker network create --driver overlay --help
Add a service to an overlay network:
    docker service create --name my-web --network my-network nginx
Configure service discovery:
    Service discovery is the mechanism Docker uses to route a request from your service's external clients to an individual swarm node, withou the client knowing how many nodes are participating in the service or their IPs or ports.
    Two types:
        1. Virtual IP. By default.
        2. DNS round robin. --endpoint-mode dnsrr
Customize the ingress network
    docker network create -d overlay --ingress --subnet=10.0.0.0/16 --gateway=10.0.0.1 my-ingress
You can have only one ingress network.

Customize the docker_gwbridge
docker_gwbridge is a virtual bridge that connects the overlay networks(including ingress) to an individual Dockre daemon's physical network. It is created automatically when initializing and join a swarm.
    brctl show docker_gwbridge
    brctl delbr docker_gwbridge
    docker network create --subnet 10.11.0.0/16 --opt com.docker.network.bridge.name=docker_gwbridge --opt com.docker.network.bridge.enable_idcc=false docker_gwbridge

Use a separate interface for control and data traffic
Traffic about joining, leaving, and managing the swarm will be sent over the --advertise-add interface, and traffic among a service containers will be sent over --datapth-addr interface.
    docker swarm init --advertise-addr 1.1.1.1 --datapath-addr 1.1.1.2
    docker swarm join --advertise-addr 2.2.2.2 --datapath-addr 2.2.2.3

## Administrator and maintain a swarm of Docker engines
Swarm manager nodes use the `Rast consensus Algorithm` to manage the swarm state. Raft requires a majority of managers, called the quorum, to agree on proposed updates to the swarm, such as node additions or removal.
If a swarm loses the quorum of managers, swarm tasks on existing worker nodes can carry on. But swarm nodes cannot be added, updated, or removed and new or existing tasks cannot be started, stopped, moved, or updated.
An odd number of N manager nodes tolerate (N-1)/2 managers loss, at most 5 for performance.
A node can only use a node ID once to join the swarm.
    docker node demote <node>
    docker node rm <node>
    docker swarm join
Swarm state and logs in /var/lib/docker/swarm/
Properties inherent to distributed systems:
1. agreement on vlaues in a fault tolerant system.
2. mutual exclusion through the leader election process.
3. cluster membership management
4. globally consistent object sequencing and CAS (compare-and swap) primitive.

## Docker container networking
### default networks
The `bridge` network represents the `docker0` network present in all Docker installations. and this is the default one for all containers.
The `none` network: containers in it lacks a network interface.
The `host` network: the container in it is on the host's network stack. Ther eis no isolation between the host machine and the container.
`none` and `host` are not direclty configurable.
Containers connected to the default `bridge` network can communicate with each other by IP.
Docker does not support automatic service discovery on the default bridge network. if needing ip resolution by container name, use user-deind networks instead.
cat /etc/hosts to see the hostnames and ip addresses the container recognizes.
### User-defined networks
User user-defined bridge networks to control container accessibility and also enable automatic DNS resolution of container names to IP addresses.
When a container is connnected to multiple networks, its external connectivity is provided via the first non-internal network, in lexial order.
1. Bridge networks
docker network create --driver bridge isolated_nw
Containers in isolated_nw can communicate with each other, but isolated from outside.
A `bridge` network is useful where you want to run a relatively small network on a single host.
2. docker_gwbridge
created when: 
            - in swarm mode
            - when none of a container's networks can provides external connectivity, Docker connects the container to the docker_gwbridge in addition to the container's other networks.
You can create it by yourself and customize it.
it is always present when you use `overlay` networks.
3. Overlay networks in swarm mode
docker network create --drive overlay 
docker service create --network
Only swarm services can connect to overlay networks.
4. Embedded DNS server
Docker Daemon runs an embedded DNS server which provides DNS resolution among containers connected to the same user-defined network. If the embedded DNS server cannot resolve the request, it will be forwarded to any external DNS server configured for the container.
In container, only the embeded DNS 127.0.0.1 is listed in /etc/resolv.conf
6. Expose and publish ports
In dockerfile, exposing ports is just a way of documenting, not actually opening it, while publishing to a port on host is not allowed in dockerfile.
7. Docker and iptables
Linux hosts use a kernel module called `iptables` to manager access to network devices, including routing, prot forwarding, NAT, and etc. Docker modifies iptables rules.

## Use compose in production
docker-compose -f foo1.yml -f foo2.yml // multi-layer compose file
docker-compose build web; docker-compose --no-deps -d web // deploying changs

## local storage on the docker host
    cat /var/lib/docker/image/devicemapper/repository.json | python -mjson.tool
    cat /var/lib/docker/image/devicemapper/respository.json | jq
    docker images // locally
    docker search // remotely on internet

## miscelleanous
Cannot publish port in images, as images are static while port mappings are dynamic.

Ports vs Expose in docker-compose
ports: expose ports to internal and external networks.
expose: without publishing them to the host machine - they'll only be accessible to linked services.
