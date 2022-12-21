# DEPLOY HELM CHARTS LIKE JENKINS AND EKS USING TERRAFORM :space_invader:

In this repo we are using Terraforms Helm Provider on top of the kubernetes provider to quickly deploy EKS clusters with our apps on top
of them. In this, I'm deploying Jenkins on EKS. 

### Pre-requisites

* Terraform installed
* AWS cli installed on a host to connect to the cluster
* AWS credentials configured
* kubectl installed on a host to deploy to the cluster

### Deployment Instructions
* Install Terraform
* Clone this repository
* Edit ```the jenkins-values.yaml``` to match your own values
* Run a ```terraform init``` to grab providers and modules
* Run ```aws_configure``` and to establish the credentials for aws
* Run a ```terraform_apply``` and wait 10 - 15 minutes. 
* Run ```aws eks --region region update-kubeconfig --name cluster_name``` to add the cluster context to your kubeconfig
* Run ```kubectl get pods``` and ```kubectl get svc``` to ensure the applications deployed as expected
* Run ```kubectl patch svc <SERVICE_NAME> -p '{"spec": {"type": "LoadBalancer"}}'``` to change the ```Service``` to type ```LoadBalancer```
* Run ```kubectl get svc``` again to grab the AWS created DNS address
* Go to your browser and navigate to ```http://<dns-address>:<port>```  
* Log in with the credentials you set in the ```*-values.yaml``` or the ```helm_release.tf```

### Connecting
* Run ```aws eks --region us-east-1 update-kubeconfig --name basic-cluster``` to add the context to your kubeconfig

### Troubleshooting

#### Pods stuck in Pending
* Possibility of resources not efficient. The instances in the worker group could be too small to assign IP addresses to all the pods

#### Workers not joining the cluster
* Ensure the workers are getting public IP addresses


