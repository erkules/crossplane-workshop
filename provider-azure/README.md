# Install the Azure Provider

----
kubectl apply -f provider.yaml
----

Wait till the Provider ist HEALTHY

----
watch -n 5 kubectl get provider.pkg.crossplane.io/provider-azure
----

CLI


----
az login
az ad sp create-for-rbac --sdk-auth --role Owner --scopes /subscriptions/<subscriptionid> >azure-credentials.json
kubectl create secret generic azure-secret -n crossplane-system --from-file=creds=./azure-credentials.json
----


