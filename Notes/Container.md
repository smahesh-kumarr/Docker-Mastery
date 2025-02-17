# Docker Containers: Simplified Overview

## 1. Containers Are Just Processes
- A container is **not** a virtual machine.
- It is just a **restricted process** running on the OS kernel.
- The container's executable must match the OS kernel (Linux or Windows).

## 2. Docker Started as Linux-Only (2013-2016)
- Initially, Docker only supported **Linux**.
- It could build images for different CPU types (**amd64, arm**, etc.), but not for Windows.

## 3. Windows Containers (2016-Present)
- Microsoft introduced **Windows Containers** in 2016.
- These run **native Windows applications** inside containers (e.g., `cmd.exe`, `.exe` files).
- No Linux is involved—they run directly on the **Windows kernel**.

## 4. Docker on Windows Uses a Tiny Linux VM
- Docker needs a **Linux kernel** to run Linux containers.
- On Windows, **Docker Desktop creates a small Linux VM** to run them.
- This is why, even on Windows, most people use **Linux-based containers**.

## 5. Platform and Architecture Matter
- Every container image is built for a specific **OS** (Linux/Windows) and **CPU architecture** (amd64, arm, etc.).
- Docker automatically picks the right image for your system.
- You can override this with `--platform` if needed.

## 6. Switching Between Linux & Windows Containers
- On **Docker Desktop (Windows)**, you can switch between:
  - **Linux Containers Mode (default)** → Uses **WSL2 VM**.
  - **Windows Containers Mode** → Uses a **Hyper-V Windows VM**.
- You **cannot** run both at the same time.

## 7. Windows Containers Never Became Popular
- Most companies found Windows Containers **difficult to use** and **poorly supported**.
- Even **Microsoft moved SQL Server to Linux**.
- Many teams are **migrating their .NET apps to Linux-based containers**.

## 🚀 Bottom Line
✅ **Linux Containers dominate** (even on Windows).  
⚠️ **Windows Containers exist but are rarely used**.  
💡 **If you're learning Docker, stick with Linux containers!**