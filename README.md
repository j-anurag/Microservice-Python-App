# Microservices Python App

## Setting Up and Managing the Microservices Python App

### 1. **Connecting kubectl with AWS EKS CLI**
To connect to the Kubernetes cluster, use the following command:

```bash
aws eks update-kubeconfig --name microservices-cluster --region ap-south-1
```
### 2. **Checking Kubernetes Resources**
view the pods in the cluster:

```bash
kubectl get pods
```
To list all namespaces:

```bash
kubectl get ns
```

### 3. **Deploying Services Using Helm Charts**
MongoDB Service
Navigate to the MongoDB directory and deploy using Helm:

```bash
cd Helm_charts/MongoDB
helm install mongo .
kubectl get pods
kubectl get pods -w
kubectl get all
kubectl get pv
```
MongoDB Connection:

```bash
mongosh mongodb://nasi:nasi1234@65.0.204.70:30005/mp3s?authSource=admin
```
MongoDB Queries:

```bash
db
db.fs.files.find({}, { filename: 1, uploadDate: 1, length: 1 }).pretty()
```
Postgres Service
Deploy Postgres in the same way:

```bash
cd Helm_charts/Postgres
helm install postgres .
kubectl get all
```
RabbitMQ Service
Deploy RabbitMQ:

```bash
cd Helm_charts/RabbitMQ
helm install rabbit .
kubectl get all
```


### 4. **Interacting with Services**
Postgres Connection:

```bash
psql 'postgres://anruag:BadeChote@13.203.73.121:30003/authdb'
```
Access RabbitMQ:

```bash
http://13.203.73.121:30004/  # Access RabbitMQ via browser
```

### 5. **Docker Commands**
Build Docker Image:

```bash
docker build -t auth .
```
Tag Docker Image:

```bash
docker tag auth:latest anurag30122003/auth
docker images
```
Push Docker Image:

```bash
docker push anurag30122003/auth
```
### 6. **Deploying Services into Kubernetes Cluster**
Navigate to each service's manifest directory and apply the Kubernetes configurations:

```bash
cd src/auth-service/manifest
kubectl apply -f .
kubectl get all
kubectl get pods -w
```
Scaling Services: To scale down the converter service from 4 replicas to 2:

```bash
kubectl scale deployment converter --replicas 2
```

### 7. **Managing AWS Resources**
In the create_instances folder, there are several command files to manage AWS resources such as creating, checking, and deleting instances.

**Available Command Files:**

- **`check-instances.py`**: Script to check the status of AWS instances.
- **`eks-create.py`**: Script to create EKS clusters in AWS.
- **`commands.txt`**: File containing additional commands to manage the resources.
- **`eks-delete.py`**: Script to delete EKS clusters in AWS.

Ensure you run these commands before executing the application.

### 8. **API Interactions**
i. Login
```bash
curl -X POST http://<service_ip>:<port>/login -u <username>:<password>
```
ii. Upload File:
```bash
curl -X POST -F 'file=@./video.mp4' -H 'Authorization: Bearer <JWT Token>' http://<service_ip>:<port>/upload
```
iii. Download File:
```bash
curl --output video.mp3 -X GET -H 'Authorization: Bearer <JWT Token>' "http://<service_ip>:<port>/download?fid=<Generated fid>"
```


