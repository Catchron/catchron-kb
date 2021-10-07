# GKE Fundamentals <img src="https://static-00.iconduck.com/assets.00/google-gke-icon-512x457-q6s0e3iu.png" alt="gke logo" width="80"/>

## Anatomy of a GKE cluster
* Like any  other  cluster - it's a collection of computers to perform a function
* Everything in GKE is an object
* A GKE cluster contains:
 - One ore more masters
 They make decision (like scheduling) and provide the control plane
 - One or more nodes
 In GCP's case these are VMs. They provide  the runtime environment. They also  provide t he resources of the cluster that can be used to run containers
 * Master:<br>
   <img src="https://i.imgur.com/p0128YT.png" alt="gke logo" width="500"/>
 * Node:<br>
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
    * Example yaml file:<br>
    <img src="https://i.imgur.com/JW39gA0.png" alt="gke logo" width="500"/>
    * When you re-apply the same file GKE does not duplicate the object as it is already live
    * If we change the configuration GKE will update the object<br>
    <img src="https://media.giphy.com/media/eq8KmQExIuwsmiYAbV/giphy.gif" width="500"/>


## Replica set
* A ReplicaSet is a GKE object used to manage multiple replicas of pods
* It makes sure that all the pods are identical replicas
* A ReplicaSet  is a GKE object that provides:
  * A stable set of Pod replicas
  * A specified number of pods
  * Logic to ensure availability
* Not recommended to create ReplicaSets on their own - instead we use Deployments
* Example:<br>
We've got a replica set of 3 pods which are all the same. Those pods are split across 3 nodes to better utilize their resources. If one node drops from the cluster, the replica sets will re-create the missing pods in the other nodes to make sure they are exactly 3<br>
<img src="https://media.giphy.com/media/jq3Vxz0NUNbKBxOsJJ/giphy.gif" width="500"/>


## Deployments
* An **object** for logically mapping **Pods** and **ReplicaSets**
* The desired state of a **Deployment** is **enforced** by the **Controller**
* **Deployments** provide logic for **updating**, **rolling back**, and **scaling** deployments
* They also provide **error checking** for rollouts
* Deployments are defined by **yaml** files like any other object
* Example file:<br>
  <img src="https://i.imgur.com/p0dRDAe.png" width="500"/><br>
  * When you update your deployments with new specification, GKE creates a new set of your deployment first. Once  the new update is up - the Deployment deletes the older version:<br>
<img src="https://media.giphy.com/media/Lp4FavzG7nRiThHqqv/giphy.gif" width="500"/>





----------------
* The primary command for interacting with GKE is `kubectl`
* **kubectl** needs to be configured  so it can  authenticate with the cluster:<br>
`gcloud container clusters get-credentials [cluster name] --zone=[cluster zone]`

* To get info for certain components:<br>
`kubectl get`<br>
Examples:<br>
`kubectl get nodes`<br>
`kubectl get pods`<br>
`kubectl get services`<br>

* Dynamically  create deployment:<br>
`kubectl create deployment [deployment name] --image [deployment image]`

* Exposing deployment to the outside world using a **service**:<br>
`kubectl expose deployment [deployment name] --port=[port number] --type=[deployment type]`

* To apply an object file declaratively:<br>
`kubectl apply -f [file name].yaml` - with this we declare that we want a new pod. The `-f` options means "local file"

* To delete the declared object file:<br>
`kubectl delete -f [file name].yaml`

* To expose a pod without a **service** we need  to  port forward our pod to the command shell public port for web preview which is **8080** :<br>
`kubectl port-forward [pod name] 8080:80`

* To attach to a container with a command line terminal:<br>
`kubectl exec -it [pod name] -c [container name] -- /bin/bash`




------------------
Video to GIF:<br>
https://ezgif.com/video-to-gif<br>
Image uploads<br>
https://imgur.com/user/Catchron/posts<br>
GIF uploads<br>
https://giphy.com/channel/Nagrimor
Markdown Syntax<br>
https://www.markdownguide.org/basic-syntax/#line-break-best-practices
