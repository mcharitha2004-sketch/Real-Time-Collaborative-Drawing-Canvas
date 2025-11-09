## Collaborative Canvas – Architecture
## Overview

Real-time multi-user drawing app using Vanilla JS, HTML5 Canvas, and Node.js + WebSockets (Socket.io).

Clients handle drawing and rendering; the server manages synchronization and broadcasts updates.

A global operation log (op log) ensures all users see the same canvas state.

## Data Flow

User draws → client captures stroke data.

Client sends stroke info via WebSocket to server.

Server logs the stroke and broadcasts it to all clients.

Other clients render the stroke instantly.

New users get a canvas snapshot (rebuilt from op log).

## WebSocket Protocol

strokeStart → begin new stroke

stroke → continuous drawing updates

strokeBatch → batch of strokes to reduce traffic

strokeEnd → finish a stroke

cursor → live cursor position updates

initState → send full canvas state to new users

userJoin / userLeave → notify others about user activity

Each message carries user ID, color, size, and mode (brush/eraser).

## Undo/Redo Strategy

All strokes stored in a global operation log with unique IDs.

Undo = mark operation as inactive (tombstone = true).

Redo = reactivate operation (tombstone = false).

Canvas rebuilt by replaying only active operations.

Keeps global consistency — all users see the same result.

## Conflict Resolution

Server assigns order to operations (by ID/timestamp).

Later operations overwrite earlier ones deterministically.

Eraser treated as a special stroke using destination-out mode.

Ensures all clients converge to identical canvas state.

## Performance Decisions

Batching: Groups pointer events to reduce network load.

Client-side prediction: Immediate local rendering for low latency.

Snapshots: Fast sync for new/reconnected users.

Room isolation: Separate state per room for scalability.

Selective replay: Redraws only active operations for undo/redo
