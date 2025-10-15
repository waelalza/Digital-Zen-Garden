Digital Zen Garden
A web-based, collaborative virtual space where multiple users can create calming patterns, sounds, and visuals in real-time to promote stress relief and connection.
Overview
Digital Zen Garden allows users to collaboratively draw patterns (e.g., raked sand), place objects (e.g., rocks, plants), and adjust ambient sounds (e.g., chimes, water) in a soothing digital environment. Built with WebGL (Three.js), WebSocket (Socket.io), and Web Audio API, it supports 2-4 concurrent users with real-time syncing.
Prerequisites

Node.js (v14 or higher) for local development or server setup.
Modern web browser (Chrome, Firefox) with WebGL support.
Accounts for hosting services (e.g., Vercel, Netlify, Heroku, or Firebase) for online deployment.
Internet connection for CDN-hosted libraries (Three.js, Socket.io) and deployment.

Setup Instructions (Local)

Clone or Download the Project

Clone the repository or download the project files (index.html, app.js, styles.css, etc.).


Install Dependencies

No manual installation is required for Three.js or Socket.io, as they are loaded via CDN in index.html.
For WebSocket server: Install Socket.io server-side if needed (see Deployment for details).


Run Locally

Install a simple HTTP server: npm install -g http-server
Navigate to the project directory: cd path/to/project
Start the server: http-server -p 8080
Open http://localhost:8080 in your browser.
Note: For full WebSocket functionality, a Node.js server is recommended (see below).



Deployment Instructions (Fully Operational Online)
To make the app fully operational and accessible online (not just local), deploy it to a hosting platform. Since the app requires real-time collaboration via WebSockets, use a platform that supports Node.js for the server-side Socket.io logic. Here's how to deploy to popular services:
Option 1: Vercel (Recommended for Simplicity)
Vercel supports static sites with serverless functions, but for Socket.io, you'll need a Node.js server.

Prepare the Project

Create a server.js file for Socket.io backend (example below).
Update app.js to connect to the deployed WebSocket URL (e.g., wss://your-app.vercel.app).

Example server.js:
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = new Server(server, { cors: { origin: '*' } });

app.use(express.static('public')); // Serve static files from 'public' folder (move your HTML/JS/CSS there)

io.on('connection', (socket) => {
  console.log('User connected');
  // Handle events: e.g., socket.on('draw', (data) => socket.broadcast.emit('draw', data));
  // Add your collaboration logic here
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => console.log(`Server running on port ${PORT}`));


Deploy to Vercel

Sign up at vercel.com.
Install Vercel CLI: npm install -g vercel
Run vercel in your project directory and follow prompts.
Select "Other" framework, configure as Node.js server.
Access your app at the provided URL (e.g., https://your-app.vercel.app).

Note: Vercel has limitations on WebSocket duration; for long sessions, consider Heroku or a VPS.


Option 2: Heroku

Prepare the Project

Add server.js as above.
Create a Procfile with: web: node server.js
Add dependencies to package.json:{
  "name": "digital-zen-garden",
  "version": "1.0.0",
  "scripts": { "start": "node server.js" },
  "dependencies": { "express": "^4.17.1", "socket.io": "^4.4.1" }
}




Deploy to Heroku

Sign up at heroku.com.
Install Heroku CLI: npm install -g heroku
Run heroku create, then git push heroku main.
Open the app: heroku open.
Heroku supports WebSockets natively.



Option 3: Firebase (For Real-Time Database Integration)

Setup Firebase

Create a project at console.firebase.google.com.
Install Firebase CLI: npm install -g firebase-tools
Login: firebase login
Initialize: firebase init (select Hosting and Functions if needed).


Integrate with App

Use Firebase Realtime Database for session data instead of in-memory.
Update app.js to use Firebase SDK (add via CDN).
For WebSockets, combine with Socket.io or use Firebase's real-time features.


Deploy

Run firebase deploy.
Access at your project's hosting URL.

Firebase is great for real-time syncing without custom WebSocket code.


Option 4: Netlify (For Static Sites, Limited WebSockets)

Suitable if you adapt to Netlify Functions for serverless WebSockets (experimental).
Upload static files and deploy via netlify.com.
For full WebSockets, prefer Vercel or Heroku.

Usage

Once deployed, share the URL with users.
Users join sessions via the online app; changes sync in real-time.
Test collaboration by opening multiple browser tabs/windows.

Project Structure

index.html: Main app structure with canvas and UI.
app.js: Core logic for Three.js, Socket.io, and Web Audio API.
styles.css: Minimalist, calming styles for the UI.
server.js: (Added for deployment) Node.js server for Socket.io.
Other JS modules (e.g., visuals.js, audio.js): Modular logic for visuals and audio.

Troubleshooting

WebSocket Connection Errors: Ensure the server is deployed and the client connects to the correct wss:// URL.
Deployment Issues: Check platform logs (e.g., vercel logs, heroku logs).
Scalability: For more users, upgrade to paid plans or use a dedicated server.
CORS Errors: Configure CORS in server.js as shown.

Notes

The app uses CDNs for Three.js and Socket.io to simplify setup.
Customize server.js with your specific event handlers for drawing, audio, etc.
For production, add security (e.g., authentication, rate limiting).

Contributing
Feel free to fork the repository, add features, or report issues via pull requests or issues on the project’s repository (if applicable).
License
MIT License. See LICENSE file for details (if included).
