# GKE Fundamentals <img src="https://static-00.iconduck.com/assets.00/google-gke-icon-512x457-q6s0e3iu.png" alt="gke logo" width="80"/>

## Anatomy of a GKE cluster
* Like any  other  cluster - it's a collection of computers to perform a function
* Everything in GKE is an object
* A GKE cluster contains:
 - One ore more masters
 They make decision (like scheduling) and provide the control plane
 - One or more nodes
 In GCP's case these are VMs. They provide  the runtime environment. They also  provide t he resources of the cluster that can be used to run containers
 * Master:

    <img src="https://i.imgur.com/p0128YT.png" alt="gke logo" width="500"/>
 * Node:

     <img src="https://i.imgur.com/aw61EfE.png" alt="gke logo" width="500"/>


 ## Pods
 * A Pod is a **logical application-centric unit** that shares network and storage configuration
 * Pods contain 1 or more tightly coupled containers
 * Example design pattern for a pod can be:
  - A **monolith** and a **web-server**
  - An **application** and a **container that proxies connection to a database** (aka database proxy)
  - An **application** and an **adapter** to process its output
  - Pods should serve a single application
 * GKE does not deploy containers on its own - puts them in pods
 * Pods are the smallest deployable units in  GKE. You cannot deploy a container on its own - you need to put it in a pod.
 * Many pods will have a single container
 * When containers run in the same pod they use the same environment. They share a local IP and can talk to each other as if they are on the same local host. They also have the same access to volumes(storage)
### Creating pods
* You can create pods **Dynamically** using the kubectl command in cloud shell
* The preferred way is to create pods **Declaratively**
    * This means that we create a specification file that declares to GKE the objects that we want to create
    * These specification files are usually **yaml** files
    * GKE tries to make the **live** state match the state we've declared in the configuration file
    * Example yaml file:
    <img src="https://i.imgur.com/JW39gA0.png" alt="gke logo" width="500"/>
    
    * When you re-apply the same file GKE does not duplicate the object as it is already live
    * If we change the configuration GKE will update the object
    * <img src="https://media.giphy.com/media/eq8KmQExIuwsmiYAbV/giphy.gif" alt="gke logo" width="500"/>


## Replica set
* A ReplicaSet is a GKE object used to manage multiple replicas of pods
* It makes sure that all the pods are identical replicas
* A ReplicaSet  is a GKE object that provides:
  * A stable set of Pod replicas
  * A specified number of pods
  * Logic to ensure availability
* Not recommended to create ReplicaSets on their own - instead we use Deployments

## Deployments





----------------
* The primary command for interacting with GKE is `kubectl`
kubectl needs to be configured  so it can  authenticate with the cluster:
 `gcloud container clusters get-credentials [cluster name] --zone=[cluster zone]`
* To get info for certain components:
 `kubectl get`
 Examples:
`kubectl get nodes`
`kubectl get pods`
`kubectl get services`
* Dynamically  create deployment:
`kubectl create deployment [deployment name] --image [deployment image]`
* Exposing deployment to the outside world using a **service** :  
`kubectl expose deployment [deployment name] --port=[port number] --type=[deployment type]`
* To apply an object file declaratively:
`kubectl apply -f [file name].yaml` - with this we declare that we want a new pod. The `-f` options means "local file"
* To delete the declared object file:
`kubectl delete -f [file name].yaml`
* To expose a pod without a **service** we need  to  port forward our pod to the command shell public port for web preview which is **8080** :
`kubectl port-forward [pod name] 8080:80`
* To attach to a container with a command line terminal:
`kubectl exec -it [pod name] -c [container name] -- /bin/bash`
