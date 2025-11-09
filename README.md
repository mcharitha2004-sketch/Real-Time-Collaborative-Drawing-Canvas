# Real-Time-Collaborative-Drawing-Canvas

### 1. Install dependencies
npm install
## 2. Start the server
npm start
## 3. Open in browser
Go to http://localhost:3000

## How It Works
Each client captures pointer movements and sends small stroke segments to the server.
The server stores these strokes in an operation log (opLog) and broadcasts them to other users.
The canvas is updated in real time for all connected clients.
A new user joining the room receives a snapshot image of the current canvas from the server.
Undo/redo commands mark operations as active/inactive (tombstone=true) for global rollback.

## Known Limitations
Currently uses an in-memory op log, not persisted to disk.
Undo/redo control ops are implemented on the server side but not yet wired to UI buttons.
Snapshot generation uses node-canvas, which may require native dependencies on some systems.
Performance may degrade with very high stroke frequency (no CRDT yet).

## Time Spent
~6 hours total
Canvas + tool logic: 2h
WebSocket client/server: 2h
Architecture + sync testing: 1.5h
Documentation & polish: 0.5h

