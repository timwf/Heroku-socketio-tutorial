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

Tutorial can be found at 

https://medium.com/@timwfowler0/socket-io-reactjs-node-js-heroku-deployment-48b139581e9f
