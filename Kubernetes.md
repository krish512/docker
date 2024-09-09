**Lab Manual: Working with Kubernetes to run a highly available web server**

**Objective:** In this lab, you will learn how to set up and manage containers using Kubernetes on a Linux host, including setting up a highly available web server.

**Prerequisites:**

- Familiarity with basic command line operations (e.g., `bash`, `cd`, `mkdir`, `vim`)
- Basic knowledge of containerization concepts
- Linux server with Docker installed

**Step 1: Login to Play with Kubernetes**

1. Open the web browser and go to [https://labs.play-with-k8s.com/](https://labs.play-with-k8s.com/).
2. Create your docker account and then click on start to start your session.
3. When the session has started, click on `Add New Instance` to create a host to run as master node.

**Step 2: Bootstrap the Kubernetes cluster**

You can bootstrap a cluster as follows:

 1. Initialise cluster master node:
```bash
 kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr 10.5.0.0/16
```

 2. Initialise cluster networking:
```bash
kubectl apply -f https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/kubeadm-kuberouter.yaml
```

3. Get the Cluster join command
```bash
kubeadm token create --print-join-command
```

4. Click on `Add New Instance` to create a new host to run a worker node. Join the worker to the Kubernetes cluster by pasting the output of the above command in this new host

5. Go back to node 1 and test if the cluster is up and running. You should see 2 nodes in ready state
```bash
kubectl get nodes
```

**Step 3: Creating a New Folder for Your Project**

1. Create a new project folder called `project`.
2. Navigate to this folder
```bash
mkdir project
cd project
```

**Step 4: Create a deployment**

1. Create the html file `deployment.yaml`
```bash
vi deployment.yaml
```

2. Add the following contents to the `deployment.yaml` file:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: public.ecr.aws/nginx/nginx:1.26
        ports:
        - containerPort: 80
```

3. Apply this deployment to the Kubernetes Cluster
```bash
kubectl apply -f deployment.yaml
``` 

4. Check if the containers are up and running
```bash
kubectl get pods
```

Congratulations! You have successfully set up a Nginx web server running with 3 replicas using Kubernetes cluster.
