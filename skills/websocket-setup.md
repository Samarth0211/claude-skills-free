---
name: websocket-setup
description: Implement WebSocket server with rooms
category: backend
tags: ["backend", "websocket", "real-time"]
difficulty: intermediate
version: 1.0.0
author: Claude Skills Hub
---

# WebSocket Setup

> Implement WebSocket server with rooms

You are a Node.js backend engineer. The user wants to implement a WebSocket server with room-based communication using Socket.IO.

## What to check first
- Verify Node.js is installed: `node --version` (v14+)
- Check if Socket.IO is in package.json: `npm list socket.io`
- Confirm an HTTP server package exists: `npm list express` or `npm list http`

## Steps
1. Install Socket.IO: `npm install socket.io` (also need `npm install express` for the HTTP server)
2. Create an Express server and attach Socket.IO to it using `http.createServer(app)`
3. Initialize Socket.IO with the server instance: `const io = require('socket.io')(httpServer)`
4. Listen for the `connection` event on the io instance to handle new socket connections
5. Inside the connection handler, use `socket.on('join', callback)` to handle room join requests with the room name parameter
6. Use `socket.join(roomName)` to add the socket to a room
7. Broadcast messages to a specific room using `io.to(roomName).emit(eventName, data)`
8. Handle disconnection with `socket.on('disconnect')` and optionally use `socket.leave(roomName)` before cleanup

## Code
```javascript
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIO(server, {
  cors: {
    origin: '*',
    methods: ['GET', 'POST']
  }
});

const PORT = process.env.PORT || 3000;

// Track room members
const rooms = {};

io.on('connection', (socket) => {
  console.log(`User connected: ${socket.id}`);

  // Handle joining a room
  socket.on('join', (roomName, userName) => {
    socket.join(roomName);
    socket.currentRoom = roomName;
    socket.userName = userName;

    if (!rooms[roomName]) {
      rooms[roomName] = [];
    }
    rooms[roomName].push({ id: socket.id, name: userName });

    // Notify others in the room
    io.to(roomName).emit('user-joined', {
      userName: userName,
      userId: socket.id,
      totalUsers: rooms[roomName].length
    });

    // Send room state to the joining user
    socket.emit('room-state', {
      members: rooms[roomName],
      roomName: roomName
    });
  });

  // Handle messages in a room
  socket.on('message', (msg) => {
    if (socket.currentRoom) {
      io.to(socket.currentRoom).emit('new-message', {
        userId: socket.id,
        userName: socket.userName,
        text: msg,
        timestamp: new Date
```

*Note: this example was truncated in the source. See [the GitHub repo](https://github.com/Samarth0211/claude-skills-hub) for the latest full version.*

## Common Pitfalls

- Treating this skill as a one-shot solution — most workflows need iteration and verification
- Skipping the verification steps — you don't know it worked until you measure
- Applying this skill without understanding the underlying problem — read the related docs first


## When NOT to Use This Skill

- When a simpler manual approach would take less than 10 minutes
- On critical production systems without testing in staging first
- When you don't have permission or authorization to make these changes


## How to Verify It Worked

- Run the verification steps documented above
- Compare the output against your expected baseline
- Check logs for any warnings or errors — silent failures are the worst kind


## Production Considerations

- Test in staging before deploying to production
- Have a rollback plan — every change should be reversible
- Monitor the affected systems for at least 24 hours after the change



---
*From [CLSkills.in](https://clskills.in/browse) — 2,300+ free Claude Code skills*

