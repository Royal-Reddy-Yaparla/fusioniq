### Configure EKS cluster

- Change directory to /EKS-cluster

- initialize the terraform directoey 
    ```
    terraform init
    ````


- Create infrastructure 
    ```
    terraform apply -auto-approve
    ````

Note: Its take a while to create infrastructure

### Configure control-plane 

- Change directory to /control-plane

- initialize the terraform directoey 
    ```
    terraform init
    ````


- Create infrastructure 
    ```
    terraform apply -auto-approve
    ````

---
### Connect to EKS Cluster
```sh
aws eks update-kubeconfig --name <clustername> --region <your-region>
```
✅ This allows you to interact with your EKS cluster using `kubectl`.