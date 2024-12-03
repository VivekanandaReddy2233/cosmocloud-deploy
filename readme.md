
# **CosmoCloud Deploy Helm Chart**

This repository contains the Helm chart `cosmocloud-deploy` for deploying a complete application stack, including a backend, frontend, and Redis database, on a Kubernetes cluster. The chart ensures scalability, modularity, and ease of deployment while meeting the specified requirements.

---

## **Repository Structure**

```plaintext
cosmocloud-deploy/
├── Chart.yaml                # Helm chart metadata
├── values.yaml               # Default values for the chart
├── templates/                # Contains Kubernetes YAML templates
│   ├── backenddeployment.yaml  # Deployment for the backend
│   ├── backend_service.yaml    # Service for the backend
│   ├── frontenddeployment.yaml # Deployment for the frontend
│   ├── frontend_service.yaml   # Service for the frontend
│   ├── redis_deployment.yaml   # Deployment for Redis
│   ├── redis_service.yaml      # Service for Redis
│   ├── ingress.yaml            # Ingress resource for traffic routing
│   ├── hpa.yaml                # Horizontal Pod Autoscaler
│   ├── service.yaml            # Shared service configurations
│   ├── serviceaccount.yaml     # Service account configuration
│   ├── _helpers.tpl            # Template helpers
│   ├── NOTES.txt               # Post-deployment notes
│   └── tests/                  # Template test files
└── .helmignore                # Ignore file for Helm packaging
```

---

## **Chart Details**

This Helm chart deploys the following components:
1. **Backend Service:**
   - Deployment: `backenddeployment.yaml`
   - Service: `backend_service.yaml`
   - Image: `shreybatra/sample-backend`
   - Environment Variable: `REDIS_URI=redis://redis-svc:6379`

2. **Frontend Service:**
   - Deployment: `frontenddeployment.yaml`
   - Service: `frontend_service.yaml`
   - Image: `shreybatra/sample-frontend`
   - Environment Variable: `BACKEND_URL=http://backend-svc:8000`

3. **Redis Database:**
   - Deployment: `redis_deployment.yaml`
   - Service: `redis_service.yaml`
   - Image: `redis`



---

## **Default Values**

Default values are defined in `values.yaml`. You can override these during deployment.

### **Example Values**
```yaml
replicaCount: 1

backend:
  image: shreybatra/sample-backend
  service:
    type: ClusterIP
    port: 8000
  env:
    REDIS_URI: redis://redis-svc:6379

frontend:
  image: shreybatra/sample-frontend
  service:
    type: NodePort
    port: 5175
    nodePort: 31000
  env:
    BACKEND_URL: http://backend-svc:8000

redis:
  image: redis
  service:
    type: ClusterIP
    port: 6379
```

---

## **Prerequisites**

1. Kubernetes cluster (v1.20+ recommended).
2. Helm CLI installed (v3.0+).
3. Proper permissions to deploy to the target namespace.

---

## **Installation Instructions**

1. **Add the Helm Chart to Your Cluster:**
   Clone this repository:
   ```bash
   git clone https://github.com/VivekanandaReddy2233/cosmocloud-deploy.git
   cd cosmocloud-deploy
   ```

2. **Deploy the Chart:**
   Run the following command to deploy:
   ```bash
   helm install testapp ./cosmocloud-deploy --atomic --timeout 30s
   ```

---

## **Key Features**

1. **Modular Helm Chart:** Each component is managed independently using its own deployment and service files.
2. **Environment Variables:** Supports dynamic configuration using `ConfigMap` or direct environment variables.
3. **Service Types:**
   - Backend: ClusterIP for internal communication.
   - Frontend: NodePort for external access.
   - Redis: ClusterIP for backend access.


---

## **Customization**

Override default values by providing a custom `values.yaml` file during installation:
```bash
helm install testapp ./cosmocloud-deploy -f custom-values.yaml
```

---



