
# Provision kubernetes gke cluster

1) Create a free google cloud account with $300 free creadites
2) Go to the cloud console Google Kubernetes Engine(GKE) section 
3) Provision a standerd GKE k8s cluster

# Copy kubectl config and certificates from google cloud

```gcloud container clusters get-credentials network-international-assesment --zone us-central1-a --project kubeecho```

# Setup ambassador API gateway 

Apply the yaml available at https://getambassador.io/yaml/ambassador/ambassador-rbac.yaml to install Ambassador

```kubectl apply -f https://getambassador.io/yaml/ambassador/ambassador-rbac.yaml```

This instruction will install Ambassador API Gateway. After installing Ambassador, we will need to create a Kubernetes service to expose a LoadBalancer that is integrated with the gateway. Servce definition yaml is available as ambassador-service.yaml in this git repo.

```kubectl apply -f https://raw.githubusercontent.com/ArunRamakani/provision-kubernetes-gke/master/ambassador-service.yaml```

Now after few minutes time you be assigned an external IP by the kubernetes cluster from external world through a API gateway 

# Setup a test deployment

Deploy the test application service and deployment yaml to see if the IP access is working 

```
kubectl apply -f https://raw.githubusercontent.com/ArunRamakani/provision-kubernetes-gke/master/test-deployment.yaml

kubectl apply -f https://raw.githubusercontent.com/ArunRamakani/provision-kubernetes-gke/master/test-service.yaml

```
We will be able to see the tweet application comming with the given. http://34.67.155.227/

# Setup a free domain

Lets add little fun by adding a free domain from Freenom. I hae added DNS A record for the domain * "network-international-assesment.tk" * pointing to the kubernetes external IP. 

http://network-international-assesment.tk/

DNS A record may take several hours to reflect the configuration in DNS servers

# Setting certificate manger for SSL setup

We have first create namespace for certificate manager and need to disable resource validation on the namespace so the installation doesn't end up on a deadlock

```
kubectl create namespace cert-manager
kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true
```

Then install a certificate manger 

```
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.7/deploy/manifests/cert-manager.yaml
```

# Issue certificates with Ambassador and cert-manager

This step will issue a TLS certificate for TLS for the mapped domain to secure the network traffic over HTTPS

```
kubectl apply -f https://raw.githubusercontent.com/ArunRamakani/provision-kubernetes-gke/master/certificate-issuer.yaml
```

Next we have to identify the acme-http-domain and acme-http-token to create a certificate challenger service 

```
kubectl logs -n cert-manager cert-manager-d4455bb6f-jwh2h | grep acme-http-domain
kubectl get pods -n cert-manager
I0917 00:00:43.802118       1 ingress.go:49] Looking up Ingresses for selector certmanager.k8s.io/acme-http-domain=4080340413,certmanager.k8s.io/acme-http-token=1471942575
```

Now apply the challenger service 







