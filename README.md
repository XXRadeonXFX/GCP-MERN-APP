# MERN Microservices Application

[![Docker](https://img.shields.io/badge/Docker-Ready-blue?logo=docker)](https://docker.com)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Supported-326CE5?logo=kubernetes)](https://kubernetes.io)
[![GCP](https://img.shields.io/badge/GCP-Deployed-4285F4?logo=google-cloud)](https://cloud.google.com)

A sample MERN (MongoDB, Express.js, React, Node.js) application demonstrating microservices architecture with containerization and Kubernetes orchestration.

## 🏗️ Architecture

This application consists of three main services:

- **Hello Service** (Port 3001): Basic greeting service
- **Profile Service** (Port 3002): User profile management with MongoDB
- **Frontend** (Port 3000): React-based user interface

## 📋 Prerequisites

- Node.js 16+ and npm
- Docker and Docker Compose
- Kubernetes cluster (local or cloud)
- MongoDB instance (local or cloud)
- Google Cloud SDK (for GCP deployment)

## 🚀 Quick Start

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd SampleMERNwithMicroservices
   ```

2. **Set up environment variables**

   Create `.env` file in `backend/helloService/`:
   ```bash
   PORT=3001
   NODE_ENV=development
   ```

   Create `.env` file in `backend/profileService/`:
   ```bash
   PORT=3002
   MONGO_URL=mongodb://localhost:27017/profiledb
   NODE_ENV=development
   ```

3. **Install dependencies**
   ```bash
   # Backend services
   cd backend/helloService && npm install
   cd ../profileService && npm install
   
   # Frontend
   cd ../../frontend && npm install
   ```

4. **Start services**
   ```bash
   # Terminal 1 - Hello Service
   cd backend/helloService && npm start
   
   # Terminal 2 - Profile Service  
   cd backend/profileService && npm start
   
   # Terminal 3 - Frontend
   cd frontend && npm start
   ```

5. **Access the application**
   - Frontend: http://localhost:3000
   - Hello Service: http://localhost:3001
   - Profile Service: http://localhost:3002

## 🐳 Docker Deployment

### Using Docker Compose (Recommended)

1. **Build and run all services**
   ```bash
   docker-compose up --build
   ```

2. **Run in detached mode**
   ```bash
   docker-compose up -d
   ```

3. **View logs**
   ```bash
   docker-compose logs -f [service-name]
   ```

4. **Stop services**
   ```bash
   docker-compose down
   ```

### Manual Docker Commands

```bash
# Build images
docker build -t hello-service ./backend/helloService
docker build -t profile-service ./backend/profileService
docker build -t frontend ./frontend

# Run containers
docker run -p 3001:3001 --env-file backend/helloService/.env hello-service
docker run -p 3002:3002 --env-file backend/profileService/.env profile-service
docker run -p 3000:3000 frontend
```

## ☸️ Kubernetes Deployment

### Prerequisites
- Kubernetes cluster running
- kubectl configured
- MongoDB accessible from cluster

### Step 1: Create ConfigMap for MongoDB
```bash
kubectl create configmap app-config \
  --from-literal=MONGO_URL='mongodb://username:password@mongodb-service:27017/profiledb'
```

### Step 2: Deploy Services
```bash
# Deploy hello service
kubectl apply -f kubernetes/helloservice/

# Deploy profile service
kubectl apply -f kubernetes/profileservice/

# Deploy frontend
kubectl apply -f kubernetes/frontend/
```

### Step 3: Verify Deployment
```bash
# Check pods status
kubectl get pods

# Check services
kubectl get services

# Check logs
kubectl logs -l app=profile-service
```

### Step 4: Access Services

**Using Port Forwarding:**
```bash
kubectl port-forward svc/hello-service 4001:3001
kubectl port-forward svc/profile-service 4002:3002
kubectl port-forward svc/frontend 4000:3000
```

**Access URLs:**
- Frontend: http://localhost:4000
- Hello Service: http://localhost:4001
- Profile Service: http://localhost:4002

**Using LoadBalancer (if supported):**
```bash
kubectl get svc frontend
# Use EXTERNAL-IP to access the application
```

## 🌐 Google Cloud Platform Deployment

### Prerequisites
- GCP account with billing enabled
- Google Cloud SDK installed
- Docker authentication configured

### Step 1: Configure Docker for GCP
```bash
gcloud auth configure-docker us-central1-docker.pkg.dev
```

### Step 2: Tag and Push Images
```bash
# Tag images
docker tag hello-service us-central1-docker.pkg.dev/heroviredacademics/my-repo/hello-service:latest
docker tag profile-service us-central1-docker.pkg.dev/heroviredacademics/my-repo/profile-service:latest
docker tag frontend us-central1-docker.pkg.dev/heroviredacademics/my-repo/frontend:latest

# Push images
docker push us-central1-docker.pkg.dev/heroviredacademics/my-repo/hello-service:latest
docker push us-central1-docker.pkg.dev/heroviredacademics/my-repo/profile-service:latest
docker push us-central1-docker.pkg.dev/heroviredacademics/my-repo/frontend:latest
```

### Step 3: Deploy to GKE
```bash
# Create GKE cluster (if not exists)
gcloud container clusters create mern-cluster \
  --zone=us-central1-a \
  --num-nodes=3

# Get cluster credentials
gcloud container clusters get-credentials mern-cluster --zone=us-central1-a

# Deploy application
kubectl apply -f kubernetes/
```

## 📁 Project Structure

```
SampleMERNwithMicroservices/
├── backend/
│   ├── helloService/
│   │   ├── Dockerfile
│   │   ├── package.json
│   │   ├── .env
│   │   └── index.js
│   └── profileService/
│       ├── Dockerfile
│       ├── package.json
│       ├── .env
│       └── index.js
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   ├── public/
│   └── src/
├── kubernetes/
│   ├── helloservice/
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── profileservice/
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   └── frontend/
│       ├── deployment.yaml
│       └── service.yaml
├── docker-compose.yaml
└── README.md
```

## 🔧 Configuration

### Environment Variables

| Service | Variable | Description | Default |
|---------|----------|-------------|---------|
| Hello Service | `PORT` | Service port | 3001 |
| Hello Service | `NODE_ENV` | Environment | development |
| Profile Service | `PORT` | Service port | 3002 |
| Profile Service | `MONGO_URL` | MongoDB connection string | Required |
| Profile Service | `NODE_ENV` | Environment | development |
| Frontend | `REACT_APP_HELLO_API_URL` | Hello service URL | http://localhost:3001 |
| Frontend | `REACT_APP_PROFILE_API_URL` | Profile service URL | http://localhost:3002 |

### Production Configuration

For production deployment, ensure:

1. **MongoDB**: Use a managed MongoDB service (MongoDB Atlas, Google Cloud MongoDB, etc.)
2. **Environment Variables**: Set `NODE_ENV=production`
3. **Resource Limits**: Configure appropriate CPU/memory limits in Kubernetes
4. **Health Checks**: Enable health check endpoints
5. **SSL/TLS**: Configure HTTPS for production traffic
6. **Monitoring**: Set up logging and monitoring

## 🔍 Monitoring and Debugging

### Check Application Health
```bash
# Hello Service health
curl http://localhost:3001/health

# Profile Service health  
curl http://localhost:3002/health

# Frontend (check if React app loads)
curl http://localhost:3000
```

### Kubernetes Debugging
```bash
# Check pod status
kubectl get pods -o wide

# Check pod logs
kubectl logs -f deployment/profile-service

# Check pod events
kubectl describe pod <pod-name>

# Check service endpoints
kubectl get endpoints

# Check ConfigMap
kubectl describe configmap app-config
```

### Docker Debugging
```bash
# Check running containers
docker ps

# Check container logs
docker logs <container-id>

# Execute into container
docker exec -it <container-id> /bin/sh

# Check container resources
docker stats
```

## 🚀 Performance Optimization

### Frontend (React)
- Build for production: `npm run build`
- Use environment-specific builds
- Enable gzip compression in nginx
- Implement code splitting

### Backend Services
- Use PM2 for process management
- Implement connection pooling for MongoDB
- Add caching layer (Redis)
- Use compression middleware

### Kubernetes
- Configure horizontal pod autoscaling (HPA)
- Set appropriate resource requests/limits
- Use persistent volumes for stateful data
- Implement readiness and liveness probes

## 🛠️ Troubleshooting

### Common Issues

**Issue**: Services can't connect to MongoDB
```bash
# Check MongoDB connectivity
kubectl exec -it <pod-name> -- nslookup mongodb-service

# Verify ConfigMap
kubectl get configmap app-config -o yaml
```

**Issue**: Frontend can't reach backend services
```bash
# Check service discovery
kubectl get svc
kubectl describe svc hello-service
```

**Issue**: Image pull errors
```bash
# Check image tags and registry authentication
docker pull us-central1-docker.pkg.dev/heroviredacademics/my-repo/hello-service:latest
```

**Issue**: Pod crashes or restarts
```bash
# Check pod events and logs
kubectl describe pod <pod-name>
kubectl logs <pod-name> --previous
```

## 📄 API Endpoints

### Hello Service (Port 3001)
- `GET /` - Welcome message
- `GET /health` - Health check

### Profile Service (Port 3002)  
- `GET /` - Welcome message
- `GET /profiles` - Get all profiles
- `POST /profiles` - Create profile
- `GET /profiles/:id` - Get profile by ID
- `PUT /profiles/:id` - Update profile
- `DELETE /profiles/:id` - Delete profile
- `GET /health` - Health check

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

For support and questions:
- Create an issue in this repository
- Check existing issues for solutions
- Review logs and debugging steps above

## 🔗 Resources

- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Google Cloud Documentation](https://cloud.google.com/docs)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [React Documentation](https://reactjs.org/docs/)
- [Express.js Documentation](https://expressjs.com/)
