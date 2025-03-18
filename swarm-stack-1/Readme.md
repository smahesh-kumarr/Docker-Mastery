🐳 Docker Swarm Voting App Deployment
A DevOps Project Showcasing Multi-Tier Architecture Orchestration


<!-- Replace with your screenshot link -->

🚀 Overview
This project demonstrates the deployment of a multi-tier voting application using Docker Swarm. Users vote between "Cats" and "Dogs", with votes processed by a worker, stored in PostgreSQL, and displayed in real-time. Perfect for learning production-grade container orchestration!

🌟 Features
Multi-Tier Architecture:
Frontend: Python-based vote interface, Redis for temporary storage.
Backend: PostgreSQL for persistent results.
Worker: Java service for vote processing.
Visualizer: Monitor Swarm nodes in real-time.
Scalability: Replicated services for fault tolerance.
Docker Swarm: 1 Manager + 2 Worker nodes.
Networking: Secure frontend and backend network segregation.
📂 Project Structure
bash
Copy
Edit
├── docker-compose.yml       # Defines services, networks, and Swarm configurations  
├── screenshots/             # Demo images (results, Swarm visualizer)  
└── README.md                # Project documentation  
🛠️ Getting Started
✅ Prerequisites
Docker installed on Linux VMs (x86_64).
Swarm initialized on the manager node.
🚀 Deployment Steps
1️⃣ Initialize Swarm (Manager Node)
bash
Copy
Edit
docker swarm init --advertise-addr <MANAGER-IP>  
2️⃣ Join Worker Nodes
Run the following command on each worker node:

bash
Copy
Edit
docker swarm join --token <TOKEN> <MANAGER-IP>:2377  
3️⃣ Deploy the Stack
Run the following command from the manager node:

bash
Copy
Edit
docker stack deploy -c docker-compose.yml votingapp  
4️⃣ Verify Services
Ensure all services are running:

bash
Copy
Edit
docker service ls  
🔍 Accessing the Application
Component	URL
Vote Interface	http://<VM-IP>:5000
Results	http://<VM-IP>:5001
Swarm Visualizer	http://<VM-IP>:8080
