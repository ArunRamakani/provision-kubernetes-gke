
# Provision kubernetes gke cluster

1) Create a free google cloud account with $300 free creadites
2) Go to the cloud console Google Kubernetes Engine(GKE) section 
3) Provision a standerd GKE k8s cluster

# Copy kubectl config and certificates from google cloud

```gcloud container clusters get-credentials network-international-assesment --zone us-central1-a --project kubeecho```

# Setup Ambassador API GateWay 

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

# Setup a free domain name

Lets add little fun by adding a feee domain from Freenom. We can get a free domain for a short period of time. I hae added DNS A record for the domain network-international-assesment.tk pointing to the kubernetes external ip






