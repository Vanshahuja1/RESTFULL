# 📖 Let's Learn RESTful APIs Together!

## 🚀 Introduction
Welcome to this guide on **RESTful APIs**! Think of RESTful APIs as the waiters in a restaurant—they take your order (request), fetch the food from the kitchen (server), and serve it back to you (response). 🍕📡

This guide will walk you through:
- What APIs are and how RESTful APIs work.
- The principles of REST.
- Building a **RESTful API** in **Node.js (Express)**.
- Implementing the same API in **Flask (Python)**.
- Hands-on examples with HTTP methods: **GET, POST, PUT, DELETE**.

---

## 📌 Chapter 1: Understanding RESTful APIs
### 🧐 What is an API?
An **API (Application Programming Interface)** allows two software applications to communicate.

**Example:**
- You use **Zomato** to order food.
- Zomato calls the **restaurant’s API** to check availability.
- The restaurant’s system **responds** with availability.
- If available, Zomato places your order via the API!

### 🔥 What is REST?
**REST (Representational State Transfer)** is a set of rules for designing networked applications. RESTful APIs follow **six key principles:**
1. **Client-Server Architecture** – The client (app) requests data, and the server provides it.
2. **Stateless** – Each request is independent; the server doesn’t store past requests.
3. **Cacheable** – Responses can be cached to improve efficiency.
4. **Uniform Interface** – Consistent request/response structure.
5. **Layered System** – API has multiple layers (security, database, business logic).
6. **Code on Demand (Optional)** – Server can send executable code (like JavaScript) to the client.

### ⚡ How RESTful APIs Work
RESTful APIs use **HTTP methods**:
- **GET** – Retrieve data 📄
- **POST** – Create new data ✍️
- **PUT** – Update existing data 🔄
- **DELETE** – Remove data ❌

**Example Endpoints:**
| HTTP Method | Endpoint         | Description                 |
|------------|-----------------|-----------------------------|
| GET        | /api/users       | Fetch all users            |
| GET        | /api/users/:id   | Get a user by ID           |
| POST       | /api/users       | Add a new user             |
| PUT        | /api/users/:id   | Update an existing user    |
| DELETE     | /api/users/:id   | Remove a user              |

---

## 📌 Chapter 2: Building a RESTful API with Node.js (Express)

### 🛠 Step 1: Setup Node.js Project
1. Install **Node.js** from [nodejs.org](https://nodejs.org/).
2. Initialize a project:
   ```sh
   mkdir rest-api-node
   cd rest-api-node
   npm init -y
   ```

### 🔧 Step 2: Install Required Packages
```sh
npm install express cors
npm install -g nodemon  # (optional for auto-restart)
```

### 📜 Step 3: Create `server.js`
```js
const express = require('express');
const cors = require('cors');
const app = express();
const port = 5000;

app.use(cors());
app.use(express.json());

let users = [
  { id: 1, name: 'John Doe', email: 'john.doe@example.com' },
  { id: 2, name: 'Jane Smith', email: 'jane.smith@example.com' },
];

// 🚀 GET all users
app.get('/api/users', (req, res) => res.json(users));

// 🔍 GET a user by ID
app.get('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).send('User not found');
  res.json(user);
});

// ✏️ POST a new user
app.post('/api/users', (req, res) => {
  const user = { id: users.length + 1, name: req.body.name, email: req.body.email };
  users.push(user);
  res.status(201).json(user);
});

// 🛠️ PUT (update) a user
app.put('/api/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).send('User not found');
  user.name = req.body.name;
  user.email = req.body.email;
  res.json(user);
});

// ❌ DELETE a user
app.delete('/api/users/:id', (req, res) => {
  const index = users.findIndex(u => u.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).send('User not found');
  users.splice(index, 1);
  res.status(204).send();
});

// 🎉 Start the server
app.listen(port, () => console.log(`✅ Server running on http://localhost:${port}`));
```

### 🚀 Step 4: Run the API Server
```sh
nodemon server.js  # or node server.js
```

---

## 📌 Chapter 3: Building a RESTful API with Flask (Python)

### 🛠 Step 1: Install Flask
```sh
pip install flask flask-cors
```

### 🔧 Step 2: Create `app.py`
```python
from flask import Flask, jsonify, request
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

users = [
    {"id": 1, "name": "John Doe", "email": "john.doe@example.com"},
    {"id": 2, "name": "Jane Smith", "email": "jane.smith@example.com"},
]

@app.route('/api/users', methods=['GET'])
def get_users():
    return jsonify(users)

@app.route('/api/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    user = next((u for u in users if u["id"] == user_id), None)
    return jsonify(user) if user else (jsonify({"error": "User not found"}), 404)

@app.route('/api/users', methods=['POST'])
def create_user():
    data = request.json
    new_user = {"id": len(users) + 1, "name": data["name"], "email": data["email"]}
    users.append(new_user)
    return jsonify(new_user), 201

@app.route('/api/users/<int:user_id>', methods=['PUT'])
def update_user(user_id):
    user = next((u for u in users if u["id"] == user_id), None)
    if user:
        user.update(request.json)
        return jsonify(user)
    return jsonify({"error": "User not found"}), 404

@app.route('/api/users/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    global users
    users = [u for u in users if u["id"] != user_id]
    return '', 204

if __name__ == '__main__':
    app.run(debug=True)
```

### 🚀 Step 3: Run the API Server
```sh
python app.py
```

---

## 🎯 Conclusion
Congratulations! You just built RESTful APIs in both **Node.js** and **Flask**! 🚀🔥

✅ **Next Steps:**
- Secure the API with authentication (JWT, OAuth).
- Connect to a real database (MongoDB, PostgreSQL, MySQL).
- Deploy your API (Heroku, AWS, etc.).

Happy Coding! 🎉
