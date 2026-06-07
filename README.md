# Challenges Faced & Solutions

## 1. Minikube Cluster Was Stopped

### Issue

While deploying the application to Kubernetes, Minikube was not running.

### Error

```bash
minikube
type: Control Plane
host: Stopped
kubelet: Stopped
apiserver: Stopped
```

### Solution

```bash
minikube start --driver=docker
```

### Learning

Always verify the Kubernetes cluster status before deployment.

---

## 2. Kubernetes API Server Connection Error

### Issue

Deployment files could not be applied.

### Error

```bash
failed to download openapi
connect: connection refused
```

### Solution

```bash
minikube start
kubectl get nodes
```

### Learning

The Kubernetes API Server must be running before applying YAML manifests.

---

## 3. Docker Port Conflict

### Issue

Docker container failed to start.

### Error

```bash
Bind for 0.0.0.0:5001 failed: port is already allocated
```

### Solution

Checked running containers:

```bash
docker ps
```

Started container on a different port:

```bash
docker run -d -p 5002:5001 flask-cicd
```

### Learning

Only one process can use a port at a time.

---

## 4. Git Push Failure

### Issue

Unable to push code to GitHub.

### Error

```bash
error: src refspec main does not match any
```

### Solution

Created the first commit before pushing:

```bash
git add .
git commit -m "Initial Commit"
git push origin main
```

### Learning

Git requires at least one commit before pushing a branch.

---

## 5. GitHub Authentication Problem

### Issue

GitHub rejected password authentication.

### Solution

Generated a GitHub Personal Access Token (PAT) and used it instead of a password.

### Learning

GitHub no longer supports password-based authentication for Git operations.

---

## 6. Jenkins Could Not Access Minikube

### Issue

Jenkins pipeline failed while building and deploying.

### Error

```bash
eval false exit code 85
```

### Solution

Configured Minikube and Kubernetes access for the Jenkins user.

### Learning

Jenkins runs under a separate Linux user and requires its own Kubernetes configuration.

---

## 7. Kubernetes Permission Errors

### Issue

Jenkins could not access Kubernetes certificates.

### Error

```bash
permission denied
client.crt
client.key
ca.crt
```

### Solution

Copied Kubernetes configuration files to the Jenkins user directory and fixed ownership.

```bash
sudo cp -r ~/.kube /var/lib/jenkins/
sudo cp -r ~/.minikube /var/lib/jenkins/

sudo chown -R jenkins:jenkins /var/lib/jenkins/.kube
sudo chown -R jenkins:jenkins /var/lib/jenkins/.minikube
```

### Learning

Proper certificate permissions are required for Jenkins to communicate with Kubernetes.

---

## 8. Jenkinsfile Syntax Error

### Issue

Pipeline failed due to invalid Jenkinsfile syntax.

### Error

```bash
unexpected char: '`'
```

### Solution

Removed Markdown backticks accidentally copied into the Jenkinsfile.

### Learning

Jenkinsfiles must contain valid Groovy syntax only.

---

## 9. Docker Image Not Updating

### Issue

Application changes were not visible after deployment.

### Solution

Rebuilt the Docker image without cache and restarted the deployment.

```bash
eval $(minikube docker-env)

docker build --no-cache -t flask-cicd .

kubectl rollout restart deployment flask-deployment
```

### Learning

Kubernetes deployments require a new image build and pod restart to reflect code changes.

---

## 10. Kubernetes Pods Running Old Code

### Issue

The application continued displaying old output even after code updates.

### Investigation

Verified the code inside the running pod:

```bash
kubectl exec -it <pod-name> -- cat app.py
```

### Solution

Rebuilt the image and restarted the deployment.

### Learning

Always validate the actual code running inside Kubernetes pods.

---

## 11. Understanding Kubernetes Rolling Updates

### Observation

After restarting the deployment:

```bash
kubectl rollout restart deployment flask-deployment
```

Old pods were terminated and new pods were created automatically.

### Learning

Kubernetes performs rolling updates to minimize downtime during application updates.
