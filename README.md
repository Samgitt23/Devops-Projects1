# Devops-Projects1

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

