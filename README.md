# Giff Maker - Kubernetes Deployment on AWS EKS

This repository provides a step-by-step guide to deploying the Giff Maker application on Amazon EKS using Kubernetes. It includes Docker configurations, Kubernetes manifests, and troubleshooting steps encountered during the deployment process.

## 🛠️ Technologies Used

- **Frontend**: Node.js with Nginx
- **Containerization**: Docker
- **Orchestration**: Kubernetes on Amazon EKS
- **Container Registry**: Amazon ECR

## 📦 Project Structure

giff-maker-k8s-deployment/

giff-maker-k8s-deployment/
├── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── troubleshooting/
│   ├── issues-faced.md
│   └── resolution-steps.md
├── README.md
├── .gitignore
└── LICENSE (optional)



## 🚀 Deployment Steps

1. Clone the Repository

```bash
git clone https://github.com/your-username/giff-maker-k8s-deployment.git
cd giff-maker-k8s-deployment
git add .
git commit -m "new commit"
git push

2. Build and Push Docker Image to ECR

# Authenticate Docker to your ECR registry
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <your-account-id>.dkr.ecr.us-east-1.amazonaws.com

# Build the Docker image
docker build -t giff-maker .

# Tag the image
docker tag giff-maker:latest <your-account-id>.dkr.ecr.us-east-1.amazonaws.com/giff-maker:latest

# Push the image to ECR
docker push <your-account-id>.dkr.ecr.us-east-1.amazonaws.com/giff-maker:latest

3. Deploy to Amazon EKS:

# Apply Kubernetes manifests
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

4. Access the Application

# Port-forward the service to access it locally
kubectl port-forward svc/giff-maker-service 8080:80
Open your browser and navigate to http://localhost:8080 to access the application.

🐞 Troubleshooting
Detailed information about issues encountered during the deployment and their resolutions can be found in the troubleshooting directory.

📄 License
This project is licensed under the MIT License - see the LICENSE file for details.

---

## 🐛 Troubleshooting Documentation

Create a `troubleshooting` directory with two markdown files:

### `issues-faced.md`

```markdown
# Issues Faced During Deployment

## 1. CNI Plugin Not Initialized

- **Error**: `cni plugin not initialized`
- **Cause**: The Amazon VPC CNI plugin was not installed.
- **Resolution**: Installed the VPC CNI plugin via the EKS Add-ons page in the AWS Management Console.

## 2. Port-Forwarding Connection Refused

- **Error**: `error forwarding port 3000 to pod ... connect: connection refused`
- **Cause**: The container was exposing port 80, but the service was forwarding to port 3000.
- **Resolution**: Updated the service to forward to port 80, matching the container's exposed port.

## 3. Static Content Loading Without Functionality

- **Issue**: The application loaded statically without interactive functionality.
- **Cause**: The frontend was making API calls to `localhost`, which doesn't resolve correctly within the container.
- **Resolution**: Updated the frontend to use relative paths or environment variables for API endpoints.
resolution-steps.md
markdown
Copy
Edit
# Resolution Steps

## Installing VPC CNI Plugin

1. Navigate to the EKS cluster in the AWS Management Console.
2. Go to the "Add-ons" section.
3. Click "Create add-on".
4. Select "Amazon VPC CNI" and proceed with the installation.

## Updating Service Port Configuration

1. Open `k8s/service.yaml`.
2. Ensure the `targetPort` matches the container's exposed port (80).
3. Apply the updated service configuration:

```bash
kubectl apply -f k8s/service.yaml
Configuring Frontend API Endpoints
Update the frontend code to use relative paths for API calls.

Rebuild the Docker image:

bash
Copy
Edit
docker build -t giff-maker .
Push the updated image to ECR:

bash
Copy
Edit
docker push <your-account-id>.dkr.ecr.us-east-1.amazonaws.com/giff-maker:latest
Update the deployment to use the new image:

bash
Copy
Edit
kubectl set image deployment/giff-maker-deployment giff-maker=<your-account-id>.dkr.ecr.us-east-1.amazonaws.com/giff-maker:latest
yaml
Copy
Edit

---

Feel free to customize the repository and documentation further to suit your project's needs. If you require assistance with any specific section or have additional questions, don't hesitate to ask!
::contentReference[oaicite:0]{index=0}
 






Sources

