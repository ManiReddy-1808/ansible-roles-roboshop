# 🤖 RoboShop Ansible Roles — Clean & Modular Automation

This repository contains a **well-structured Ansible automation setup** for the **RoboShop microservices application**.

Earlier everything was written as big standalone playbooks 😵‍💫
Now it has been converted into a **modular role-based architecture** (just like real enterprise projects) — making it easy to reuse, manage, and scale.

---

## 🏗️ Project Structure (What lives where?)

To follow the **DRY (Don’t Repeat Yourself)** principle, the project is divided into small reusable parts:

* **`roles/`** → Each service has its own automation (MongoDB, Redis, MySQL, Catalogue, etc.)
* **`group_vars/`** → Central place for variables (keeps code clean & configurable)
* **`roboshop.yaml`** → Main playbook that runs services in the correct order
* **`ansible.cfg`** → Project-level Ansible settings
* **`inventory.ini`** → Server details (which host runs what)

👉 In short:
**Logic is separated from configuration = cleaner + production-ready code**

---

## 🎯 How the Automation Works

This project handles a full 3-tier application using advanced Ansible concepts:

### 🔹 Common Role (Shared Setup)

Instead of repeating tasks everywhere, common things are written once:

* app setup
* NodeJS setup
* Java setup
* Python setup

All microservices reuse it 👍

---

### 🔹 Role Dependencies

Some services require others to exist first.

Example:
Shipping service will automatically ensure required setup runs before starting.

---

### 🔹 Dynamic Configuration (Jinja Templates)

We use `.j2` templates to dynamically configure:

* ports
* database endpoints
* environment variables

So same code works in **DEV / QA / PROD** environments 🔄

---

### 🔹 Reverse Proxy Routing

Nginx routes frontend requests to correct backend service automatically:

```
/catalogue → catalogue service
/cart      → cart service
/payment   → payment service
```

---

### 🔹 Safe Database Loading (Idempotency)

Database schema loads only if missing — not every time playbook runs.

Meaning:
👉 You can run playbook 100 times safely 😌

---

## 🛠️ Tech Stack Used

| Category   | Technology                          |
| ---------- | ----------------------------------- |
| Automation | Ansible                             |
| Cloud      | AWS (EC2, Route53, Security Groups) |
| OS         | RHEL 9 / CentOS                     |
| Backend    | NodeJS, Java (Maven), Python        |
| Web Server | Nginx                               |
| Databases  | MySQL, MongoDB                      |
| Cache      | Redis                               |
| Messaging  | RabbitMQ                            |

---

## 📂 Role Responsibilities

| Role          | What it Does                         | Tech            |
| ------------- | ------------------------------------ | --------------- |
| **common**    | Shared setup for all apps            | Shell / Ansible |
| **mongodb**   | Install & configure MongoDB          | NoSQL           |
| **mysql**     | Setup MySQL securely                 | SQL             |
| **catalogue** | NodeJS service + MongoDB schema      | NodeJS          |
| **shipping**  | Java service + MySQL schema          | Java / Maven    |
| **payment**   | Python service connected to RabbitMQ | Python          |
| **frontend**  | Nginx UI + reverse proxy             | Nginx           |
| **redis**     | Cache for sessions                   | In-Memory       |
| **rabbitmq**  | Message queue system                 | AMQP            |

---

## 🚀 How to Run

### Run Complete Application

Deploy everything:

```bash
ansible-playbook -e component=all roboshop.yaml
```

---

### Run Only One Service

Example: deploy only **payment service**

```bash
ansible-playbook -e component=payment roboshop.yaml
```

---

💡 **Tip:**
Because roles are modular, you can deploy or fix just one service without touching others — just like real production environments.
