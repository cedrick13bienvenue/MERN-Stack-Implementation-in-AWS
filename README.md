# MERN Stack Implementation on AWS

## Project Overview

This project involves the deployment of a **MERN Stack** (MongoDB, Express, React, Node.js) web application on an AWS EC2 instance. The architecture consists of a React frontend communicating with a Node.js/Express backend, which performs CRUD operations on a MongoDB database.

---

## Phase 0: Preparing Prerequisites

To begin, provision a virtual server that will host all four components of the MERN stack.

* **Instance Provisioning:** Launch a new EC2 Instance of the `t2.micro` family running **Ubuntu Server 24.04 LTS**.
* **Tagging:** Add a Tag with Key `Name` and Value `MERN-Stack-Server`.
* **Networking:** Ensure your Security Group allows inbound traffic on **Port 22 (SSH)** and **Port 5000 (Express Default)**.


---

## Phase 1: Backend Configuration (Node.js & Express)

### 1.1 Node.js Installation

```bash
sudo apt update && sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

```

> **Expected Output:** Running `node -v` and `npm -v` should display the installed versions.
>![Web Preview](screenshoots/1.png)
### 1.2 Application Code Setup

```bash
mkdir Todo && cd Todo && npm init -y
npm install express mongoose dotenv
touch index.js .env

```

> **Expected Output:** A `package.json` file should be generated in the directory.
>![Web Preview](screenshoots/2.png)

---

## Phase 2: Defining the API Routes

### 2.1 Route Folder Setup

```bash
mkdir routes && cd routes && touch api.js

```

### 2.2 Implementing the API Endpoints (`routes/api.js`)

```javascript
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {
  Todo.find({}, 'action').then(data => res.json(data)).catch(next);
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body).then(data => res.json(data)).catch(next);
  } else {
    res.json({ error: "The input field is empty" });
  }
});

router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({"_id": req.params.id}).then(data => res.json(data)).catch(next);
});

module.exports = router;

```

---

## Phase 3: The Data Model (Mongoose)

### 3.1 Model Setup

```bash
cd ..
mkdir models && cd models && touch todo.js

```

### 3.2 Defining the Schema (`models/todo.js`)

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
  }
})

const Todo = mongoose.model('todo', TodoSchema);
module.exports = Todo;

```


---