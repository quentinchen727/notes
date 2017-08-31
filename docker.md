# Docker Swarm
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
