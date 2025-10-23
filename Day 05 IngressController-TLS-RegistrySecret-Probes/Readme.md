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

### 🧠 Pro Tip

If you're using the **AKS Application Gateway Ingress Controller (AGIC)** instead of NGINX, the setup is different. Let me know if you want to explore that route or wire this into a full HTTPS ingress with TLS secrets.

Sources: [Microsoft Learn](https://learn.microsoft.com/en-us/azure/aks/app-routing-nginx-configuration), [GitHub ingress-nginx deploy guide](https://github.com/kubernetes/ingress-nginx/blob/main/docs/deploy/index.md).
