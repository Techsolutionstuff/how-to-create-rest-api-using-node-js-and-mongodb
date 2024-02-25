# How to Create REST API using Node.js and MongoDB

<p>Hey there! Welcome to my article on creating a RESTful API using Node.js and MongoDB. If you&#39;re looking to build powerful and scalable web applications, understanding how to create a robust API is essential.</p>

<p>In this guide, I&#39;ll walk you through the process of building a REST API step by step using express.js and mongoDB.</p>

<p>A REST API utilizes various HTTP methods to perform operations on resources. It typically employs a GET request to retrieve a record, a POST request to create one, a PUT request to update a record, and a DELETE request to delete one.</p>

<p>These HTTP methods provide a standardized way for clients to interact with the server and perform CRUD (Create, Read, Update, Delete) operations on resources.</p>

<p>So, let&#39;s see how to create a REST API using node js and MongoDB, express.js, and MongoDB rest API, how to create a rest API with node js and express, and how to create REST API in node.js.</p>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 1: Setup Development Environment</strong></div>

<p>Ensure Node.js is installed on your machine. You can download it from the official Node.js website and follow the installation instructions.</p>

<p>Install MongoDB Community Edition from the official MongoDB website and set it up on your local machine or use a cloud-based MongoDB service like MongoDB Atlas.</p>

<p>&nbsp;</p>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 2: Initialize Node.js Project</strong></div>

<ul>
</ul>

<ul>
</ul>

<p>Create a new directory for your project. Initialize a new Node.js project using npm (Node Package Manager):</p>

<pre>
<code>mkdir my-app
cd my-app
npm init -y</code></pre>

<p>&nbsp;</p>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 3: Install Dependencies</strong></div>

<ul>
</ul>

<p>Install necessary dependencies such as Express.js (for building the API) and Mongoose (for interacting with MongoDB):</p>

<pre>
<code class="language-javascript">npm install -g express-generator
npx express --view=ejs
npm install
npm install express-flash --save
npm install express-session --save
npm install body-parser --save
npm install cors --save
npm install mongoose
</code></pre>

<p>&nbsp;</p>

<p><a href="https://www.npmjs.com/package/body-parser"><strong>body-parser:</strong></a>&nbsp;Parse incoming request bodies in a middleware before your handlers, available under the&nbsp;<code>req.body</code>&nbsp;property.</p>

<p><strong><a href="https://www.npmjs.com/package/express-flash" target="_blank">Express-Flash</a>:</strong>&nbsp;Flash is an extension of&nbsp;<code>connect-flash</code>&nbsp;with the ability to define a flash message and render it without redirecting the request.</p>

<p><strong><a href="https://www.npmjs.com/package/express-session" target="_blank">Express-Session</a>:</strong>&nbsp;HTTP server-side framework used to create and manage a session middleware.</p>

<p><strong><a href="https://www.npmjs.com/package/express-ejs-layouts" target="_blank">Express-EJS</a>:</strong>&nbsp;EJS is&nbsp;a simple templating language&nbsp;that is used to generate HTML markup with plain JavaScript. It also helps to embed JavaScript to HTML pages</p>

<p><strong><a href="https://www.npmjs.com/package/mongoose" target="_blank">Mongoose</a>:</strong>&nbsp;Mongoose is a&nbsp;<a href="https://www.mongodb.com/" target="_blank">MongoDB</a>&nbsp;object modeling tool designed to work in an asynchronous environment. Mongoose supports both promises and callbacks.</p>

<p>&nbsp;</p>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 4: Connect to MongoDB</strong></div>

<ul>
</ul>

<p>Set up a connection to your MongoDB database using Mongoose:</p>

<p><strong>database.js</strong></p>

<pre>
<code class="language-javascript">var mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/my_database', {useNewUrlParser: true});

var conn = mongoose.connection;

conn.on('connected', function() {
    console.log('database is connected successfully');
});

conn.on('disconnected',function(){
    console.log('database is disconnected successfully');
});

conn.on('error', console.error.bind(console, 'connection error:'));

module.exports = conn;</code></pre>

<p>&nbsp;</p>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 5: Define Schema and Model</strong></div>

<p>Define a schema for your MongoDB documents and create a model using Mongoose. This will define the structure of your data.</p>

<p><strong>userModel.js</strong></p>

<pre>
<code class="language-javascript">var db = require("../database");
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
 
