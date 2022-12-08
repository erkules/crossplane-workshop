# Checking Prerequisites: Crossplane workshop

Hi 👋

Let's make sure we've got everything.

# Accessing a Kubernetes Cluster

* It is recommended to have (cluster-admin) access to a K8s cluster
* A local one like kind, k3d, Docker Destop etc. is totally fine
* ☝️ If you just want to listen, that's fine too ☝️
* ☝️ Just keep in mind we got some HandsOn stuff ☝️
* `kubectl`
* `git`
* `helm`

# Cloud Credentials

* Credentials for AWS, Digitalocean or Azure
* One is enough
* More than one is also fine
* ☝️ This ist also optional, as we repeat only stuff we did with our local Kubernetes resources ☝️
# Crossplane

The cloud native control plane framework

GitRepo https://github.com/erkules/crossplane-workshop

# Attention

* I just started with Asciidoc 
* Missed to create Slides
* Some topics are mentioned (too) often
* Please don't hurt me


# Let's socialize

* Why are you here?
* What do you expect?

# The Cloud Native Control Plane Framework

* The cloud native control plane framework
* Universal control plane

# Control Plane?

* We know control planes
* Hey we know Kubernetes .. so?!!!
* It is about getting/managing additional resources
* Even Crossplane ist much more then managing 'cloud' resources
* It is even something else
* But we know Crossplane for accessing 'cloud' resources

# Recap

* Think about Crossplane like a control plane for everything
* Giving you some gluecode CRDs on top 

# Disclaimer

* I'm still critical regarding Crossplane
* Hope it is fine to have a critical approach 🤔



# 
# Basics ./. Managed Resource

* It is all about managing `Managed Resources`
* Like in Kubernetes
* Not only cloud
* Everything with an API :)
* This is why we use API- and Cloud-Resources/CRDs alternately

# So following is the same:

* API resources
* Cloud resources
* Cloud [resource] CRD


# Everything is a Kubernetes Resource

* As our well known CRDs \o/
* So we manage them like i.e. Deployments
* Having i.e. a control loop
* Labels
* Spec
* Status ...

# Provider

* Later we install Providers 
* Getting Cloud/API specific CRDs

i.e.

* Kubernetes
* AWS
* Digitalocean
* Azure 
* ArgoCD
* Helm
* ...

# Kubernetes Workflow

* Declaring  API resources
* Reconciliation (looping) API resources
* Like we know
* So `kubectl` is still our friend <3

# 🤔

