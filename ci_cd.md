# Presentation: CI/CD Pipeline with Docker Stack

## **Slide 1: Title Slide**
- **Title:** End-to-End CI/CD Pipeline with Docker Stack
- **Subtitle:** Automating Development to Deployment
- **Your Name & Date**

---

## **Slide 2: Introduction to CI/CD**
- **What is CI/CD?**
  - Continuous Integration (CI): Automates code integration from multiple contributors into a shared repository.
  - Continuous Delivery (CD): Ensures code is always in a deployable state.
  - Continuous Deployment (Optional): Automatically deploys every change to production.

- **Why CI/CD Matters:**
  - Accelerates development cycles.
  - Reduces manual errors.
  - Enables frequent and reliable deployments.

---

## **Slide 3: Tools Used in this Demo**
- **GitHub**: Source code management.
- **Docker**: Containerization of the application.
- **Jenkins**: CI/CD automation server.
- **FastAPI**: Example Python web application.
- **Docker Hub**: Container image registry.

---

## **Slide 4: The Architecture**
### CI/CD Workflow with Docker Stack:
1. **Developer Pushes Code:**
   - GitHub triggers the pipeline.
2. **Jenkins Pipeline Stages:**
   - Clone repository.
   - Build Docker image.
   - Run unit tests.
   - Push image to Docker Hub.
   - Deploy application using Docker.
3. **Deployment:**
   - Application runs in a containerized environment.

*Include a diagram with arrows illustrating the workflow from GitHub to deployment.*

---

## **Slide 5: FastAPI Application Overview**
- **What is FastAPI?**
  - A high-performance Python web framework for APIs.
- **Features of the Demo App:**
  - Root endpoint: `GET /` (returns "Hello World").
  - Item endpoint: `GET /items/{item_id}` (returns item details).

- **Dockerfile:**
  - Dockerizes the FastAPI app for deployment.

---

## **Slide 6: Jenkins Pipeline Stages**
1. **Clone Repository:**
   - Fetches the latest code from GitHub.
2. **Build Docker Image:**
   - Uses Dockerfile to containerize the application.
3. **Run Tests:**
   - Executes automated tests (optional).
4. **Push to Docker Hub:**
   - Stores the Docker image in Docker Hub for easy deployment.
5. **Deploy Application:**
   - Runs the Docker container on the server.

---

## **Slide 7: Key Configuration Files**
1. **Dockerfile:**
   ```dockerfile
   FROM python:3.9-slim
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   COPY . .
   CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
   ```

2. **Jenkins Pipeline Script:**
   ```groovy
   pipeline {
       agent any
       stages {
           stage('Clone Repository') {
               steps {
                   git branch: 'main', url: 'https://github.com/<your-repo>.git'
               }
           }
           stage('Build Docker Image') {
               steps {
                   sh 'docker build -t fastapi-demo .'
               }
           }
           stage('Run Tests') {
               steps {
                   sh 'docker run --rm fastapi-demo pytest'
               }
           }
           stage('Push to Docker Hub') {
               steps {
                   withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                       sh 'docker tag fastapi-demo <your-dockerhub-username>/fastapi-demo:latest'
                       sh 'docker push <your-dockerhub-username>/fastapi-demo:latest'
                   }
               }
           }
           stage('Deploy to Container') {
               steps {
                   sh 'docker run -d -p 8000:8000 <your-dockerhub-username>/fastapi-demo:latest'
               }
           }
       }
   }
   ```

---

## **Slide 8: Demo Walkthrough**
1. **Code:**
   - Show the FastAPI app.
   - Highlight the `Dockerfile` and Jenkinsfile.

2. **Pipeline Execution:**
   - Trigger the pipeline.
   - Show logs/output for each stage.

3. **Deployment Verification:**
   - Test the application in the browser or using `curl`.
     ```bash
     curl http://localhost:8000
     curl http://localhost:8000/items/42
     ```

---

## **Slide 9: Benefits of CI/CD with Docker**
- **Speed:** Faster builds and deployments.
- **Reliability:** Automated testing ensures consistent results.
- **Scalability:** Easy to replicate containers across environments.
- **Portability:** Run the same container in development, staging, and production.

---

## **Slide 10: Challenges and Best Practices**
### **Challenges:**
- Setting up Jenkins and Docker securely.
- Managing secrets (e.g., Docker Hub credentials).
- Monitoring and logging for pipelines.

### **Best Practices:**
- Use `--force-with-lease` for Git pushes during CI/CD.
- Implement health checks for deployed containers.
- Automate rollback in case of failure.
- Use lightweight base images for Docker (e.g., `python:3.9-slim`).

---

## **Slide 11: Q&A**
- Invite questions from the audience.

---

## **Slide 12: Thank You**
- Thank the audience for their attention.
- Share links to your GitHub repo and contact details.