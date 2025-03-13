# Drupal Docker Compose Setup

This repository contains a **Docker Compose** setup for running a Drupal application with PostgreSQL, designed to support **development, testing, and production** environments.

## **📌 Project Overview**
This setup uses **Docker Compose** to manage a multi-container Drupal application with PostgreSQL. It includes different Compose files for **development, testing, and production**, allowing for a smooth **CI/CD process**.

### **📂 File Structure**
```
├── Dockerfile                     # Builds custom Drupal image
├── docker-compose.yml              # Base configuration
├── docker-compose.override.yml     # Overrides for local development
├── docker-compose.test.yml         # Configuration for CI/CD testing
├── docker-compose.prod.yml         # Configuration for production
├── secrets/                        # Secret storage (ignored by Git)
│   ├── psql-fake-password.txt
├── sample-data/                    # Sample database data for testing
└── themes/                          # Custom Drupal themes
```

---

## **🚀 Getting Started**

### **1️⃣ Build & Run the Application**
To start the Drupal application with PostgreSQL:
```sh
docker-compose up -d
```
This will use **`docker-compose.yml`** and **`docker-compose.override.yml`** automatically.

- Drupal will be available at **http://localhost:8080**.
- PostgreSQL runs in the background with secrets managed securely.

---

### **2️⃣ Running in Different Environments**

#### ✅ **Development Mode**
```sh
docker-compose up
```
- Uses `docker-compose.yml` and `docker-compose.override.yml`.
- Binds local theme files to the container.

#### 🔍 **Testing Mode (for CI/CD)**
```sh
docker-compose -f docker-compose.yml -f docker-compose.test.yml up
```
- Overrides database configuration for testing.
- Mounts **sample data** for integration testing.

#### 🌍 **Production Mode**
```sh
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```
- Uses `docker-compose.prod.yml` for secure deployment.
- Runs in **detached mode** (`-d`) to keep services running in the background.

---

## **🔎 Checking Merged Configurations**
To preview the final merged Compose configuration **before running**, use:
```sh
docker-compose -f docker-compose.yml -f docker-compose.test.yml config > output.config
```
This is useful for **debugging** and ensuring the correct configuration is applied.

---

## **📁 Docker Secrets & Volumes**

### **🔐 Managing Secrets**
Secrets are used to store sensitive credentials securely. PostgreSQL password is stored in `secrets/psql-fake-password.txt` and referenced in Compose files.
```yaml
secrets:
  psql-pw:
    file: secrets/psql-fake-password.txt
```
Ensure secrets are **never committed to Git**!

### **📦 Persistent Volumes**
Volumes are used to **persist data** and avoid losing information when containers restart.
```yaml
volumes:
  drupal-data:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
```
These keep Drupal and PostgreSQL data intact between restarts.

---

## **🔄 CI/CD Integration**
### **⚡ Automated Testing in CI/CD**
- Run integration tests using the test environment:
  ```sh
  docker-compose -f docker-compose.yml -f docker-compose.test.yml up --abort-on-container-exit
  ```
- Use the **sample data** to ensure reproducible test results.

### **🚀 Deployment in CI/CD**
- Use `docker-compose.prod.yml` for **production deployments**.
- Deploy the latest image:
  ```sh
  docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d --build
  ```

---

## **🛠 Troubleshooting**

### **Check Running Containers**
```sh
docker ps
```

### **View Logs for Debugging**
```sh
docker-compose logs -f drupal
```

### **Restart the Services**
```sh
docker-compose restart
```

### **Stop and Remove Containers**
```sh
docker-compose down -v
```

---

## **📜 License**
This project is licensed under the **MIT License**.

---

## **🙌 Contributions**
Feel free to fork, open issues, or submit PRs to improve this setup!