* So Crossplane (Providers) offers something like Terraform?!
* Jain 🇩🇪
* It is declarative (Kubernetes way)
* Crossplane offers something on top of the Cloud Resources (later)
* Pun: Crossplane switches to Terraform/Jit Providers `/o\`


# Crossplane

The cloud native control plane framework

(With Kubernetes you know at least one control plane 😇)
# More than offering CRDs for API-resources

* Managing AWS Resources without Crossplane -> https://aws-controllers-k8s.github.io/community/
* A lot like this exsists
* Crossplane is more
* CRDs on top of the API/Cloud-CRDs
* So it leverages Kubernetes

# Idee of Crossplane (sneak peak)

* So Crossplane offers CRDs on top of (Cloud/API) CRDs
* This is the basic idea of Crossplane
* Hint: Kube-bind -> https://github.com/kube-bind/ (replacing providers)
* Divide between Consumers and Infra

# Dividing Consumers/Dev and Infra/Ops😱? 

* Yes!!
* Infra packages the Api/Cloud CRDs [into Compositions]
* Infra masquerades them behind a CompositeResourceDefinition
* Dev   consumes only the CompositeResourceDefinition
* The CompositeResourceDefinition hides the complexity `\o/`

# Rephrasing Infra

* Maybe think about the Infra team as your cloud provider

image::mindblow.gif[]


# Crossplane Objects (best off)

* CompositeResourceDefinition (XRD)
* Composite Resource (XR)
* Claim  (XRD)
* Composition 

----
----

* Provider
* ProviderConfig

## Composition

* Composite Resources are opinionated Kubernetes Resources
* Instead of using 'Managed Resources' directly
* A Composition creates a bundle of 'Managed Resources'
* Think about a Helm Chart bundling a lot of Resources

## Spaghetti code

* Just get the idea. 
* We are repeating it anyway

----
kind: compostition
version: apiextensions.crossplane.io/v1
metadata:
  name: jupp
  labels:
    crossplane.io/xrd: newworld.my.own.stuff
    provider:          aws
...
spec:
  compositeTypeRef:                       <<-- XRD reference
    apiextensions: my.own.stuff/v1
    kind:          newworld
  resources:                               the bundle of resources
  - name: databae
    base:
      kind:       RDSInstance
      apiVersion: database.aws.crossplane.io/v1beta1
      ...
  - name: 
    base:
      kind:       ELB
      apiVersion: apiVersion: elasticloadbalancing.aws.crossplane.io/v1alpha1
      ...
----


## CompositeResourceDefinition (XRD)

* XRDs are opinionated CRDs for a Composition (XR)
* So follows [OpenAPIv3](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/#specifying-a-structural-schema) (like "all" CRDs)
* It exposes the Composition
* It exposes the Composition configuration
* Again think about Helm and the `values.yaml`
* But Helm as CRD (Helmoperator)

## Spaghetti code

* Just get the idea. 
* We are repeating it anyway

----
apiVersion: apiextensions.crossplane.io/v1
kind: CompositeResourceDefinition
metadata:
  name:  newworld.my.own.stuff
spec:
  group: my.own.stuff    
  names:  newworld
    kind:
  claimNames:
    kind newworld:
  versions: <<-- Standard openAPVv3 Schema
----

Attention: The Composition and the XRD "describe" the same Resource

Erkan: Explain the "Offering Concept" (of Compositions) in Crossplane.



## Pure K8s?!

By any chance start playing/repeating XRD/CRD

Check the `xrdcrd/` in the Repo  directory

## RBAC

* In the end the best feature :)
* As CompositeResourceDefinition (XRD) is an CRD
* Limit consumers to only (some) XRDs only
* Instead of giving cloud access away
* Again: Also hiding complexity
* Think about the Helmoperator


# Picture (simplified)

* mypaint!!
* Claim of 
* Composite Resorce Definition (XDR)
* Generates an Composite Resource (XR)
* Configured by the Composition
* The XR manages the Managed Resources (MR)
* Works with multiple Providers (XR(D) 1:n Compostions)

Using something like  (in your Claim XR)

----
compositionSelector
  machtLabels:
    provider: lala
----

* Using 

# Getting the Architecture (K8s-way)

* This is an idea I asked Crossplane to put into there docs :)
* Compare it to Operators
* So there is an Operator and a CustomResourceDefinition

Crossplane

* Has a opinionated CRD => XRD
* Instead of Operator we've got Compositions
* They just configure a specific Controller (kinda)

# Missing: (later)

* Patches  (passing configuration from XRD -> XR)

# Crossplane and DevOps

* The Infra Team creates/manages the XRDs
* The Devs consume the XRDs

# Lobhudelei

* Reconciliation Loop all the time
* All as Code
* GitOps (aka everything is i Git)
* Shifting Left (to the Devs)
* Giving Complexity to the Ops aka Infra


# Installing Crossplane

We just use Helm to install the framework:

----
kubectl api-resources --api-group=apiextensions.crossplane.io
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
helm upgrade --install crossplane --create-namespace -n crossplane-system  crossplane-stable/crossplane --wait
kubectl api-resources --api-group=apiextensions.crossplane.io
----

# Nice ShortCuts

* Get all Crossplane related Resources
* Yes Crossplane creates `Categories`
* "Now" you know what `all` is

----
kubectl get crossplane
----

* Get all managed resources
* Should be empty

----
kubectl get managed
----

# Categories?

* Check other `Categories`

:)


----
kubectl get crd -o custom-columns=NAME:.metadata.name,CATEGORIES:.spec.names.categories
----

# Provider

* Our Crossplane has no resources to access
* For these wie need to access our backends
* Using a Provider


# So a Provider is:

* Extends Crossplane with new Managed Resource Types
* CRDs for Cloud Resources
* Talks with the Cloud-Api
* Needs credentials (ProviderConfig)
* Installs a Pod
* [List of Providers](https://github.com/crossplane-contrib)
* [List of Providers](https://github.com/upbound)               ☝️ Terraform ☝️
* [List of Providers](https://marketplace.upbound.io/providers) ☝️ Terraform ☝️

# ProviderConfig

* While Provider install the ProviderControlPlane
* ProviderConfig configures the ProviderControlPlane
* Most likely providing Credentials ☝️
# Kubernetes Provider

* Manage Kubernetes Ressources using Crossplane
* It doesn't need to be Cloud
* [Repo](https://github.com/crossplane-contrib/provider-kubernetes)

# Idea

* Using Provider CRDs
* Create multi-bundle Compositions
* Create XRD

# Specials

* The Kubernetes Providers has some special features
* We will ignore this for the sake to focus on Crossplane core features.

## So we miss

* Install order
* Uninstall order
* Resource Management Policy
* Handling unmanaged resources

## But why a Kubernetes Provider?

* Maybe you like the (missing) features
* A way to integrate "vanilla" Kubernetes
* -> Multi-provider Deployments
* something else?

# Installation

Again create/install a Provider

----
kubectl apply -f provider-kubernetes/provider.yaml    --wait   
kubectl -n crossplane-system  get pods
----

* Provide cluster-admin privileges to the ServiceAccount 
* (We could also provide a kubeconfig (remote accessing a K8s))

Provide cluster-admin privileges

----
SA=$(kubectl -n crossplane-system get sa -o name | grep provider-kubernetes | sed -e 's|serviceaccount\/|crossplane-system:|g')
kubectl create clusterrolebinding provider-kubernetes-admin-binding --clusterrole cluster-admin --serviceaccount="${SA}"
kubectl apply -f provider-kubernetes/ProviderConfig.yaml
----

# Let's see

----
$ kubectl api-resources --api-group=kubernetes.crossplane.io
NAME                   SHORTNAMES   APIVERSION                          NAMESPACED   KIND
objects                             kubernetes.crossplane.io/v1alpha1   false        Object
providerconfigs                     kubernetes.crossplane.io/v1alpha1   false        ProviderConfig
providerconfigusages                kubernetes.crossplane.io/v1alpha1   false        ProviderConfigUsage
----

So that's all

# Objects

* This is straight forward
* We putting a K8s-Object into our Crossplane-Object
* That's it

----
apiVersion: kubernetes.crossplane.io/v1alpha1
kind: Object
metadata:
  name: sample-namespace
spec:
  forProvider:
    manifest:
      apiVersion:  ...  Put your K8s-object
        ..............  in here 👍️👍️👍️👍️

  providerConfigRef:
    name: kubernetes-provider
----

# Simple Example

----
kubectl apply -f provider-kubernetes/simple.pod.yaml
kubectl get object.kubernetes.crossplane.io/kubernetes-provider-pod
kubectl get pods
----

# Reconciliation

Let's delete the Pod

Let's run on window/pane with

----
watch kubectl get pods
----

then:

----
kubectl delete Pod kubernetes-provider-pod
----

We have something like

* ReplicaSet
* ArgoCD/Flux
* \o/

# Multi-Resource Compositions

* This is no deep dive into the Kubernetes provider
* It just - also - works without having a cloud provider access
* Simplifies the Workshop

# Why are the examples so simple

* To get the idea
* We could use complex Objects
* But for what?
* But there is another Problem
* Managing komplex K8s-resources inside an Object
* => Run your own templating system

# 

----
kubectl delete -f provider-kubernetes/twopods.yaml
----

# Composition with 2 MR

* Check `provider-kubernetes/twopods-composition.yaml`
* Of course our XRD is still missing
* Apply it

----
kubectl apply -f provider-kubernetes/twopods-composition.yaml
kubectl get composition.apiextensions.crossplane.io/xtwopods.acme.example.org
----


# Let's create the XRD

Check and apply 

---
kubectl api-resources --api-group=acme.example.org
kubectl apply -f provider-kubernetes/twopods-xrd.yaml
kubectl api-resources --api-group=acme.example.org
---

# Claim

* Claims are namespaced resources
* Check and apply 

----
kubectl        create ns abc
kubectl -n abc get claim
kubectl -n abc apply -f provider-kubernetes/twopods.yaml
kubectl -n abc get claim
----

# Let's discuss 

image::discussion.gif[]


# Aufräumen

----
kubectl apply -f provider-kubernetes/twopods.yaml
----

# Ideen

##
Anfangs ganz einfach. 
Wir exposen nur die Replicas und version
mit HPA, Service i.e.

##

Ein XRD-XR machen mit einem small/medium/large mapping (Patches) 
bei dem sich die replicas und resources ändern


# Delete an Deployment
# Provider Helm

Managing Helm Releases

# Idea

* Use the Provider CRDs
* Create XRD
* Create Composition

# Installation

We need to install the Provider

----
kubectl apply -f provider-helm/provider.yaml
----

Check the new Pod

----
kubectl -n crossplane-system get pods
----

# New CRDs

----
$ kubectl api-resources --api-group=helm.crossplane.io
NAME                   SHORTNAMES   APIVERSION                   NAMESPACED   KIND
providerconfigs                     helm.crossplane.io/v1beta1   false        ProviderConfig
providerconfigusages                helm.crossplane.io/v1beta1   false        ProviderConfigUsage
releases                            helm.crossplane.io/v1beta1   false        Release
----

# ProviderConfig

We need to tell the Pod/Provider which Credentials to use

Lets empower the ServiceAccount the helm-provider runs with:

----
SA=$(kubectl -n crossplane-system get sa -o name | grep provider-helm | sed -e 's|serviceaccount\/|crossplane-system:|g')
kubectl create clusterrolebinding provider-helm-admin-binding --clusterrole cluster-admin --serviceaccount="${SA}"
----


# Applying ProviderConfig

----
kubectl apply -f provider-helm/providerconfig.yaml
----

# Let's create a HelmRelease

* This example uses directly the release object
* Nothing from the Crossplane "overhead" is used

----
kubectl apply -f kafka-release.yaml --wait
kubectl get releases.helm.crossplane.io
----

# Pro Tip

Get all Resorces managed by Crossplane

----
kubectl get managed
----


# New Columns

----
$ kubectl get releases.helm.crossplane.io 
NAME            CHART   VERSION   SYNCED   READY   STATE      REVISION   DESCRIPTION        AGE
kafka-example   kafka   19.1.4    True     True    deployed   1          Install complete   4m36s
----

SYNCED tells us about the connection to the API 

# Reconciliation Loop

* We got a helm release

----
helm -n kafka ls
----

Deleting the Helm Release should trigger the Controller


----
helm -n kafka delete kafka-example
----

Check it out :)

# Getting Crossplane on the Road

* We start with a simple example
* Using the release CRD
* We would be to utilise all the `values.yaml`

# Changing Hats

As a Consumer

* I want to use Kafka
* In a simple way

As an Infra/Ops

* I want to provide a Kafka API/CRD
* Hiding complexity
* Reducing 'attack' vectors

# The Idea

* Create an XRD kafka.example.org
* Having only replicas configurable
* Create the matching XR
* Have fun

# Cleaning Woman

----
kubectl delete -f kafka-release.yaml 
----

image::dead-men-dont-wear-plaid-cleaning-woman.gif[]

# We create the XRD 

* Lets start with the Consumer side
* kafka.acme.example.org
* Lets use `anzahl` to define the number of replicas

----
apiVersion: apiextensions.crossplane.io/v1
kind: CompositeResourceDefinition
metadata:
  name:  kafka.acme.example.org
spec:
  group: acme.example.org
  names:  kafka
    kind:
  versions: <<-- Standard openAPVv3 Schema
----


# XRD

---
kubectl apply -f provider-helm/xrd.yaml
kubectl get xrd
---

Let's have a look into the file

# Compostion

---
kubectl apply -f provider-helm/composition.yaml
kubectl get compostions
---

Let's have a look into the file

# Compostion Questions

* How ist the Compostion connected to the XRD
* 


# Recap

* We worked with Managed Resources
* But that's not the way!

This ist the Way:

* Compostions
* XRDs

Why?



# Idea

* Use the Provider CRDs
* Create XRD
* Create Composition

# Reminder

* We could use other/additional ProviderConfig
* Deploying to remote Cluster
* 🐘 in the room ... GitOps


# Crossplane Provider DigitalOcean

* Have/Create an Accesstoken
* *Web*  --> API -> Generate New Token
* `doctl` CLI is used. Feel free to use Web 
* FYI: The quality of the Providers differ a lot :/

# Provider


----
$ kubectl apply -f provider-digitalocean/provider.yaml 
$ kubectl get provider.pkg.crossplane.io/provider-do  -w
----

Still not configured

# ProviderConfig

----
kubectl -n crossplane-system create secret generic provider-do-secret --from-literal=token=<YOURTOKEN>
kubectl apply -f provider-digitalocean/providerconfig.yaml
----

# New CRDs 💃

----
$ kubectl api-resources | grep do.crossplane.io
droplets                                       compute.do.crossplane.io/v1alpha1        false        Droplet
dodatabaseclusters                             database.do.crossplane.io/v1alpha1       false        DODatabaseCluster
providerconfigs                                do.crossplane.io/v1alpha1                false        ProviderConfig
providerconfigusages                           do.crossplane.io/v1alpha1                false        ProviderConfigUsage
dokubernetesclusters                           kubernetes.do.crossplane.io/v1alpha1     false        DOKubernetesCluster
lbs                                            loadbalancer.do.crossplane.io/v1alpha1   false        Lb
----


# Create a Cluster

* Create a K8s Cluster
* As Managed Ressource
* Take some time

----
kubectl apply -f provider-digitalocean/k8s-cluster.yaml
----

Check Progress:

----
watch -n 5 kubectl get  dokubernetescluster.kubernetes.do.crossplane.io/digitalocean-k8s
----

and/or

Web


and/or


----
watch -n 5 doctl  kubernetes cluster get  digitalocean-k8s
----

# Access the Cluster

Remember:

----
  writeConnectionSecretToRef:
    name:      digitalocean-k8s
    namespace: default
----

So:

----
kubectl get secret digitalocean-k8s -o json | jq -r '.data.kubeconfig'  | base64 -d  >/tmp/do-kubeconfig.yaml
kubectl --kubeconfig /tmp/do-kubeconfig.yaml get nodes
----

# 

image::dead-men-dont-wear-plaid-cleaning-woman.gif[]



# Example S3 Bucket

Create a Bucket
Explain syncing: Aka syncing with the remote api
Check for Sync status
Delete the Bucker via aws cli
Check the K8sobject
Terrible example :)


# Editors?!!
