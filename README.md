# minikube-node-app

Here’s a step-by-step guide to creating a Minikube project with code. This project will deploy a simple Node.js application on Minikube using Kubernetes manifests.


---

1. Prerequisites

Minikube installed on your local machine.

kubectl installed and configured.

Docker installed for building the image.



---

2. Project Structure

Here’s how your project directory should look:

minikube-project/
├── app/
│   ├── server.js
│   ├── package.json
├── Dockerfile
├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml


---

3. Application Code

app/server.js

const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, Minikube!');
});

app.listen(port, () => {
  console.log(`App running at http://localhost:${port}`);
});

app/package.json

{
  "name": "minikube-app",
  "version": "1.0.0",
  "description": "A simple Node.js app for Minikube",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}


---

4. Dockerfile

Dockerfile

# Base image
FROM node:18

# Set working directory
WORKDIR /app

# Copy app files
COPY app/ .

# Install dependencies
RUN npm install

# Expose port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]


---

5. Kubernetes Manifests

k8s/deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: minikube-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: minikube-app
  template:
    metadata:
      labels:
        app: minikube-app
    spec:
      containers:
      - name: minikube-app
        image: minikube-app:latest
        ports:
        - containerPort: 3000

k8s/service.yaml

apiVersion: v1
kind: Service
metadata:
  name: minikube-app-service
spec:
  selector:
    app: minikube-app
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
  type: NodePort


---

6. Steps to Deploy

a. Start Minikube

minikube start

b. Build the Docker Image

Ensure that Minikube can access the Docker image:

eval $(minikube docker-env)
docker build -t minikube-app:latest .

c. Apply Kubernetes Manifests

Deploy the application using the YAML files:

kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml

d. Verify the Deployment

Check the status of pods:

kubectl get pods

e. Access the Application

Find the service NodePort and access it:

minikube service minikube-app-service


---

7. Testing

Open the provided URL in your browser, and you should see:

Hello, Minikube!


---

Would you like me to assist further with enhancements or automation?

