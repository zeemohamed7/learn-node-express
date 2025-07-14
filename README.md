# Node.js + Express.js + MongoDB: A Beginner's Guide

In this tutorial, I will walk you through the process of setting up Express, Node.js, two popular frameworks used for building web applications, along with MongoDB.

> ⚠️ Note: The way I’ve organized folders, named files, and handled exports/imports might differ from what you’ve seen before, but the core idea and functionality remain the same.

## Table of Contents

- [Node.js](#nodejs)
- [Express.js](#expressjs)
- [Setting Up](#setting-up)
- [Building a Basic Server](#building-a-basic-expressjs-server)
- [Nodemon](#usingnode-and-nodemon)
- [Environment Variables & MongoDB](#setting-up-mongoose--dotenv)
- [Models, Routes, Controllers](#models-routes-and-controllers)
- [Final Project Structure](#final-project-structure)
- [Extra Resources](#extra-resources)

## Node.js:

Node.js is an **open-source JavaScript runtime environment** that allows you to run JavaScript on the server-side. It enables developers to build backend applications using JavaScript.

## Express.js:

Express.js is a **minimal and flexible web application framework** for Node.js that simplifies building web apps and APIs. It provides tools for routing, middleware, and more.

Think of it as a **set of Lego blocks** to help you quickly build websites or web applications.

In summary, Node.js allows us to use JS on the server-side, and Express.js provides a convenient way to build web applications using Node.js by providing pre-built tools and functionality.

---

## Setting up

### Install Node.js

#### ⚠️ Note: this only needs to be done once, if you’ve done this before, skip this step!

1. Go to [nodejs.org](https://nodejs.org) and download the latest version for your OS.
2. Run the installer and follow the instructions.
3. Verify installation by running:
   ```bash
   node -v
   ```
   It should display the installed Node.js version.

---

## Getting Started with Express.js

1. Create a new project, navigate into it and create server.js

```bash
mkdir express-project
cd express-project
touch server.js
```

2. Run this command to initialize new Node.js project

```bash
npm init
# or use the -y flag to skip prompts
npm init --y
```

After you have provided the necessary information, `npm init` will generate a package.json file in your project directory with the configured settings. This file serves as a manifest for your project and contains information about its dependencies, scripts, and other metadata.

The prompts at the end will look like this:

```js
package name: (my-project)
version: (1.0.0)
description:
entry point: (server.js) // NOTE!
test command:
git repository:
keywords:
author:
license: (ISC)
```

> ⚠️ Note: Make sure the entry point is server.js if you created that file.

3. Install Express

```bash
   npm install express
```

Congrats! You’ve set up your project with Node.js and Express, next step is building a basic Express.js server.

Here’s what we have so far:
![node.js](https://github.com/zeemohamed7/learn-node-express/assets/142171425/2fef2c40-39e2-4a6f-8a27-4ec24f71f7db)

---

## Building a Basic Express.js Server:

1. Open server.js in VS code
2. Import Express.js module:

```js
const express = require('express')
```

3. Create an instance of the Express application:

```js
const app = express()
```

4. Define a route that responds with "Hello, World!":

```js
app.get('/', (req, res) => {
  res.send('Hello, World!')
})
```

5. Add middleware

This middleware parses incoming JSON requests and puts the parsed data in req.body. Without it, req.body would be undefined.

```js
app.use(express.json())
```

Use this when the client sends JSON:

```json
{
  "name": "Zainab",
  "email": "zainab@example.com"
}
```

This middleware parses application/x-www-form-urlencoded payloads (typical of HTML forms).

```js
express.urlencoded({ extended: true })
```

Use this if you’re handling form submissions like:

```html
<form method="POST" action="/submit">
  <input name="title" />
  <input name="description" />
</form>
```

<details><summary>Explanation</summary>

We are using express (as an instance) and calling get() to make a GET request on ‘/’ root url

The arrow function has two parameters: _req_ and _res_.

`req` stands for "request" and contains information about the incoming request from the user, such as the URL, headers, and any data sent. `res` stands for "response" and is used to send a response back to the user's browser.

`res.send('Hello, World!')`: Here, we are using the res object to send a response back to the user's browser. It's like telling the computer to send a message saying "Hello, World!" back to the user's browser when they visit ‘/’

</details>

5. Start the server:

```js
app.listen(3000, () => {
  console.log('Server started on port 3000')
})
```

<details><summary>Explanation</summary>

We are asking our Express application to start listening for incoming requests on a specific port number (3000 in this case)

Port numbers and salt numbers are usually stored in .env files

</details>
So far we have:

![Screenshot 2024-02-19 at 10 02 39 am](https://github.com/zeemohamed7/learn-node-express/assets/142171425/e3f8a894-ce19-4a54-8a1a-34729cc19502)

5. Testing:
   Make sure you are in the root of your project
   `node server.js`

Open a web browser and visit http://localhost:3000. You should see the message "Hello, World!" displayed.

#### Using`node` and `nodemon`

When running node server.js, you are executing the server.js file using the Node.js runtime. It starts the server and runs the code within server.js.

However, if you make any changes to the server.js file, you need to manually stop and restart the server using node server.js again for those changes to take effect.

On the other hand, nodemon automatically restarts the server whenever changes are made to the files.

To install nodemon, run:

```bash
npm -g nodemon
```

Then, instead of running node server.js, run:

```bash
nodemon server.js
```

This will start the server and automatically restart it whenever changes are made to the server.js file.

_Note:_ Installing `nodemon` globally isn't recommended, instead:

1. Install it as a development dependency:

```bash
npm install --save-dev nodemon
```

2. Open your `package.json` file and locate the "scripts" section. If it doesn't exist, you can add it.

3. Inside the "scripts" section, add a new entry for "dev" (or any other name you prefer) and set the value to `nodemon server.js`. For example:

```js
"scripts": {
  "dev": "nodemon server.js"
}
```

4. Run your server using

```bash
npm run dev
```

# Other dependencies

We import `dotenv` to load environment variables, and mongoose to work with MongoDB databases.

1. Install other backend dependencies such as dotenv and mongoose

```bash
npm install dotenv mongoose
```

2. Import mongoose using:

```js
const mongoose = require('mongoose')
```

## Setting Up Mongoose & Dotenv:

1. Connect to a MongoDB database by calling the mongoose.connect() function and passing in the MongoDB connection URL:

#### Make sure to replace `'mongodb://localhost/mydatabase'`` with your own connection string!

```js
mongoose.connect('mongodb://localhost/mydatabase')

mongoose.connection.on('connected', () => {
  console.log(`Connected to ${mongoose.connection.name}`)
})
```

Keep your connection URI in your .env file so others won’t have access to it, we do this using dotenv:

2. `.env` and `.gitignore` files

- Create a `.env `file
- Create a `.gitignore` file
- Add this to `.gitignore`:

```
.env
node_modules
```

3. In .env file:

```
 MONGODB_URI=mongodb://localhost/mydatabase
```

- key and value pair
- convention to keep key in all capital letters
  > ⚠️ Note: Make sure to replace this with your own connection string!

4. Import dotenv and config in server.js

```js
require('dotenv').config()
```

6. Instead of connection string, put process.env.MONGODB_URI

```js
mongoose.connect(process.env.MONGODB_URI)
```

---

# Models, Routes and Controllers

- Models: Models define the structure and behavior of data in our application. They tell us what information we need to store and how it should be organized.

- Controllers: Controllers handle the logic and actions for specific routes or endpoints. They process requests, interact with models, and prepare responses to be sent back to the client.

- Routes: Routes define URL patterns and HTTP methods that our application will respond to. They map incoming requests to the corresponding controller functions.

> ![Screenshot 2024-02-19 at 10 35 41 am](https://github.com/zeemohamed7/learn-node-express/assets/142171425/54178863-a8cd-4f00-9b9b-c9035d777e63)

We will create a simple to-do application to understand these.

### Creating models, routes, controllers

Create your folders

![Screenshot 2024-02-19 at 10 36 31 am](https://github.com/zeemohamed7/learn-node-express/assets/142171425/93a65932-d4ae-45e9-95d6-9846db6ee385)

### Models

1. Create your model (Todo.js) inside of models folder
2. Create a model for todo item:

```js
const mongoose = require('mongoose')
// Create Schema
const todoSchema = mongoose.Schema(
  {
    title: { type: String, required: true },
    description: { type: String }
  },
  {
    timestamps: true
  }
)

// Create model from the schema
const Todo = mongoose.model('Todo', todoSchema)

// Export Todo model
module.exports = Todo
```

When creating a model in Mongoose, you always follow the same steps.

First, you import Mongoose so you can use its tools. Next, you create a schema - this is like making a blueprint of a toy, where you define what parts (or pieces of data) the toy should have.

After that, you register the model by telling Mongoose about the blueprint — this is like registering your toy design with a factory so it knows how to build it.

Finally, you export the model so other parts of your application can use it to create, read, update, or delete toys (or data) in the database.

### Controllers

1. Create controller (todosController.js) inside of controllers folder
2. Import Todo model

```js
const Todo = require('../models/Todo')
```

3. In this file, you can define your CRUD operations (Create, Read, Update and Delete). For example, to create a todo item:

```js
// Create a new todo item
const createTodo = async (req, res) => {
  try {
    const { title, description } = req.body

    const todo = await Todo.create({ title, description })

    res.status(201).json(todo)
  } catch (error) {
    res.status(500).json({ error: 'Failed to create todo item' })
  }
}

module.exports = createTodo
```

<details>

  <summary>Click for explanation of each line</summary>

```js
 async (req, res) => { ... }
```

We write `async` before a function when we plan to use `await` inside it. This tells JavaScript that the function will run asynchronously. In other words, it might need to "wait" for some operations (like talking to a database) to finish before moving on.

```js
const { title, description } = req.body
```

This extracts the title and description properties from the `req.body` object. The req.body object contains the data sent by the client in the request body.

```js
const todo = await Todo.create({ title, description })
```

This line creates a new todo item in the database using the Todo model (which should have been defined and imported earlier). The `await` keyword tells the program to pause and wait for the database operation to finish before moving on. Once it’s done, the new todo item is saved into the todo variable.

> Note: The code is wrapped in a try...catch block to handle any errors that might occur while creating the todo item. If the creation is successful, it sends a response with a 201 status code (which means the resource was created) and returns the new todo item as JSON. If there's an error, the catch block sends a 500 status code (internal server error) along with a JSON error message.

</details>

### Other controllers:

```js
// Get all todo items
const getAllTodos = async (req, res) => {
  try {
    // Find all todos in database using .find()
    const todos = await Todo.find()

    // Display those todos
    res.json(todos)
  } catch (error) {
    res.status(500).json({ error: 'Failed to retrieve todo items' })
  }
}

// Update a todo item
const updateTodo = async (req, res) => {
  try {
    const { id } = req.params
    const updatedTodo = await Todo.findByIdAndUpdate(id, req.body, {
      new: true
    })

    res.json(updatedTodo)
  } catch (error) {
    res.status(500).json({ error: 'Failed to update todo item' })
  }
}
```

<details>
  <summary>Click to expand</summary>

`const { id } = req.params;` extracts the id parameter from req.params. This assumes that the id parameter is passed in the request URL, such as /todos/:id, where :id represents the dynamic todo item ID.

`const updatedTodo = await Todo.findByIdAndUpdate(id, req.body, { new: true });` uses the Todo model to find a todo item by its id and update its property with the new todo. The { new: true } option ensures that the updated todo item is returned. The `await` keyword is used to wait for the update operation to complete before proceeding.

`res.json(updatedTodo);` sends the updated todo item as a JSON response to the client.

</details>

```js
// Delete a todo item
const deleteTodo = async (req, res) => {
  try {
    const { id } = req.params

    // Takes the ID of Todo you want to remove
    await Todo.findByIdAndRemove(id)

    res.json({ message: 'Todo item deleted successfully' })
  } catch (error) {
    res.status(500).json({ error: 'Failed to delete todo item' })
  }
}

module.exports = {
  createTodo,
  getAllTodos,
  updateTodo,
  deleteTodo
}
```

### Routes

> In REST APIs, we use different HTTP methods:

- GET: retrieve data
- POST: create data
- PUT: update data
- DELETE: remove data

1. Create todoRoutes.js in routes folder
2. In the todoRoutes.js file:

```js
const express = require('express')
const router = express.Router()

// Import controllers file
const todosController = require('../controllers/todosController')

// Create a new todo item
router.post('/new', todosController.createTodo)

// Get all todo items
router.get('/', todosController.getAllTodos)

// Update a todo item
router.put('/:id', todosController.updateTodo)

// Delete a todo item
router.delete('/:id', todosController.deleteTodo)

module.exports = router
```

> Define the routes using the Express Router, specifying the URL paths and the corresponding controller functions to be executed when those routes are accessed.

> Export this router!

### Server.js

To tie everything back together:

1. In server.js, import and mount the routes

```js
// Import routes
const todosRoutes = require('./routes/todoRoutes')

// Mount the todos routes
app.use('/todos', todosRoutes)
```

Done!

## Final Project Structure

<img width="726" alt="Directory tree example" src="https://github.com/user-attachments/assets/8c1aa547-208e-4294-aa01-977e00243e7f">

Created by Zainab Mohamed

## Extra Resources

- [Node.js Docs](https://nodejs.org/en/docs/)
- [Express Docs](https://expressjs.com/)
- [Mongoose Docs](https://mongoosejs.com/docs/)
- [Dotenv Guide](https://www.npmjs.com/package/dotenv)
- [Creating salt rounds](https://www.uuidgenerator.net/version4)
