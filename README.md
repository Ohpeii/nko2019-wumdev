# BACKEND DOCUMENTATION

__Current Features:__
> One Room Support
> User Events (userJoin & userDisconnect)
> Real-Time Communication by chat (Server: message, Client: userMessage)
> Random Username Color (Hex color code)

## Setup

### Server

Install the dependencies
```js
npm i
```
Start the server
```js
npm start
or
node index.js
```

### Client

Import Socket.IO
```js
// Javascript/HTML
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.3.0/socket.io.js"></script> // Via CDN
// Node.js
const io = require('socket.io-client');
// ES6
import io from 'socket.io-client';
```
Connect to server & Events
```js
<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.3.0/socket.io.js"></script>
<script>
    // Initialize
    var socket = io('http://localhost:3000');
    socket.emit('data', { // Must be emitted first 
        username
    });
    socket.on('clientData', function(data) {
        id
    });

    // Send Message
    socket.emit('message', "message");

    // User Events
    socket.on('userMessage', function(msg) {
        msg {
            message
            by
            id
        }
    });
		  
	socket.on('userJoin', function(u) {
        username
    });
		  
	socket.on('userDisconnect', u => {
        username
    });
</script>
```
Example:
```html
<script>
    var username = getfield("Please enter your username");
    while(username == '') username = getfield("Please enter your username");

    var socket = io("http://localhost:3000");
    var id;

    let init = false
    socket.on('connect', () => {
      socket.emit("data", {
        username
      });

      socket.on('err', e => {
        let err = e.code;
        switch(err) {
          case 0:
            username = getfield('Invalid username! Please re-enter your username!');
            socket.emit("data", {
              username
            });
            break;

          case 1:
            username = getfield('Username is already been used! Please enter another username!');
            socket.emit("data", {
              username
            });
            break;
        };
      })

      socket.on("clientData", data => {
        if (!init) {
          id = data.id;
          init = true;
        }
      });

      const form = document.querySelector('form');

      socket.on('userMessage', function (msg) {
        $('#messages').append('<li>').text(`${msg.by} » ${msg.message} - - - - ${new Date(Date.now())}`);
      });

      socket.on('userJoin', u => {
        $('#messages').append($('<li>')).text(`${u} joined - - - - ${new Date(Date.now())}`);
      });

      socket.on('userDisconnect', u => {
        $('#messages').append($('<li>')).text(`${u} left - - - - ${new Date(Date.now())}`);
      });

      form.addEventListener('submit', e => {
        e.preventDefault();
        if ($('#m').val() == '') return;
        socket.emit('message', $('#m').val());

        $('#messages').append($('<li>')).text(`${username} » ${$('#m').val()} - - - - ${new Date(Date.now())}`);

        $('#m').val('');
        return;
      });
    })

    function getfield(txt) {
      return prompt(txt);
    }
</script>