# 🚀 Jenkins + Docker + Email Notification Demo

## 🧪 Objective

This demo covers a complete DevOps flow where we:

* Build a Docker image using Jenkins
* Run a containerized application
* Send email notifications on build success/failure

---

## 🧠 Prerequisites

* Jenkins installed on EC2
* Docker installed
* Gmail account with App Password
* Email Extension Plugin installed in Jenkins

---

# ⚙️ Step 1: Install Docker

```bash
sudo apt update
sudo apt install docker.io -y
```

---

## 🔹 Give Jenkins access to Docker

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
sudo systemctl restart docker
```

---

# 📁 Step 2: Create Demo Application

```bash
mkdir demo-app
cd demo-app
```

---

## 🔹 Create index.html

```bash
echo "<h1>Hello from Jenkins 🚀</h1>" > index.html
```

---

## 🔹 Create Dockerfile

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

---

# ⚙️ Step 3: Configure Jenkins Job

---

## 🔹 Create Job

* Go to Jenkins Dashboard
* Click **New Item**
* Name: `docker-demo`
* Select **Freestyle Project**

---

## 🔹 Add Build Step

Go to:

```
Build → Execute Shell
```

---

## 🔹 Add Commands

```bash
cp -r /home/ubuntu/demo-app/* .

docker rm -f demo-container || true

docker build -t demo-app .

docker run -d -p 8082:80 --name demo-container demo-app
```

---

# 🌍 Step 4: Access Application

Open in browser:

```
http://<EC2-IP>:8082
```

---

# 📩 Step 5: Configure Email Notification

---

## 🔹 Go to:

```
Manage Jenkins → System
```

---

## 🔹 Configure SMTP

| Field       | Value          |
| ----------- | -------------- |
| SMTP Server | smtp.gmail.com |
| Port        | 587            |
| Use TLS     | ✅              |
| Use SSL     | ❌              |
| Username    | your Gmail     |
| Password    | App Password   |

---

## 🔹 Test Email

* Add recipient email
* Click **Test Configuration**

---

# 📬 Step 6: Add Email in Job

---

## 🔹 Go to Job → Configure

---

## 🔹 Add Post-build Action

```
Editable Email Notification
```

---

## 🔹 Configure:

* Recipient List: [your-email@gmail.com](mailto:your-email@gmail.com)
* Triggers:

  * Success
  * Failure

---

# ▶️ Step 7: Run Job

Click:

```
Build Now
```

---

## ✅ Expected Output

* Docker container runs
* App accessible in browser
* Email received on success

---

# 💥 Step 8: Test Failure Case

---

## 🔹 Modify build step:

```bash
exit 1
```

---

## 🔹 Run job again

---

## ❌ Expected

* Build fails
* Failure email received

---

# 🔄 Complete Flow

```
Jenkins Job
   ↓
Build Docker Image
   ↓
Run Container
   ↓
Send Email Notification
```

---

# 🧠 Key Learnings

* Jenkins automation basics
* Docker integration
* CI/CD workflow
* Email alerting system

---

# 🎯 Conclusion

This demo simulates a real-world CI/CD pipeline where:

* Applications are built and deployed automatically
* Teams are notified instantly about build status

---

# 🚀 Next Steps

* Push Docker image to DockerHub
* Convert to Jenkins Pipeline (Groovy)
* Deploy to Kubernetes (EKS)
* Add Slack notifications

---
