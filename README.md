# -Chat-Application
 Chat Application
// Require the necessary modules
const express = require('express');
const app = express();
const http = require('http');
const server = http.createServer(app);
const io = require('socket.io')(server);

// Serve the web app
app.use(express.static('public'));

// Listen for connections on the server
io.on('connection', (socket) => {
  console.log('User connected');

  // Create a new chat room
  socket.on('createRoom', (roomName) => {
    socket.join(roomName);
    console.log('Room created: ' + roomName);
  });

  // Join an existing chat room
  socket.on('joinRoom', (roomName) => {
    socket.join(roomName);
    console.log('User joined room: ' + roomName);
  });

  // Send a message to the chat room
  socket.on('sendMessage', (message, roomName) => {
    io.to(roomName).emit('message', message);
    console.log('Message sent: ' + message);
  });

  // Send a multimedia message to the chat room
  socket.on('sendMultimedia', (data, roomName) => {
    io.to(roomName).emit('multimedia', data);
    console.log('Multimedia sent: ' + data);
  });

  // Disconnect event
  socket.on('disconnect', () => {
    console.log('User disconnected');
  });
});

// Start the server
server.listen(3000, () => {
  console.log('Server started on port 3000');
});
