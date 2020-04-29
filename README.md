# Repository Supporting react + socket.io + nodeJs + Heroku Deployment

![Image](https://github.com/timwf/Heroku-socketio-tutorial/blob/master/readme-images/Group%201.png)


*I spend far too long trying to deploy my obligatory messaging app to Heroku built in ReactJs — this short tutorial is designed to help anyone struggling with the same issues and to internalize the process myself.*

**This step by step tutorial is aimed at developers with limited knowledge of back end technologies and is by no means intended for production of commercial apps — it will help you get your shiny new app up and running in the most basic way in my opinion.**


### Step one — create a react app
Create a react app in your chosen directory…

    npx create-react-app socket-messaging-app
   
   
### Step two — install your dependencies
I will be using npm to install the required packages in the root folder — make sure you cd into your root (socket-messaging-app in my case)

    cd socket-messaging-app
    npm install express socket.io socket.io-client`

### Step Three — Folder structure
Create-react-app should have created a “src” folder — Create a new folder in the “”src” folder and name it “server” create a file within that folder and name it “index.js”

![Image](https://github.com/timwf/Heroku-socketio-tutorial/blob/master/readme-images/image%208.png)



### Step Four — Create your server
Copy and paste the below code into your newly created index.js file-

    const express = require('express');
    const path = require('path');
    const app = express();
    const server = require('http').createServer(app);
    const io = require('socket.io')(server);
    const port = process.env.PORT || 3001;
    app.use(express.static(path.join(__dirname, '../../build')));
    app.get('/', (req, res, next) => res.sendFile(__dirname + './index.html'));
    io.on('connection', function(socket){
    io.emit('message from server', 'message from server - it works!')
    })
    server.listen(port);
    
This creates your express server and will use the built react app as its location (we haven't built the app just yet).
 
 
### Step Five — amend the React App.
Move back down to your src folder and locate the index.js file created by React. You will need to import the socket.io-client module and add the socket listening event from the server — your code should like this…

    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import * as serviceWorker from './serviceWorker';
    import io from 'socket.io-client'
    let socket = io("/");
    socket.on('message from server', function(msg){
    alert(msg);
    });
    ReactDOM.render(
    <React.StrictMode>
    <App />
    </React.StrictMode>,
    document.getElementById('root')
    );
    // If you want your app to work offline and load faster, you can change
    // unregister() to register() below. Note this comes with some pitfalls.
    // Learn more about service workers: https://bit.ly/CRA-PWA
    serviceWorker.unregister();
 
### Step Six — configure package.json and add Procfile
Heroku requires a Procfile for this to work. Move back into your root directory (socket-messaging-app in my case) and create a file named “Procfile” (no extension). Within this file add the following code:

    web: node ./src/server/index.js
_**Important Note** — update the node version in the code above to your current version of node — to find your version run node -v in your terminal._


### Step Seven — Build your app
In your terminal cd into the root directory (socket-messaging-app in my case)
    npm run-script build
You should now see a new folder “build” your project files should look like this..

![Image](https://github.com/timwf/Heroku-socketio-tutorial/blob/master/readme-images/image%209.png)

### Step Eight — Test your app! (optional)
In the terminal cd into you server folder and run
    node index.js

visit http://localhost:3001/ and you should now see the default react app in all its glory — and more importantly you should see an alert triggered by our server!

![Image](https://github.com/timwf/Heroku-socketio-tutorial/blob/master/readme-images/image%2010.png)


### Step Nine— Deploy to Heroku
You will need to have downloaded the Heroku CLI — visit https://devcenter.heroku.com/articles/heroku-cli if you haven't already done so.

_I am assuming you have git installed — visit https://git-scm.com/downloads if not!_

First we need to give the project a repository, add and commit the files — cd into the root folder (socket-messaging-app in my case) and run the following commands

    git init
    git add .
    git commit -m 'first deploy to heroku'

Now we need to create a heroku project login to your heroku account via the terminal..

    heroku login

you will be directed to the website where you will need to login.

Next lets create the app in the terminal..

    heroku create

Once the app has successfully deployed run

    heroku open

Voila! There you have it!

Tutorial can be found at 

https://medium.com/@timwfowler0/socket-io-reactjs-node-js-heroku-deployment-48b139581e9f
