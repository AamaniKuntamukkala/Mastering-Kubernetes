**To deploy the NGINX Ingress Controller for Azure, use the Azure-specific manifest instead of the AWS one. Here's the correct command:**

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.1/deploy/static/provider/cloud/deploy.yaml
```

This manifest is designed for **cloud environments like Azure**, and it provisions the necessary resources including a public LoadBalancer service.

---

### ✅ What This Does

- Deploys the **ingress-nginx controller** into your AKS cluster.
- Creates a **LoadBalancer service** that gets a public IP from Azure.
- Enables routing external traffic to internal services via Ingress rules.

---

### 🔍 After Applying

You can verify the external IP with:
```bash
kubectl get svc -n ingress-nginx
```

Look for:
```
NAME            TYPE           EXTERNAL-IP     PORT(S)
ingress-nginx   LoadBalancer   <your-ip>       80:30080/TCP
```

Once the IP is assigned, you can map it to a domain and configure TLS if needed.

---

This setup is configuring **secure ingress and private image access** in Kubernetes. Let’s break it down step by step:

---

### 🔐 1. **Ingress Controller Deployment**

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.1/deploy/static/provider/aws/
```

- This command deploys the **NGINX Ingress Controller** on an AWS-based Kubernetes cluster.
- It enables routing external HTTP/HTTPS traffic to internal services using Ingress rules.

---

### 🔐 2. **TLS Secret for HTTPS**

```bash
kubectl create secret tls nginx-tls-default --key='tls.key' --cert='tls.crt'
```

- Creates a **TLS secret** named `nginx-tls-default`.
- Used by the Ingress Controller to serve HTTPS traffic securely.
- `tls.key` and `tls.crt` are your SSL certificate and private key files.

---

### 🐳 3. **Docker Registry Secret**

```bash
kubectl create secret docker-registry docker-pwd \
--docker-server=docker.io \
--docker-username=kiran2516993 \
--docker-password=8KMA8F10zkFtKAMYvh2s6p0E \
--docker-email=pinapathuri.saikiran@gmail.com
```

- Creates a secret named `docker-pwd` for pulling images from a **private Docker Hub repository**.
- This secret is used in your pod specs like this:

```yaml
imagePullSecrets:
  - name: docker-pwd
```

---

### 🧠 Summary

| Component        | Purpose                                      |
|------------------|----------------------------------------------|
| Ingress Controller | Routes external traffic to services         |
| TLS Secret       | Enables HTTPS via SSL certificates           |
| Docker Secret    | Authenticates with private Docker registry   |

Would you like help wiring this into an Ingress rule or validating the TLS setup with a test domain?
