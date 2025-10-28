# Explanation of Containerization Design

## 1. Choice of Base Image
- **Backend:** Used `node:18-alpine` for a lightweight runtime and good compatibility with MongoDB drivers.
- **Frontend:** Used `nginx:alpine` to serve the React build; minimal and production-ready.
- **Database:** Used the official `mongo:6` image to ensure security patches and stability.

## 2. Dockerfile Directives
- Multi-stage builds reduce image size.
- Used `WORKDIR` for clean environment setup.
- `COPY` and `RUN npm install` for dependency management
- `EXPOSE` and `CMD` to define runtime behavior.

## 3. Docker Compose Networking
- All containers use a custom **bridge network (`app-net`)** for isolated communication.
- `yolo-client` communicates with the backend through `http://yolo-backend:5000/api`.
- Port mappings:
  - Client → 3000:80
  - Backend → 5000:5000
  - MongoDB → 27017:27017

## 4. Volumes and Persistence
- Defined `app-mongo-data` as a named volume.
- Ensures MongoDB data persists across container restarts.

## 5. Git Workflow
- Multiple commits documenting each stage (Dockerfile creation, compose setup, debugging, tagging).
- Tags used:  
  - `edwinkorir38/yolo-backend:v1.0.0`  
  - `edwinkorir38/yolo-client:v1.0.1`
- Images pushed to DockerHub for easy verification.

## 6. Running & Debugging
- Start containers:  
  ```bash
  docker-compose up --build



# Step 2: Add Infrastructure as Code (IaC) Layer

# Explanation of Configuration Management Design (Ansible)

## 1. Choice of Configuration Management Tool
- **Tool:** Ansible  
- **Reasoning:** Chosen for its **agentless architecture**, **idempotent behavior**, and **YAML-based playbooks** which make it human-readable and version-controlled.
- Works seamlessly with **Terraform** and **Vagrant** for hybrid Infrastructure as Code workflows.
- Ensures **consistent**, **repeatable**, and **scalable** deployment of the Yolomy microservices (MongoDB, Backend, and Frontend).

---

## 2. Playbook Overview
- The primary playbook: `playbook.yml`
- Targets all hosts defined in the inventory.
- Uses `become: true` for privilege escalation.
- Organizes automation into **separate roles** for modularity and clarity.

### Structure:
```yaml
- name: Configure YOLO (E-Commerce Microservice-Dockerized)
  hosts: all
  become: true

  roles:
    - role: common
      tags: ["setup", "common"]

    - role: mongodb_role
      tags: ["database"]

    - role: backend_role
      tags: ["backend"]

    - role: frontend_role
      tags: ["frontend"]

    - role: post_deploy
      tags: ["verify"]
```
---

# Command-Line Tools for Ansible Virtual Machine Setup

## 1. **Vagrant**
Vagrant is used to create and manage the virtual machine environment used for provisioning the Yolomy e-commerce app.

### 🔹 Key Commands
| Command | Description |
|----------|--------------|
| `vagrant init geerlingguy/ubuntu2004` | Initializes a new Vagrant project using the geerlingguy/ubuntu2004. |
| `vagrant up` | Creates and configures the virtual machine as defined in the `Vagrantfile`. |
| `vagrant ssh` | Connects (SSH) into the running Vagrant virtual machine. |
| `vagrant halt` | Gracefully shuts down the virtual machine. |
| `vagrant reload` | Restarts the VM and reloads the configuration (used after editing `Vagrantfile`). |
| `vagrant destroy` | Removes the VM and all related files completely. |
| `vagrant status` | Displays the current state of the Vagrant environment. |
| `vagrant ssh-config` | Displays SSH configuration details (used to connect via terminal or VS Code). |

###  Usage Example
```bash
vagrant up
vagrant ssh

```

##  Other Ansible Management Command Lines

| Command | Description | Example |
|----------|--------------|----------|
| `ansible --version` | Shows installed Ansible version | `ansible --version` |
| `ansible all -m ping -i inventory.ini` | Tests connectivity to all hosts | `ansible all -m ping -i inventory.ini` |
| `ansible-playbook playbook.yml` | Executes the main Ansible playbook | `ansible-playbook playbook.yml` |
| `ansible-playbook playbook.yml --tags "backend"` | Runs only tasks with the backend tag | `ansible-playbook playbook.yml --tags "backend"` |
| `ansible-playbook playbook.yml -vvv` | Runs playbook in verbose mode for debugging | `ansible-playbook playbook.yml -vvv` |
| `ansible-galaxy init <role_name>` | Creates a new Ansible role directory | `ansible-galaxy init backend_role` |

---



##  Git & Version Control

| Command | Description | Example |
|----------|--------------|----------|
| `git init` | Initializes a local repository | `git init` |
| `git clone <repo_url>` | Clones repository from GitHub | `git clone https://github.com/edwinkorir38/yolo.git` |
| `git add .` | Adds changes to staging area | `git add .` |
| `git commit -m "message"` | Commits staged changes | `git commit -m "added backend role"` |
| `git push origin main` | Pushes commits to GitHub | `git push origin main` |
| `git log --oneline` | Shows commit history | `git log --oneline` |
| `git branch` | Lists or creates branches | `git branch ansible-stage` |
| `git checkout <branch>` | Switches between branches | `git checkout ansible-stage` |

---

##  Networking and Connectivity

| Command | Description | Example |
|----------|--------------|----------|
| `curl <url>` | Tests connectivity and endpoint response | `curl http://localhost:5000/api/products` |
| `ping <hostname>` | Checks host reachability | `ping yolo-backend` |
| `netstat -tulpn` | Lists all active listening ports | `sudo netstat -tulpn` |
| `ss -tuln` | Modern alternative to netstat | `sudo ss -tuln` |
| `dig <hostname>` | DNS lookup tool | `dig google.com` |

---

##  File and Directory Management

| Command | Description | Example |
|----------|--------------|----------|
| `ls -lah` | Lists files with details and hidden files | `ls -lah` |
| `cd /path/to/dir` | Changes directory | `cd /etc/ansible` |
| `cat <file>` | Displays file content | `cat playbook.yml` |
| `nano <file>` | Opens file in text editor | `nano Vagrantfile` |
| `mkdir <dir>` | Creates new directory | `mkdir roles` |
| `rm -rf <dir>` | Removes files or directories recursively | `rm -rf old_role` |

---

##  Monitoring and Logs

| Command | Description | Example |
|----------|--------------|----------|
| `journalctl -u docker` | Checks Docker service logs | `sudo journalctl -u docker` |
| `tail -f /var/log/syslog` | Follows system logs live | `sudo tail -f /var/log/syslog` |
| `docker logs <container>` | Checks logs for a specific container | `sudo docker logs yolo-backend` |
| `df -h` | Displays disk usage | `df -h` |
| `free -h` | Shows memory usage | `free -h` |

---



##  Verification Steps
After provisioning, you can verify the full stack is functional:

```bash
# Check running containers
sudo docker ps

```

## Verify backend API
curl http://localhost:5000/api/products

## Verify frontend accessibility
curl http://localhost:3000

## Check MongoDB connection
sudo docker exec -it yolo-mongo mongo



