# Devops-Projects1


$ sudo yum remove -y aws-cli
$ pip3 install --user awscli
$ sudo ln -s $HOME/.local/bin/aws /usr/bin/aws
$ aws --version

# Installing kubectl
Refer===https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
$ curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.27.1/2023-04-19/bin/linux/amd64/kubectl
$ chmod +x ./kubectl 
$ mv kubectl /bin  OR $ mv kubectl /usr/local/bin
$ kubectl version --output=yaml

#Installing or eksctl
Refer==https://github.com/eksctl-io/eksctl/blob/main/README.md#installation
$ curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
$ cd /tmp
$ sudo mv /tmp/eksctl /bin   OR  $ sudo mv /tmp/eksctl /usr/local/bin
$ eksctl version



# Setup Kubernetes using eksctl
Refer===https://github.com/aws-samples/eks-workshop/issues/734
eksctl create cluster --name virtualtechbox-cluster \
--region ap-south-1 \
--node-type t2.small \
$ kubectl get nodes

# Create deployment Manifest File
Refer===https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
$ nano regapp-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: virtualtechbox-regapp
  labels:
     app: regapp

spec:
  replicas: 2
  selector:
    matchLabels:
      app: regapp

  template:
    metadata:
      labels:
        app: regapp
    spec:
      containers:
      - name: regapp
        image: rg7121/samuel-devops-2
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1

# Create Service Manifest File 
Refer===https://kubernetes.io/docs/tutorials/services/connect-applications-service/
$ nano regapp-service.yml
apiVersion: v1
kind: Service
metadata:
  name: virtualtechbox-service
  labels:
    app: regapp 
spec:
  selector:
    app: regapp 

  ports:
    - port: 8080
      targetPort: 8080

  type: LoadBalancer


## AFER PASTE THE URL LINK FROM SERVICES 

-- kubectl get all 

-------------------DOMAIN LINK FROM KUBERNETES SERVICE--------------------:8080 [ make sure to add 8080 in the end of the link pasting it on the browser

