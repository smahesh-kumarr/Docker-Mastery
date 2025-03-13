# Private Docker Registry with TLS & Authentication

## Overview
This guide walks you through setting up a **private Docker registry** on your local system or server, enabling **TLS encryption** and **authentication** for secure image storage and distribution.

---

## 1️⃣ Setting Up a Local Private Registry

### **Run a Basic Registry**
```sh
docker run -d -p 5000:5000 --name my-registry registry:2
```
- This starts a **private registry** accessible at `http://localhost:5000` (insecure by default).

---

## 2️⃣ Enabling TLS (Secure HTTPS Access)

### **Generate a Self-Signed SSL Certificate**
```sh
mkdir -p /etc/docker/certs && cd /etc/docker/certs
openssl req -newkey rsa:4096 -nodes -sha256 -keyout domain.key -x509 -days 365 -out domain.crt
```
- When prompted, set `Common Name (CN)` to your **server’s domain or IP**.

### **Run the Registry with TLS**
```sh
docker run -d -p 5000:5000 --name my-registry \
  -v /etc/docker/certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  registry:2
```
- Now, the registry is available at `https://localhost:5000`.

### **Trust the Self-Signed Certificate (If Required)**
```sh
sudo mkdir -p /etc/docker/certs.d/localhost:5000/
sudo cp /etc/docker/certs/domain.crt /etc/docker/certs.d/localhost:5000/ca.crt
sudo systemctl restart docker
```

---

## 3️⃣ Enabling Authentication (Username/Password)

### **Create Authentication File**
```sh
sudo apt install apache2-utils  # Ubuntu
htpasswd -Bbn myuser mypassword > /etc/docker/auth/htpasswd
```

### **Run Registry with Authentication**
```sh
docker run -d -p 5000:5000 --name my-registry \
  -v /etc/docker/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \
  registry:2
```

### **Login & Push Images**
```sh
docker login localhost:5000
# Enter 'myuser' and 'mypassword'
docker tag nginx localhost:5000/nginx
docker push localhost:5000/nginx
```

---

## 4️⃣ Using Docker Hub Private Registry
### **Steps to Create a Private Repository**
1. **Go to** [hub.docker.com](https://hub.docker.com)
2. **Create a new repository**
3. **Set visibility to "Private"**
4. **Push an image securely:**
   ```sh
   docker login
   docker tag myapp docker.io/myusername/myapp
   docker push docker.io/myusername/myapp
   ```

---

## 5️⃣ Configuring Docker to Use a Private Registry Mirror
Edit `/etc/docker/daemon.json`:
```json
{
  "registry-mirrors": ["https://your-private-registry"]
}
```
Restart Docker:
```sh
sudo systemctl restart docker
```

---

## 6️⃣ Summary
| Feature | Private Registry (Local) | Docker Hub (Private Repo) |
|---------|----------------------|------------------------|
| Stores Custom Images | ✅ Yes | ✅ Yes |
| Works Without Internet | ✅ Yes | ❌ No |
| Requires TLS Setup | ✅ Yes | ❌ No |
| Requires Authentication | ✅ Optional | ✅ Yes |
| Costs Money? | ✅ Free | 🚫 Limited Free |

---

## 🎯 Next Steps
- **Set up high availability** with multiple registry servers
- **Automate builds** with a CI/CD pipeline
- **Integrate with Kubernetes** for private image storage

🚀 Happy containerizing! 🐳
