## what is kubernetes?

came out of google. not an open source version of Borg or Omega

can be shortened to k8s "kates"

jargon

- monolith: massive application w/ all code in a single program
- microservices: services broken into their own apps instead of a monolith

## kubernetes architecture

kubernetes is an orchestrator of microservices apps

made up of masters and nodes

- masters are controlling / orchestrating

- nodes do the work of the app

- we take the app put it in a container => put that into a pod and that gets wrapped in a deploy!
    - this is described in a YAML file

# masters

- known as control planes
- multi-masters are just multiple copies in different failure domains
H/A design:

- 3 is standard
- 5 is an option if you're paranoid
- more than that can take longer for the clusters to reach concensus

never an odd number so that if a failure mode happens, you'll be able to reach a concensus w/ a majority of clusters

in kubernets only one master is ever making changes to the cluster
    - leader and followers

in a hosted platform, you don't get to see the masters. they're set up by the hosting program

# bits

## masters

API server => front end to control the plane
    - everything communicates through this thing
    - exposes RESTFul api consumes JSON or YAmML

Cluster store
    - persists cluster state
    - based on etcd ?

Kube controller manager

    - controller of controllers
    - node controller, deployment controller etc.
    - watch loops => reconciles observed state w/ 

Scheduler

    - watches API server for new work tasks
    - assigns work to cluster nodes

## nodes

generally speaking business logic runs on these

### kubelets

main kubernetes agent that runs on every cluster
windows / linux machine
registers the node w/ the cluster

watches api server for work tasks (new pods assigned to it)
executes pods

reports to master on state of cluster

### container runtime

often docker
supports other containers
builds / starts containers
low level container intelligence

### kube proxy

network brains of the node
makes sure pods gets its own ip address
basic load balancing 


# declarative model and desired state

## declarative model

- we give api server a manifest file that describes an end state (desired state)
- just a description of what we want it to look like

## pod

- what is a pod? a thin wrapper that kubernetes insists every container needs
- shared execution environment
- collection of things an app needs to run
- containers running in a pod share the environment of the pod

if two containers don't need to be tightly coupled, run containers on separate pods and they will communicate over a network

scaling: adding / removing pods!!! not containers

containers in a pod are always connected to the same node

## stable networking

can't rely on pod ips
keeps networking consistent (uses the same ip and dns name) between pods and handles load balancing between pods

labels! very important and powerful
- they tie the service and pods together

## deployments!

- deployment controller watches for new deployments
- implements them
- compares desired state w/ observed and reconciles

## k8s API / API server

- catalog of features / services and def for how it works
- we use ```kubectl``` to speak to the api server


# getting Kubernetes (some quick / easy ways)

k8s in the cloud: linode is a simpler option that GKE but those two are pretty similar in function. 

# working with pods

app deployment workflow

steps:

1. create code
2. build into container image
3. store in repo (dockerhub or something)
4. define it in a kubernetes manifest YAML
5. post it to the api server

# kubernetes service object

## theory