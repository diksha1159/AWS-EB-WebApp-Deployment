## Introduction

Hey there! In this module, we're going to build a Node.js web application that will serve an HTML file and also have a simple REST API. So what exactly are we doing here? We're creating a non-containerized application that we'll eventually deploy to the cloud.

To get started, we're going to use Node.js to build a web app server. Our web app will be a simple one that serves static HTML files and has a REST API endpoint. We'll get our hands dirty with some coding and before you know it, you'll have a running web application!

So, what will you accomplish by the end of this module? You'll develop a simple Node.js web app that serves an HTML file and has a simple REST API. And, the best part is that you'll be able to run the application locally on your computer. Sounds exciting, right? Let's get started!

---



First things first, let's create a new directory for our app. I'm gonna call mine "my\_webapp", but you can call it whatever you want. Just don't call it something boring like "webapp" or "app". We're better than that.

Next, we're gonna initialize the Node.js project. This creates a file called "package.json" that will hold all the info about your app. To do this, just run the command "npm init -y" in your terminal. If you don't have npm installed, you're gonna need to get on that ASAP.


here are the steps given below:

let's create a new directory for our application by typing in this command:

```bash
mkdir my_webapp
```

Then, change into this directory by running:

```bash
cd my_webapp
```

Now, we can initialize the Node.js project by typing in:

```csharp
npm init -y
```

This creates the `package.json` file that will contain all the definitions of your Node.js application. If you don't have npm installed, no worries! Just follow the instructions found at Setting up your Node.js development environment.



Now, we're gonna install Express. This is the framework we're gonna use to build our app. To do this, just run "npm install express" in your terminal. This will download all the files we need and add them to our project's "node\_modules" directory.

Now comes the fun part. We're gonna write some code! Create a file called "app.js". This is where we'll put all the logic for our app.

In "app.js", we're gonna start by requiring Express and setting up our server. We'll also define the port we want to use (I'm gonna go with 8080 because it's a cool number).

Create Express app We're going to use Express as our web application framework. To use it, we need to install Express as a dependency in our Node.js project by running:

`npm install express`

After running this command, you will see the dependency appear in the `package.json` file. Additionally, the `node_modules` directory and `package-lock.json` files will be created.

Now, we can create a new file called `app.js`. This file will contain the business logic for where our Node.js Express server will reside.

We're now ready to start adding some code. The first thing we need to add is the dependencies for the app—in this case, adding Express to allow use of the module we previously installed, and then the code to start up the web server. We will specify the web server to use port 8080, and fun fact, that is what Elastic Beanstalk uses by default. Here's the code you can use:

javascript

```javascript
var express = require('express');
var app = express();
var fs = require('fs');
var port = 8080;

app.listen(port, function() {
  console.log('Server running at http://127.0.0.1:', port);
});
```

We can start up our application now, but it won't do anything yet as we have not defined any code to process requests.



Create a REST API We will now add code to serve a response for a HTTP REST API call. To create our first API call, add the following code in the `app.js` file:

javascript

```javascript
var express = require('express');
var app = express();
var fs = require('fs');
var port = 8080;

// New code
app.get('/test', function (req, res) {
    res.send('the REST endpoint test run!');
});


app.listen(port, function() {
  console.log('Server running at http://127.0.0.1:%s', port);
});
```

This is just to illustrate how to connect the `/test` endpoint to our code; you can add in a different response, or code that does something specific, but that is outside the scope of this tutorial.

Serve HTML content Our Express Node.js application can also serve a static web page. We need to create an HTML page to use as an example. Let's create a file called `index.html`.

Inside this file, add the following HTML with a link to the REST endpoint we created earlier to show how it connects to the backend.

html

```html
<html>
    <head>
        <title>Elastic Beanstalk App</title>
    </head>

    <body>
        <h1>Welcome to the demo for ElasticBeanstalk</h1>
        <a href="/test">Call the test API</a>
    </body>
</html>
```

To serve this HTML page from our Express server, we need to add some more code to render the `/` path when it is called. To do this, add the following code above the **/test** call:

```javascript
app.get('/', function (req, res) {
    html = fs.readFileSync('index.html');
    res.writeHead(200);
    res.write(html);
    res.end();
});
```

This code will serve the **index.html** file whenever a request for the root of the app (/) is made.

Running application Locally:

It's time to put our programming skills to the test and see if our application is functioning properly on our local machine. To simplify the process, we'll be updating our package.json file with a nifty little script that makes running the app a breeze.

update the package.json file's scripts section


replace the **scripts **section with:

```json
"scripts": {
    "start": "node app.js"
  },
```


All you have to do is head over to your terminal and execute the following command: "npm start". This will fire up a local server with the URL "http://127.0.0.1:8080" or "http://localhost:8080" - pretty neat, huh?

Now, just copy and paste that URL into your favorite browser and behold the magic that unfolds before your very eyes. If everything's working as it should, you should see a wonderful display of your application's awesomeness.

Go forth, and bask in the glory of your hard work!

To stop the server, press **ctrl + c** to stop the process in your terminal where you ran  **npm start** .

see it was easy right? We just finished building our first Node.js application and made sure it works locally. Exciting stuff, right? In the upcoming block, we'll dive into the world of AWS and use CDK to create the infrastructure needed to run our app on Elastic Beanstalk.