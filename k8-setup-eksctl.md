# k8-with-eksctl

create an ec2 instannce extralarge
create IAM role with admin policies and attach to ec2 istance

--------------------------------------------------
## install kubectl
```
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
mkdir -p $HOME//usr/bin && cp ./kubectl $HOME//usr/bin/kubectl && export PATH=$PATH:$HOME/usr/bin
echo 'export PATH=$PATH:$HOME/usr/bin' >> ~/.bashrc
kubectl version --short --client
```

------------------------------------------------------------------------
## Install eksctl

```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
 echo 'export PATH=$PATH:/usr/local/bin/' >> ~/.bashrc
eksctl version
```

--------------------------------------------

## Create eks cluster
```
eksctl create cluster \
--name myeks-cluster \
--region us-east-1 \
--without-nodegroup
```

----------------------------------
```
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster myeks-cluster \
    --approve
    
```    
    
--------------------------------------------

## create a node group 
```
eksctl create nodegroup \
  --cluster myeks-cluster \
  --region us-east-1 \
  --name my-mngnodes \
  --node-type t2.medium \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 4 \
  --ssh-access \
  --ssh-public-key Spinnaker \
  --managed \
  --asg-access \
  --external-dns-access \
  --full-ecr-access \
  --appmesh-access \
  --alb-ingress-access
```  
  
  --------------------------------------------------
  ## Clean UP
  
  ```
  eksctl delete nodegroup --cluster=myeks-cluster \
                   --region=us-east-1 \
	          			   --name=my-mngnodes
```
```
					   
						   
	eksctl delete cluster --name=myeks-cluster \
                  --region=us-east-1
```		  