var UserSchema = new Schema({
    title: String,
    author: String,
    category: String
});
 
module.exports = mongoose.model('User', UserSchema);
</code></pre>

<pre>
&nbsp;</pre>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 6: Implement CRUD Operations</strong></div>

<p>Implement controller functions for handling CRUD operations on your MongoDB data.</p>

<p><strong>users.js</strong> route file</p>

<pre>
<code class="language-javascript">var express = require('express');
var User = require('../models/user');
var router = express.Router();
 
router.get('/', function(req, res){
    console.log('getting all users');
    User.find({}).exec(function(err, users){
        if(err) {
            res.send('error has occured');
        } else {
            console.log(users);
            res.json(users);
        }
    });
});
 
router.get('/:id', function(req, res){
    console.log('getting one user');
    User.findOne({
        _id: req.params.id
    }).exec(function(err, user){
        if(err) {
            res.send('error has occured');
        } else {
            console.log(user);
            res.json(user);
        }
    });
});
 
router.post('/', function(req, res){
    var newUser = new User();
    newUser.title = req.body.title;
    newUser.author = req.body.author;
    newUser.category = req.body.category;
    newUser.save(function(err, user){
        if(err) {
            res.send('error saving user');
        } else {
            console.log(user);
            res.send(user);
        }
    });
});
 
router.put('/:id', function(req, res){
    User.findOneAndUpdate({
        _id: req.params.id
    },{
        $set: {
            title: req.body.title,
            author: req.body.author,
            category: req.body.category
        }
    },{
        upsert: true
    },function(err, newUser){
        if(err) {
            res.send('error updating user');
        } else {
            console.log(newUser);
            res.send(newUser);
        }
    });
});
 
router.delete('/:id', function(req, res){
    User.findByIdAndRemove({
        _id: req.params.id
    },function(err, user){
        if(err) {
            res.send('error deleting user');
        } else {
            console.log(user);
            res.send(user);
        }
    });
});
 
module.exports = router;
</code></pre>

<ul>
</ul>

<p>&nbsp;</p>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 7: Import Modules in App.js</strong></div>

<p>Import express flash session body-parser Mongoose dependencies in <strong>app.js</strong>&nbsp;as shown below.</p>

<pre>
<code class="language-javascript">const createError = require('http-errors');
const express = require('express');
const path = require('path');
const cookieParser = require('cookie-parser');
const logger = require('morgan');
const bodyParser = require('body-parser');
const cors = require('cors');
const users = require('./routes/users');
const app = express();
app.use(express.json());
app.use(bodyParser.json());
 
app.use(bodyParser.urlencoded({
    extended: true
}));
 
app.use(cookieParser());
 
app.use(cors());
 
app.use('/users', users);
 
// Handling Errors
app.use((err, req, res, next) =&gt; {
    // console.log(err);
    err.statusCode = err.statusCode || 500;
    err.message = err.message || "Internal Server Error";
    res.status(err.statusCode).json({
        message: err.message,
    });
});
 
app.listen(3000,() =&gt; console.log('Server is running on port 3000'));
</code></pre>

<p>&nbsp;</p>

<div style="background:#eeeeee;border:1px solid #cccccc;padding:5px 10px;"><strong>Step 8: Run App Server</strong></div>

<p>Start the app server using the following command.</p>

<pre>
<code>npm start</code></pre>

<p>&nbsp;</p>

<hr />
<p><strong><big>You might also like:</big></strong></p>

<ul>
	<li><strong><big>Read Also:&nbsp;<a href="https://techsolutionstuff.com/post/user-roles-and-permissions-using-centralized-database-in-laravel-10">User Roles and Permissions Using Centralized Database in Laravel 10</a></big></strong></li>
	<li><strong><big>Read Also:&nbsp;<a href="https://techsolutionstuff.com/post/how-to-install-and-setup-mongodb-in-laravel-10">How to Install and Setup MongoDB in Laravel 10</a></big></strong></li>
	<li><strong><big>Read Also:&nbsp;<a href="https://techsolutionstuff.com/post/how-to-insert-data-in-mongodb-using-node-js">How to Insert Data in MongoDB using Node.js</a></big></strong></li>
	<li><strong><big>Read Also:&nbsp;<a href="https://techsolutionstuff.com/post/laravel-10-mongodb-crud-operation">Laravel 10 MongoDB CRUD Operation</a></big></strong></li>
</ul>
