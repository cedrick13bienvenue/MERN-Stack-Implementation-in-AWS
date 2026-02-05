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

## Phase 2: Defining the API Routes

### 2.1 Route Folder Setup

* **Create Routes Directory and File:**
```bash
mkdir routes && cd routes && touch api.js

```



### 2.2 Implementing the API Endpoints

* **Configure `api.js`:** Open the file and define the initial Task handling logic.
```javascript
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => { });
router.post('/todos', (req, res, next) => { });
router.delete('/todos/:id', (req, res, next) => { })

module.exports = router;

```



> **Expected Output:** The code should define exported router modules for GET, POST, and DELETE actions.

---

## Phase 3: The Data Model (Mongoose)

### 3.1 Model Setup

* **Install Mongoose:**
```bash
cd ..
npm install mongoose
mkdir models && cd models && touch todo.js

```



### 3.2 Defining the Schema

* **Configure `todo.js`:**
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



> **Expected Output:** A Mongoose model exported for use in the API routes.

---

## Phase 4: Database Infrastructure (MongoDB Atlas)

* **Cluster Provisioning:** Create a free tier cluster on MongoDB Atlas and set Network Access to `0.0.0.0/0`.

> **Expected Output:** The Atlas dashboard should show an active cluster with a configured DB user.

---

## Phase 5: Database Connection and Security

### 5.1 Create the .env File

* **Store Connection String:**
```bash
cd ..
touch .env && vim .env

```


Paste your Atlas string: `DB = 'mongodb+srv://<username>:<password>@cluster0.mongodb.net/myTodoDB'`.

> **Expected Output:** Running `cat .env` should display your database credentials securely.

---

## Phase 6: Updating Backend Logic

### 6.1 Finalizing `index.js`

* **Full Backend Entry Point:**
```javascript
const express = require('express');
const mongoose = require('mongoose');
const routes = require('./routes/api');
require('dotenv').config();

const app = express();
const port = process.env.PORT || 5000;

mongoose.connect(process.env.DB)
  .then(() => console.log(`Database connected successfully`))
  .catch(err => console.log(err));

app.use(express.json());
app.use('/api', routes);

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});

```



### 6.2 Completing API Routes (`api.js`)

* **Update `routes/api.js` with Logic:**
```javascript
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {
  Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next)
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next)
  } else {
    res.json({ error: "The input field is empty" })
  }
});

router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({"_id": req.params.id})
    .then(data => res.json(data))
    .catch(next)
});

module.exports = router;

```



> **Expected Output:** The backend is now fully integrated with MongoDB Atlas for real-time CRUD operations.

---

## Phase 7: Testing the Backend API

* **Start the Server:**
```bash
node index.js

```



> **Expected Output:** You should see your rows of data displayed in the terminal.

> **Web Preview:** Navigating to `http://<IP>:5000/api/todos` should display an empty array or the tasks in JSON format.

---
