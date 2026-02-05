# Project: MERN Stack Implementation on AWS

## Project Overview

This project involves the deployment of a **MERN Stack** (MongoDB, Express, React, Node.js) web application on an AWS EC2 instance. The architecture consists of a React frontend communicating with a Node.js/Express backend, which performs CRUD operations on a MongoDB database.

---

## Phase 0: Preparing Prerequisites

To begin, provision a virtual server that will host all four components of the MERN stack.

* **Instance Provisioning:** Launch a new EC2 Instance of the `t2.micro` family running **Ubuntu Server 24.04 LTS**.
* **Tagging:** Add a Tag with Key `Name` and Value `MERN-Stack-Server`.
* **Networking:** Ensure your Security Group allows inbound traffic on **Port 22 (SSH)** and **Port 5000 (Express Default)**.

> **Expected Output:** The EC2 dashboard should show your instance as "Running" with the specified name tag.

---

## Phase 1: Backend Configuration (Node.js & Express)

### 1.1 Node.js Installation

* **Update and Upgrade Ubuntu:**
```bash
sudo apt update && sudo apt upgrade -y

```


* **Install Node.js:**
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

```



> **Expected Output:** Running `node -v` and `npm -v` should display the installed versions.

### 1.2 Application Code Setup

* **Initialize Project Directory:**
```bash
mkdir Todo && cd Todo && npm init -y

```



> **Expected Output:** A `package.json` file should be generated in the directory.

### 1.3 ExpressJS Installation

* **Install Dependencies:**
```bash
npm install express dotenv
touch index.js

```



---

