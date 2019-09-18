
# Provision kubernetes gke cluster

1) Create a free google cloud account with $300 free creadites
2) Go to the cloud console Google Kubernetes Engine(GKE) section 
3) Provision a standerd GKE k8s cluster

# Copy kubectl config and certificates from google cloud

```gcloud container clusters get-credentials network-international-ass --zone us-central1-a --project kubeecho```

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
We will be able to see the tweet application comming with the given. http://34.69.90.135/test-page

# Setup a free domain

Lets add little fun by adding a free domain from Freenom. I hae added DNS A record for the domain * "network-international-assesment.tk" * pointing to the kubernetes external IP. 

http://network-international-assesment.tk/test-page

DNS A record may take several hours to reflect the configuration in DNS servers

# Monitoring and logging

We have enabled Stackdriver during kubernetes cluster creation, this aggregates of metrics, logs, and events from k8s, infrastructure, pods, services and other asserts of k8s. 

# Overall Landscape 

Tech Stack  | Usage
------------ | -------------
Google Kubernete Engine | Managed Kubernete for container Orchestration 
Ambassador | API gateway 
Ambassador | Load Balancer
Istio | Connect, Secure, Control and Observe  microservices
Stackdriver | Monitoring & Logging


![alt landscape](https://i.ibb.co/7z5dnCZ/Screen-Shot-2019-09-18-at-1-36-33-PM.png)
