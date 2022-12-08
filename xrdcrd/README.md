We just have an CRD and and XRD have fun 

run in a Shell:

----
watch kubectl api-resources --api-group=linsenraum.de
----

Apply the CRD

----
kubectl apply -f crd.yaml
----

Apply the XRD

----
kubectl apply -f xrd.yaml
----

Play around and delete the resources again ☝️

With playing around you could i.e. create an instance/object of that CRD :)

----
kubectl delete -f crd.yaml
kubectl delete -f xrd.yaml
----

