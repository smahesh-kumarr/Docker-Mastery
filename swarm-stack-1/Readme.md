# 🐳 Docker Swarm Voting App Deployment  
**A DevOps Project Showcasing Multi-Tier Architecture Orchestration**  

---

## 🚀 Overview  
This project demonstrates the deployment of a **multi-tier voting application** using Docker Swarm. Users vote between "Cats" and "Dogs," with votes processed by a worker, stored in PostgreSQL, and displayed in real-time. Perfect for learning production-grade container orchestration!  

---

## 🌟 Features  
- **Multi-Tier Architecture**:  
  - **Frontend**: Vote interface (`Python`), Redis for temporary storage.  
  - **Backend**: PostgreSQL for persistent results.  
  - **Worker**: Java service for vote processing.  
  - **Visualizer**: Monitor Swarm nodes in real-time.  
- **Scalability**: Replicated services for fault tolerance.  
- **Docker Swarm**: 1 Manager + 2 Worker nodes.  
- **Networking**: Secure `frontend` and `backend` network segregation.  

---

## 📂 Project Structure  
```bash  
docker-compose.yml  # Defines services, networks, and Swarm configurations  
screenshots/        # Demo images (results, Swarm visualizer)  
```

---

## 🛠️ Getting Started  

### Prerequisites  
- Docker installed on Linux VMs (x86_64).  
- Swarm initialized on the manager node.  

### Deployment Steps  

#### 1️⃣ Initialize Swarm (Manager Node):  
```bash  
docker swarm init --advertise-addr <MANAGER-IP>  
```

#### 2️⃣ Join Worker Nodes:  
```bash  
docker swarm join --token <TOKEN> <MANAGER-IP>:2377  
```

#### 3️⃣ Deploy the Stack:  
```bash  
docker stack deploy -c docker-compose.yml votingapp  
```

#### 4️⃣ Verify Services:  
```bash  
docker service ls  
```

---

## 🔍 Accessing the Application  
| Component           | URL                      |
|--------------------|-------------------------|
| Vote Interface    | `http://<VM-IP>:5000`   |
| Results           | `http://<VM-IP>:5001`   |
| Swarm Visualizer  | `http://<VM-IP>:8080`   |

### Demo:  
- 🗳️ **Vote Result**: Attach your "100% Dogs" screenshot.  
- 📊 **Swarm Visualizer**: Attach Swarm node screenshot.  

---

## ⚙️ Custom Configurations  

### Stack File Snippet (`docker-compose.yml`)  
```yaml  
services:  
  vote:  
    image: bretfisher/examplevotingapp_vote  
    ports:  
      - "5000:80"  
    deploy:  
      replicas: 3  
      restart_policy:  
        condition: on-failure  
```

### Scaling Services  
```bash  
docker service scale votingapp_vote=5  # Scale vote service to 5 replicas  
```

---

## 📚 Learning Outcomes  
- **Swarm Management**: Configuring manager/worker nodes and join tokens.  
- **Stack Files**: Defining services, networks, and deploy strategies.  
- **Production-Ready Practices**: Replicas, restart policies, placement constraints.  

---

## 🚨 Troubleshooting  

### Check Service Logs:  
```bash  
docker service logs votingapp_vote  
```

### Verify Networks:  
```bash  
docker network inspect votingapp_frontend  
```

---

## 📝 Notes  
- Uses prebuilt images from Bret Fisher's Voting App.  
- Tested on Linux VMs with 4.9GB RAM and x86_64 architecture.  

---

## 🤝 Contributing  
Open to suggestions! Fork the repo and submit a PR.  
