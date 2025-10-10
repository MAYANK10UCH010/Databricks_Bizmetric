# End-to-End Deployment Process

## 1. Pull Latest Code
1. Clone or pull your latest repository.
2. Update necessary components:
   - **GitHub Actions / Workflow Folder** (`.github/workflows`)
   - `utils/model_loader.py` (latest version)

---

## 2. AWS Account Setup
- Ensure you have access to your AWS account.

---

## 3. AWS IAM User
1. Create or use an existing IAM user.
2. Fetch credentials:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
3. Store these credentials securely in **GitHub Secrets**.

---

## 4. AWS Secrets Manager
- Store sensitive API keys or credentials here to avoid hardcoding them.

---

## 5. Setup AWS ECR
- Create an **Elastic Container Registry** to store your Docker images.

---

## 6. Create ECS Cluster
- Set up an **ECS Cluster** to run your containerized application.

---

## 7. IAM Policies
1. Attach an **inline policy** either to the IAM user or to the ECS role.
2. Custom policy ensures correct permissions between services.
3. Policy steps:
   - Review the **inline policy JSON** in your code repository.
   - There are **two policies**:
     1. Create the first policy with required permissions.
     2. Create the second policy with appropriate permissions.

---

## 8. Configure EC2 Security Group
- Navigate: **EC2 → Security Groups → Your Security Group**
- Add inbound rule:
  - **Type:** Custom TCP
  - **Port:** 8080
  - **Source:** Anywhere (0.0.0.0/0)

---

## 9. Accessing the Application
- Navigate: **ECS Cluster → Service → Task → Public IP**
- If no public IP:
  - Check **ENI ID** → Public IPv4 address
- Run the application in your browser:
