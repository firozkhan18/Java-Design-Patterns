Sure! I'll explain several common microservices design patterns and provide example code snippets for each to illustrate their usage.

### 1. **API Gateway**

**Use:** An API Gateway routes requests from clients to the appropriate microservices. It also handles cross-cutting concerns like authentication and logging.

**Example Code:**

Here's a simple example using Node.js with Express for an API Gateway:

```javascript
// api-gateway.js
const express = require('express');
const axios = require('axios');
const app = express();
const port = 3000;

app.use(express.json());

app.get('/users/:id', async (req, res) => {
  const userId = req.params.id;
  try {
    const response = await axios.get(`http://localhost:4000/users/${userId}`);
    res.send(response.data);
  } catch (error) {
    res.status(500).send('Service unavailable');
  }
});

app.listen(port, () => {
  console.log(`API Gateway listening at http://localhost:${port}`);
});
```

In this example, the API Gateway forwards requests to a user service running on port 4000.

### 2. **Service Discovery**

**Use:** Allows services to find each other dynamically. Service registries maintain a list of available services and their locations.

**Example Code:**

Using `Consul` for service discovery:

**Service Registration:**

```javascript
// user-service.js
const consul = require('consul')();
const express = require('express');
const app = express();
const port = 4000;

app.get('/users/:id', (req, res) => {
  res.send({ userId: req.params.id, name: 'John Doe' });
});

app.listen(port, () => {
  consul.agent.service.register({
    name: 'user-service',
    address: 'localhost',
    port: port
  }, (err) => {
    if (err) throw err;
    console.log(`User service listening at http://localhost:${port}`);
  });
});
```

**Service Discovery Query:**

```javascript
// client-service.js
const consul = require('consul')();

consul.catalog.service.list((err, result) => {
  if (err) throw err;
  console.log('Available services:', result);
});
```

### 3. **Circuit Breaker**

**Use:** Protects a service from repeated failures by monitoring its health and preventing requests if it’s failing.

**Example Code:**

Using `opossum` for a circuit breaker in Node.js:

```javascript
// circuit-breaker.js
const CircuitBreaker = require('opossum');
const axios = require('axios');

const breaker = new CircuitBreaker(() => axios.get('http://localhost:4000/users/1'));

breaker.fallback(() => 'Service is down');

breaker.on('open', () => console.log('Circuit breaker opened'));
breaker.on('close', () => console.log('Circuit breaker closed'));
breaker.on('halfOpen', () => console.log('Circuit breaker half-open'));

breaker.fire().then(console.log).catch(console.error);
```

### 4. **Saga**

**Use:** Manages distributed transactions across multiple services by executing a sequence of operations and handling compensations if needed.

**Example Code:**

Implementing a simple saga in Node.js:

```javascript
// saga.js
const axios = require('axios');

async function bookOrder(orderId) {
  try {
    await axios.post('http://localhost:5000/charge', { orderId });
    await axios.post('http://localhost:6000/inventory', { orderId });
  } catch (error) {
    console.error('Error in saga:', error);
    await axios.post('http://localhost:6000/cancelInventory', { orderId });
    await axios.post('http://localhost:5000/refund', { orderId });
  }
}

bookOrder('12345');
```

### 5. **Event Sourcing**

**Use:** Stores the state of an application as a series of events. The state is reconstructed by replaying these events.

**Example Code:**

**Event Storage:**

```javascript
// event-store.js
const events = [];

function storeEvent(event) {
  events.push(event);
  console.log('Event stored:', event);
}

storeEvent({ type: 'USER_CREATED', data: { userId: '1', name: 'John Doe' } });
```

**Event Replay:**

```javascript
// event-replay.js
const events = [
  { type: 'USER_CREATED', data: { userId: '1', name: 'John Doe' } }
];

function replayEvents() {
  events.forEach(event => {
    if (event.type === 'USER_CREATED') {
      console.log('Replaying event:', event);
    }
  });
}

replayEvents();
```

### 6. **CQRS (Command Query Responsibility Segregation)**

**Use:** Separates read and write operations into different models to optimize performance and scalability.

**Example Code:**

**Command Service:**

```javascript
// command-service.js
const express = require('express');
const app = express();
const port = 7000;

app.use(express.json());

app.post('/createUser', (req, res) => {
  // Logic to create a user
  res.status(201).send('User created');
});

app.listen(port, () => {
  console.log(`Command service listening at http://localhost:${port}`);
});
```

**Query Service:**

```javascript
// query-service.js
const express = require('express');
const app = express();
const port = 8000;

app.get('/user/:id', (req, res) => {
  // Logic to fetch user details
  res.send({ userId: req.params.id, name: 'John Doe' });
});

app.listen(port, () => {
  console.log(`Query service listening at http://localhost:${port}`);
});
```

### 7. **Strangler Fig**

**Use:** Gradually replaces parts of a monolith with microservices, allowing incremental migration.

**Example Code:**

**Monolith Endpoint (Old):**

```javascript
// old-monolith.js
const express = require('express');
const app = express();
const port = 9000;

app.get('/api/users/:id', (req, res) => {
  // Old monolithic logic
  res.send({ userId: req.params.id, name: 'John Doe' });
});

app.listen(port, () => {
  console.log(`Old monolith listening at http://localhost:${port}`);
});
```

**Microservice Endpoint (New):**

```javascript
// user-microservice.js
const express = require('express');
const app = express();
const port = 10000;

app.get('/users/:id', (req, res) => {
  // New microservice logic
  res.send({ userId: req.params.id, name: 'John Doe' });
});

app.listen(port, () => {
  console.log(`User microservice listening at http://localhost:${port}`);
});
```

In the migration phase, the API Gateway would route `/api/users/:id` to either the old monolith or the new microservice, depending on which part of the system has been migrated.

### 8. **Database per Service**

**Use:** Each microservice has its own database, promoting loose coupling between services.

**Example Code:**

**User Service with MongoDB:**

```javascript
// user-service.js
const express = require('express');
const mongoose = require('mongoose');
const app = express();
const port = 11000;

mongoose.connect('mongodb://localhost/userdb', { useNewUrlParser: true, useUnifiedTopology: true });

app.get('/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  res.send(user);
});

app.listen(port, () => {
  console.log(`User service listening at http://localhost:${port}`);
});
```

**Order Service with PostgreSQL:**

```javascript
// order-service.js
const express = require('express');
const { Client } = require('pg');
const app = express();
const port = 12000;

const client = new Client({ connectionString: 'postgresql://localhost/orderdb' });
client.connect();

app.get('/orders/:id', async (req, res) => {
  const result = await client.query('SELECT * FROM orders WHERE id = $1', [req.params.id]);
  res.send(result.rows[0]);
});

app.listen(port, () => {
  console.log(`Order service listening at http://localhost:${port}`);
});
```

In this example, the User Service and Order Service each use different databases.

These code examples illustrate the core concepts of each microservices design pattern and how they can be implemented in practice.
