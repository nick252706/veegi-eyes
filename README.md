# veegi-eyes
video call
// server.js
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server);

io.on('connection', (socket) => {
  console.log('New client connected');

  socket.on('signal', (data) => {
    io.to(data.to).emit('signal', data);
  });

  socket.on('join', (room) => {
    socket.join(room);
    socket.to(room).broadcast.emit('user-joined', socket.id);
  });

  socket.on('disconnect', () => {
    console.log('Client disconnected');
  });
});

server.listen(4000, () => {
  console.log('Server is listening on port 4000');
});
