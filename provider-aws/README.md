Install the Provider

----
kubectl apply -f provider.yaml
kubectl get provider.pkg.crossplane.io/provider-aws -w
----

# Get Credentials


----
kubectl create secret generic aws-provider -n crossplane-system --from-file=creds=.aws/credentials
kubectl apply -f providerconfig.yaml
----
