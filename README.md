# Introduction
In this tutorial, I will walk you through the process of setting up Express.js and Node.js, two popular frameworks used for building web applications.

## Node.js: 
Open-source JavaScript runtime environment that allows you to run JavaScript code on the server-side. It allows developers to build server-side applications using JavaScript.

## Express.js:
Express.js is like a toolbox or set of tools that makes building web applications using Node.js easier. It provides a collection of pre-built code that developers can use to handle common tasks in web development, such as managing routes (URLs) and handling requests and responses. 

It's like having a set of Lego blocks that you can use to quickly build a website or web application without starting from scratch!

In summary, Node.js allows us to use JS on the server-side, and Express.js provides a convenient way to build web applications using Node.js by providing pre-built tools and functionality.

---

## Setting up
Install Node.js
Note: this only needs to be done once, if you’ve done this before, skip this step!

1. Visit the official Node.js website (https://nodejs.org) and download the latest version compatible with your operating system.

2. Run the installer and follow the on-screen instructions to complete the installation process.

3. Verify the installation by opening a terminal or command prompt and running the command node -v. It should display the installed Node.js version.

---

## Getting Started with Express.js

1. Create a new project and navigate into it
` mkdir express-project 
cd express-project `

2. Run this command to initialize new Node.js project
`npm init
// or npm init —-y to skip prompts `

The prompts will look like this:
```
package name: (my-project)
version: (1.0.0)
description: A simple web application
entry point: (index.js) // NOTE!
test command:
git repository:
keywords: web application, node.js, express
author: John Doe
license: (ISC)
```
**Note:** Make sure to change entry point from index.js to server.js

After you have provided the necessary information, npm init will generate a package.json file in your project directory with the configured settings. This file serves as a manifest for your project and contains information about its dependencies, scripts, and other metadata.

3. Install Express.js by running the command
`npm install express`

4. Create a new file named server.js in the project directory
`mkdir server.js`

Congrats! You’ve set up your project with Node.js and Express, next step is building a basic Express.js server. Here’s what we have so far:
![node.js](https://github.com/zeemohamed7/learn-node-express/assets/142171425/2fef2c40-39e2-4a6f-8a27-4ec24f71f7db)

---

## Building a Basic Express.js Server:
1. Open server.js in VS code
2. Import Express.js module:
`const express = require('express') `


3. Create an instance of the Express application:
`const app = express()`


4. Define a route that responds with "Hello, World!":
```
app.get('/', (req, res) => {
  res.send('Hello, World!');
});
```

**Explanation:**

> We are using express (as an instance) and calling get() to make a GET request on ‘/’ root url 
> The arrow function has two parameters: *req* and *res*. 
> *req* stands for "request" and contains information about the incoming request from the user, such as the URL, headers, and any data sent. *res* stands for "response" and is used to send a response back to the user's browser.
> res.send('Hello, World!'): Here, we are using the res object to send a response back to the user's browser. It's like telling the computer to send a message saying "Hello, World!" back to the user's browser when they visit ‘/’


5. Start the server: 
```
app.listen(3000, () => {
  console.log('Server started on port 3000');
});
```


> We are asking our Express application to start listening for incoming requests on a specific port number (3000 in this case)
>
> Port numbers are usually stored in .env files as per convention

So far we have: 

![Screenshot 2024-02-19 at 10 02 39 am](https://github.com/zeemohamed7/learn-node-express/assets/142171425/e3f8a894-ce19-4a54-8a1a-34729cc19502)

5. Testing:
   Make sure you are in the root of your project
`` node server.js ``

Open a web browser and visit http://localhost:3000. You should see the message "Hello, World!" displayed.


** Note: node and nodemon **
When running node server.js, you are executing the server.js file using the Node.js runtime. It starts the server and runs the code within server.js. However, if you make any changes to the server.js file, you need to manually stop and restart the server using node server.js again for those changes to take effect. 
On the other hand, nodemon automatically restarts the server whenever changes are made to the files.

To install nodemon, run:
`` npm -g nodemon ``


# Other dependencies
We import dotenv to load environment variables, and mongoose to work with MongoDB databases.

1. Install other backend dependencies such as dotenv and mongoose
`` npm install dotenv mongoose ``


2. Import mongoose
`` const mongoose = require('mongoose') ``


## Setting Up Mongoose & Dotenv:
1. Connect to a MongoDB database by calling the mongoose.connect() function and passing in the MongoDB connection URL:
```
mongoose.connect('mongodb://localhost/mydatabase', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    // Connection successful
    console.log('Connected to MongoDB');
  })
  .catch((error) => {
    // Connection failed
    console.error('Error connecting to MongoDB:', error);
  });
```

It is best to keep your connection URL in your .env file so others won’t have access to it, we do this using dotenv:

2. .env and .gitignore files
- Create a .env file
- Create a .gitignore file
- Add this to gitignore:
```
.env
node_modules
```

3. In .env file: 
`` DB_CONNECTION = mongodb://localhost/mydatabase ``
- key and value pair
- convention to keep key in all capital letters


4. Import dotenv and config in server.js
`` require('dotenv').config() ``


5. Instead of connection string, put process.env.DB_CONNECTION
```
mongoose.connect(process.env.DB_CONNECTION, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    // Connection successful
    console.log('Connected to MongoDB');
  })
  .catch((error) => {
    // Connection failed
    console.error('Error connecting to MongoDB:', error);
  });
```
---

# Models, Routes and Controllers
- Models: Models define the structure and behavior of data in our application. They tell us what information we need to store and how it should be organized. 
- Controllers: Controllers handle the logic and actions for specific routes or endpoints. They process requests, interact with models, and prepare responses to be sent back to the client. 
- Routes: Routes define URL patterns and HTTP methods that our application will respond to. They map incoming requests to the corresponding controller functions.

We will create a simple to-do application to understand these.

## Creating models, routes, controllers


### Models

1. Create your model (Todo.js) inside of models folder
2. Create a model for todo item:
```
const mongoose = require('mongoose')
// Create Schema
const todoSchema = mongoose.Schema ({
title: {type: String, required: true},
description: {type: String},
daysToComplete: {type: Number, required: true}
}, {
timestamps: true
})

// Create model from the schema
const Todo = mongoose.model('Todo', todoSchema)

// Export Todo model
module.exports = Todo;
```

### Controllers
1. Create controller (todosController.js) inside of controllers folder
2. Import Todo model
` const Todo = require('./models/todo') `


3. In this file, you can define your CRUD operations (Create, Read, Update and Delete)
For example, to create a todo item:
```
// Create a new todo item
const createTodo = async (req, res) => {
  try {
    const { title, description } = req.body;
    const todo = await Todo.create({ title, description });
    res.status(201).json(todo);
  } catch (error) {
    res.status(500).json({ error: 'Failed to create todo item' });
  }
};

module.exports = createTodo
```



> *async (req, res) => { ... }*: we use async because of await
> *const { title, description } = req.body*: extract the title and description properties from the req.body object. The req.body object contains the data sent by the client in the request body.
> *const todo = await Todo.create({ title, description })*: uses the Todo model (assumed to be imported and defined earlier) to create a new todo item in the database. The await keyword is used because the Todo.create() function is an asynchronous operation that returns a promise. By using await, we wait for the promise to resolve and assign the created todo item to the todo variable.

### Other controllers: 
```
// Get all todo items
const getAllTodos = async (req, res) => {
  try {
// Find all todos in database using .find()
    const todos = await Todo.find();
Display those todos
    res.json(todos);
  } catch (error) {
    res.status(500).json({ error: 'Failed to retrieve todo items' });
  }
};

// Update a todo item
const updateTodo = async (req, res) => {
  try {
    const { id } = req.params;
    const { completed } = req.body;
    const updatedTodo = await Todo.findByIdAndUpdate(id, { completed }, { new: true });
    res.json(updatedTodo);
  } catch (error) {
    res.status(500).json({ error: 'Failed to update todo item' });
  }
};

// Delete a todo item
const deleteTodo = async (req, res) => {
  try {
    const { id } = req.params;
    await Todo.findByIdAndRemove(id);
    res.json({ message: 'Todo item deleted successfully' });
  } catch (error) {
    res.status(500).json({ error: 'Failed to delete todo item' });
  }
};

module.exports = {
  createTodo,
  getAllTodos,
  updateTodo,
  deleteTodo
};

```
> *const { id } = req.params;* extracts the id parameter from req.params. This assumes that the id parameter is passed in the request URL, such as /todos/:id, where :id represents the dynamic todo item ID.
> *const { completed } = req.body;* takes the new updated todo from req.body.
> *const updatedTodo = await Todo.findByIdAndUpdate(id, { completed }, { new: true });* uses the Todo model to find a todo item by its id and update its property with the new todo. The { new: true } option ensures that the updated todo item is returned as the result of the operation. The await keyword is used to wait for the update operation to complete before proceeding.
> *res.json(updatedTodo);* sends the updated todo item as a JSON response to the client.

### Routes
1. Create todoRoutes.js in routes folder
2. In the todoRoutes.js file:
```
const express = require('express');
const router = express.Router(); 

// Import controllers file
const todosController = require('./todosController'); 

// Create a new todo item
router.post('/todos/add', todosController.createTodo);

// Get all todo items
router.get('/todos', todosController.getAllTodos);

// Update a todo item
router.put('/todos/:id', todosController.updateTodo);

// Delete a todo item
router.delete('/todos/:id', todosController.deleteTodo);

module.exports = router;
```

> Define the routes using the Express Router, specifying the URL paths and the corresponding controller functions to be executed when those routes are accessed.
Export this router!

### Server.js
To tie everything back together:
1. In server.js, import and mount the routes
```
// Import routes
const todosRoutes = require('./todosRoutes');

// Mount the todos routes
app.use('/', todosRoutes);
```

Done!

<script>
  // Initialize highlight.js
  hljs.initHighlightingOnLoad();
</script>





