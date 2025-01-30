Here's your rewritten, plagiarism-free version:  

---

# **Sports API Management System**

## **Project Overview**  
This project focuses on developing a containerized API management system designed for retrieving and querying real-time sports data. It utilizes **Amazon ECS (Fargate)** to run containers, **Amazon API Gateway** to provide RESTful endpoints, and an external **Sports API** to fetch live sports data. The system incorporates best practices in cloud computing, including API management, container orchestration, and secure AWS service integrations.

---

## **Key Features**  
- Provides a RESTful API for accessing real-time sports data  
- Uses Amazon ECS with Fargate for containerized deployment  
- Offers a scalable, serverless architecture  
- Manages API routing and security through Amazon API Gateway  

---

## **Prerequisites**  
- **Sports API Key**: Register for an account and obtain an API key from [SerpApi](https://serpapi.com/)  
- **AWS Account**: Ensure you have an AWS account and a basic understanding of ECS, API Gateway, Docker, and Python  
- **AWS CLI Installed and Configured**: Install and configure AWS CLI to enable programmatic interaction with AWS services  
- **SerpApi Library**: Install the required SerpApi library using:  
  ```bash
  pip install google-search-results
  ```  
- **Docker CLI and Desktop**: Install Docker to build and push container images  

---

## **Technical Architecture**  
![Architecture Diagram](https://github.com/user-attachments/assets/32e49fe6-df16-40cb-b262-af1478cf01d5)  

---

## **Technology Stack**  
- **Cloud Provider**: AWS  
- **Core AWS Services**: Amazon ECS (Fargate), API Gateway, CloudWatch  
- **Programming Language**: Python 3.x  
- **Containerization**: Docker  
- **Security**: IAM roles with least-privilege policies for ECS task execution and API Gateway access  

---

## **Project Structure**  

```bash
sports-api-management/
├── app.py            # Flask application for querying sports data
├── Dockerfile        # Dockerfile for containerizing the Flask app
├── requirements.txt  # Python dependencies
├── .gitignore
└── README.md         # Project documentation
```

---

## **Setup Guide**  

### **Clone the Repository**  
```bash
git clone https://github.com/saajibade/containerized-sports-api.git
cd containerized-sports-api
```

### **Create an Amazon ECR Repository**  
```bash
aws ecr create-repository --repository-name sports-api --region us-east-1
```

### **Build and Push Docker Image to ECR**  
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker build --platform linux/amd64 -t sports-api .
docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
```

---

## **Deploying to Amazon ECS with Fargate**  

### **Step 1: Create an ECS Cluster**  
1. Navigate to the **ECS Console** → **Clusters** → **Create Cluster**  
2. Provide a name (e.g., `sports-api-cluster`)  
3. Choose **Fargate** as the infrastructure type  
4. Click **Create Cluster**  

### **Step 2: Define a Task**  
1. Go to **Task Definitions** → **Create New Task Definition**  
2. Choose **Fargate** as the launch type  
3. Define the container:  
   - Name: `sports-api-container`  
   - Image URI: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest`  
   - Container Port: **8080**  
   - Protocol: **TCP**  
4. Set environment variables:  
   - Key: `SPORTS_API_KEY`  
   - Value: `<YOUR_SPORTS_API_KEY>`  
5. Create the task definition  

### **Step 3: Deploy the Service Using ALB**  
1. Navigate to **Clusters** → **Select Cluster** → **Create Service**  
2. Choose **Fargate** as the capacity provider  
3. Set **sports-api-task** as the task definition  
4. Define the service:  
   - Name: `sports-api-service`  
   - Desired tasks: **2**  
5. Configure networking:  
   - Create a new security group  
   - Allow **All TCP** traffic from **anywhere**  
6. Load Balancer Configuration:  
   - Select **Application Load Balancer (ALB)**  
   - Create a new ALB (`sports-api-alb`)  
   - Set health check path to `/sports`  
7. Deploy the service  

### **Step 4: Test the ALB**  
- Locate the DNS name of the ALB from the ECS console  
- Access the API by visiting:  
  ```
  http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com/sports
  ```

---

## **Configuring API Gateway**  

### **Step 1: Create a New API**  
1. Open **API Gateway Console** → **Create API** → **REST API**  
2. Provide a name (e.g., `Sports API Gateway`)  

### **Step 2: Configure Integration**  
1. Create a resource `/sports`  
2. Add a **GET** method  
3. Choose **HTTP Proxy** as the integration type  
4. Set the endpoint URL to:  
   ```
   http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com/sports
   ```

### **Step 3: Deploy the API**  
1. Deploy the API to a stage (e.g., `prod`)  
2. Note the generated API Gateway URL  

---

## **Testing the System**  
Use `curl` or a web browser to verify the API response:  
```bash
curl https://<api-gateway-id>.execute-api.us-east-1.amazonaws.com/prod/sports
```

---

## **Key Takeaways**  
- Successfully implemented a **scalable, serverless** API using AWS services  
- Leveraged **Amazon ECS (Fargate)** for container management  
- Used **API Gateway** for secure API exposure  

---

## **Future Improvements**  
- Implement **caching** for frequent API requests using **Amazon ElastiCache**  
- Integrate **DynamoDB** to store user-specific query history  
- Enhance security by enforcing API key or IAM-based authentication  
- Automate deployments with **CI/CD pipelines**  

---

This project highlights a **modern cloud-native approach** to API management, ensuring scalability, cost-efficiency, and security while delivering **real-time sports data**. 🚀  

# **containerized-sports-api**😊
