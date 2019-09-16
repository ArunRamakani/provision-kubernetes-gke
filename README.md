# provision-kubernetes-gke

1) Create a free google cloud account with $300 free creadites
2) Go to the cloud console Google Kubernetes Engine(GKE) section 
3) Provision a standerd GKE k8s cluster

# Copy kubectl config and certificates from google cloud

gcloud container clusters get-credentials network-international-assesment --zone us-central1-a --project kubeecho
