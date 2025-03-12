# EKS Cluster Setup and Application Deployment using Jenkins CI/CD

## Overview
This document provides a step-by-step guide to setting up an Amazon EKS cluster, configuring the control plane, and deploying applications using Jenkins CI/CD. The deployment includes AWS EBS CSI Drivers for persistent storage and Helm for Kubernetes package management.

---

## 1. Configure EKS Cluster
1. Change directory to the EKS cluster configuration:
   ```sh
   cd /EKS-cluster
   ```
2. Initialize the Terraform directory:
   ```sh
   terraform init
   ```
3. Create the infrastructure:
   ```sh
   terraform apply -auto-approve
   ```
   **Note**: Terraform backend is currently commented out.

---

## 2. Configure the Control Plane
1. Change directory to the control-plane configuration:
   ```sh
   cd /control-plane
   ```
2. Initialize the Terraform directory:
   ```sh
   terraform init
   ```
3. Create the infrastructure:
   ```sh
   terraform apply -auto-approve
   ```
   **Note**: Terraform backend is currently commented out.

---

## 3. Connect to EKS Cluster from Control Plane
1. Provide AWS credentials:
   ```sh
   aws configure
   ```
2. Attach IAM permissions to the control plane (EC2 instance):
   - `AmazonEKS_CNI_Policy`
   - `AmazonEKSWorkerNodePolicy`
   - `AmazonEC2ContainerRegistryReadOnly`
3. Connect to the EKS cluster:
   ```sh
   aws eks update-kubeconfig --name <clustername> --region <your-region>
   ```

---

## 4. Helm Installation
Helm is used for managing Kubernetes applications. Install Helm using:
```sh
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
**Resource**: [Helm Documentation](https://helm.sh/docs/intro/install/)

---

## 5. Install AWS EBS CSI Drivers
To manage persistent storage for Kubernetes:
1. Add the Helm repository for AWS EBS CSI Driver:
   ```sh
   helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
   helm repo update
   ```
2. Install the AWS EBS CSI Driver:
   ```sh
   helm upgrade --install aws-ebs-csi-driver \
       --namespace kube-system \
       aws-ebs-csi-driver/aws-ebs-csi-driver
   ```
**Resource**: [AWS EBS CSI Driver GitHub](https://github.com/kubernetes-sigs/aws-ebs-csi-driver/blob/master/docs/install.md)

---

## 6. Attach IAM Policies to Node Role
Ensure the following IAM policies are attached to the node role:
- `AmazonEBSCSIDriverPolicy`
- `AmazonEC2FullAccess`

---

## 7. Create a Storage Class
Apply the storage class configuration for AWS EBS:
```sh
kubectl create -f ebs-sc.yaml
```

---

## 8. Create Namespace
Create a dedicated namespace for the Robokart project:
```sh
kubectl apply -f namespace.yaml
```

---

## 9. Set the Default Namespace
Use `kubectx` and `kubens` for easier namespace management:
1. Install `kubectx` and `kubens`:
   ```sh
   sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
   sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
   sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
   ```
2. Switch to the `fusioniq` namespace:
   ```sh
   kubens fusioniq
   ```
**Resource**: [kubectx Repository](https://github.com/ahmetb/kubectx)

---

## 10. Exclude Jenkins Pipeline
- Save AWS credentials in Jenkins for authentication.
- **Note**: The database must be deployed before the backend deployment.
- Use the `/kubernetes` repository to deploy the MySQL database.

---

## Next Steps
- Verify EKS cluster connectivity.
- Deploy applications using Jenkins CI/CD.
- Ensure persistent storage is working as expected.

This completes the setup process. Happy deploying! 🚀

