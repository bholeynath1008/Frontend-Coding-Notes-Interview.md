# Notification Systems: WebSocket, SSE, Polling, and Cron Jobs

This guide explains four common approaches for implementing notifications in a Node.js + React project. Each section covers the technology, implementation steps (backend, frontend, database), workflow diagram, and communication flow.

---

## 1. WebSocket (Two‑Way Real‑Time)

### How It Works
WebSocket maintains a persistent, full‑duplex connection between client and server. Ideal for chat, collaborative tools, live dashboards.

### Implementation Steps

#### Backend (Node.js)
- Use `ws` or `socket.io` library.
- Attach WebSocket server to HTTP server.
- Authenticate connections using JWT (query param or headers).
- Store connected clients in memory (or Redis for multi‑server).
- Implement message handlers: `ping`, `subscribe`, `ack`.
- Use Redis PubSub to broadcast notifications across server instances.
- Add a Bull/BullMQ queue for reliable delivery and offline storage.

#### Frontend (React)
- Create a custom hook that manages WebSocket connection.
- On mount, connect with stored token.
- Send periodic pings, handle reconnection with exponential backoff.
- Subscribe to user‑specific channels (e.g., `user:${userId}`).
- Display notifications and send acknowledgments.

#### Database
- Store notifications with status (`pending`, `delivered`, `read`).
- Keep offline notifications for users who are disconnected.

### Workflow Diagram
```
Client                     Server                     Redis                     Database
  |                          |                          |                          |
  |--- WS connect + token -->|                          |                          |
  |                          |--- verify token -------->|                          |
  |<-- connected confirm ----|                          |                          |
  |                          |                          |                          |
  |--- subscribe: user:123 -->|                          |                          |
  |                          |--- store subscription ---|                          |
  |                          |                          |                          |
  |                          |                          |                          |
  [ Notification triggered by another service ]        |                          |
  |                          |<-- REST POST ------------|                          |
  |                          |--- queue job ----------->|                          |
  |                          |                          |                          |
  |                          |--- publish to Redis ----->|                          |
  |                          |                          |--- broadcast to server ->|
  |<-- notification ---------|                          |                          |
  |--- ack ---------------->|                          |                          |
  |                          |--- mark delivered ------>|                          |
```

### Communication Steps
1. Client connects with JWT → server authenticates.
2. Client subscribes to channels (e.g., `user:123`).
3. Notification arrives via REST API → server queues it.
4. Queue worker processes: if client online, send via WebSocket; else store in DB.
5. Client receives, sends ACK → server marks delivered.

---

## 2. Server‑Sent Events (SSE) – One‑Way Real‑Time

### How It Works
SSE allows the server to push events to the client over a single HTTP connection. The client receives text/event‑stream data and automatically reconnects if the connection drops.

### Implementation Steps

#### Backend (Node.js)
- Use Express endpoint that sets headers:  
  `Content-Type: text/event-stream`, `Cache-Control: no-cache`, `Connection: keep-alive`.
- Write data with `res.write('data: ${JSON.stringify(event)}\n\n')`.
- Maintain a map of active SSE clients (by userId) to send targeted events.
- For multi‑server scaling, use Redis PubSub to forward events to the correct server instance.
- No need for a message queue if you only need at‑most‑once delivery; but for reliability, combine with a queue.

#### Frontend (React)
- Use the native `EventSource` API.
- Construct URL with authentication token (e.g., `/events?token=...`).
- Listen to `message` events and update UI.
- Handle reconnection automatically (EventSource does it).
- Send heartbeat if needed (but SSE has built‑in reconnect).

#### Database
- Similar to WebSocket: store notifications with statuses for offline handling.

### Workflow Diagram
```
Client                      Server                       Redis
  |                           |                            |
  |--- GET /events?token= --->|                            |
  |                           |--- verify token -----------|
  |<-- 200 OK, text/event-stream |                         |
  |                           |                            |
  |                           |-- store client in memory --|
  |                           |                            |
  |                           |                            |
  [ Notification triggered ]   |                            |
  |                           |<-- POST /notify -----------|
  |                           |--- publish to Redis ------>|
  |                           |                            |
  |                           |<-- receive from Redis -----|
  |<-- data: { ... } ---------|                            |
  |                           |                            |
  (EventSource auto‑reconnects if connection drops)
```

### Communication Steps
1. Client opens SSE connection with authentication.
2. Server keeps the connection open and stores a reference.
3. When a notification arrives, server pushes data to the relevant open connection(s).
4. If no client is connected, the server may store the event in a database for later delivery.

---

## 3. Polling – Simple Request‑Response

### How It Works
The client periodically asks the server for new notifications. Two variants: **short polling** (regular interval) and **long polling** (server holds request until data available).

### Implementation Steps

#### Backend (Node.js)
- Create REST endpoint `GET /api/notifications?since=<timestamp>`.
- Return list of new notifications (optionally with `Last-Event-ID`).
- For long polling, set a timeout (e.g., 30s) and wait for new notifications to arrive (using an event emitter or Redis PubSub).
- Use a queue to store pending notifications for users.

#### Frontend (React)
- Use `setInterval` for short polling.
- Or use a recursive `fetch` with a timeout for long polling.
- Keep track of the last received timestamp.
- Display notifications as they arrive.

#### Database
- Store notifications with a timestamp; query for those newer than client's last poll.

### Workflow Diagram (Short Polling)
```
Client                   Server                   Database
  |                        |                         |
  |--- GET /notifications?since=... -->|                         |
  |                        |--- SELECT ... -------->|
  |<-- [new notifications] -|                         |
  |                        |                         |
  |-- wait N seconds ------|                         |
  |--- GET /notifications?since=... -->|                         |
  |                        |--- SELECT ... -------->|
  |<-- [new notifications] -|                         |
```

### Communication Steps
1. Client sends request with last known timestamp.
2. Server queries DB for newer notifications and returns them.
3. Client updates UI and repeats after delay.
4. (Long polling) Server holds request until new data arrives or timeout.

---

## 4. Cron Jobs – Scheduled Batch Notifications

### How It Works
Cron jobs run scheduled tasks on the server, useful for reminders, daily summaries, or periodic cleanup. They are not interactive but can trigger notifications via other channels (WebSocket, SSE, email).

### Implementation Steps

#### Backend (Node.js)
- Use `node-cron` to define scheduled jobs.
- Or use Bull's `repeatable jobs` for more robust scheduling.
- The job queries the database for users who need notifications (e.g., pending tasks, birthdays).
- For each user, it pushes a notification into a queue (Bull) that will deliver it (via WebSocket/SSE/push).
- Alternatively, the job can directly send emails or SMS.

#### Frontend (React)
- No direct involvement; the user may see the notification appear when delivered through the real‑time channel.

#### Database
- Store user preferences for scheduled notifications.
- Keep logs of sent scheduled notifications.

### Workflow Diagram
```
Scheduler (cron)                Job Processor              Queue / Notification Service
      |                               |                              |
      |-- triggers at 9:00 AM ------->|                              |
      |                               |--- fetch users needing alert->|
      |                               |<-- list of users -------------|
      |                               |                              |
      |                               |--- for each user, add to queue ->|
      |                               |                              |
      |                               |                              |--- deliver (WebSocket/SSE/email)
```

### Communication Steps
1. Cron job runs at scheduled time.
2. It queries for recipients based on business logic.
3. For each recipient, it enqueues a notification.
4. The queue worker delivers the notification using the appropriate channel (WebSocket, SSE, push notification, etc.).

---

## Enterprise Best Practices (Apply to All Approaches)

### Reliability
- **Acknowledgment**: Clients should send ACK for important notifications; server retries on missing ACK.
- **Message Queue**: Use Bull/BullMQ to ensure notifications are not lost, with retries and dead‑letter handling.
- **Offline Storage**: Store notifications in DB for users who are offline; deliver on next connection.

### Scalability
- **Horizontal Scaling**: Use Redis PubSub to sync WebSocket/SSE connections across servers.
- **Sticky Sessions**: For WebSocket, ensure load balancer preserves connection affinity.
- **Connection Limits**: Monitor and limit connections per user/device.

### Security
- **Authentication**: Always authenticate connections (JWT, API keys).
- **Authorization**: Validate subscription to channels (e.g., user can only subscribe to their own channel).
- **Rate Limiting**: Prevent abuse of notification endpoints.

### Performance
- **Compression**: Use message compression for large payloads.
- **Batching**: Combine multiple notifications when possible.
- **Connection Heartbeats**: Detect dead connections and clean up resources.

### Monitoring
- Track metrics: connected clients, queue depth, delivery latency, failure rates.
- Log every notification send and receipt for debugging.

---

## Comparison Table

| Feature           | WebSocket                       | SSE                             | Polling                         | Cron Jobs                       |
|-------------------|---------------------------------|---------------------------------|---------------------------------|---------------------------------|
| **Direction**     | Bi‑directional                  | Server → Client                 | Client → Server                 | Server → Server                 |
| **Real‑time**     | ✅ Low latency                  | ✅ Near‑instant                 | ❌ Delay depends on interval    | ❌ Scheduled                    |
| **Complexity**    | High (state management)         | Medium                          | Low                             | Low                             |
| **Server Load**   | High (persistent connections)   | Medium (persistent)             | High (frequent requests)        | Low (runs at intervals)         |
| **Auto‑reconnect**| Manual implementation           | Built‑in                        | Manual                          | N/A                             |
| **Use Cases**     | Chat, live collaboration        | News feed, stock ticker         | Simple apps, admin panels       | Reminders, reports, cleanup     |

---

Choose the right tool based on your use case:
- **WebSocket** when you need low‑latency two‑way communication.
- **SSE** for one‑way real‑time updates with simpler implementation.
- **Polling** for very simple scenarios or when real‑time isn’t critical.
- **Cron Jobs** for scheduled batch notifications.

# Notification Queue System - Complete Guide

## Understanding the Core Concepts

### 1. **WebSockets** (Two-way Real-time)
- Persistent connection between client and server
- Best for: Chat, real-time collaboration, live updates
- Pros: Low latency, bi-directional
- Cons: Server resource intensive

### 2. **Server-Sent Events (SSE)** (Server → Client)
- One-way stream from server to client
- Best for: News feeds, stock tickers, notifications
- Pros: Simpler than WebSockets, auto-reconnect
- Cons: Client can't send data through same connection

### 3. **Polling** (Client asks)
- Client repeatedly asks server for updates
- **Short Polling**: Ask every few seconds
- **Long Polling**: Hold connection until data available
- Best for: Simple apps with low-frequency updates

### 4. **Cron Jobs** (Scheduled)
- Server runs tasks at scheduled times
- Best for: Batch notifications, reminders, reports
- Pros: Reliable, predictable
- Cons: Not real-time

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT LAYER                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                      │
│  │   Browser    │  │  Mobile App  │  │  Desktop App │                      │
│  │  React App   │  │  React Native│  │    Electron  │                      │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘                      │
│         │                 │                 │                               │
│         └─────────────────┼─────────────────┘                               │
│                           │                                                 │
│                    WebSocket/WSS                                            │
│                    (wss://api.example.com)                                  │
└───────────────────────────┼─────────────────────────────────────────────────┘
                            │
┌───────────────────────────┼─────────────────────────────────────────────────┐
│                           ▼                                                 │
│                      LOAD BALANCER                                          │
│                    (Nginx / AWS ALB)                                        │
│                           │                                                 │
│         ┌─────────────────┼─────────────────┐                              │
│         ▼                 ▼                 ▼                              │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐                       │
│  │ Node Server │   │ Node Server │   │ Node Server │                       │
│  │   Instance  │   │   Instance  │   │   Instance  │                       │
│  │  ┌───────┐  │   │  ┌───────┐  │   │  ┌───────┐  │                       │
│  │  │WS     │  │   │  │WS     │  │   │  │WS     │  │                       │
│  │  │Server │  │   │  │Server │  │   │  │Server │  │                       │
│  │  └───────┘  │   │  └───────┘  │   │  └───────┘  │                       │
│  └──────┬──────┘   └──────┬──────┘   └──────┬──────┘                       │
│         │                 │                 │                               │
│         └─────────────────┼─────────────────┘                               │
│                           │                                                 │
│                    ┌──────▼──────┐                                         │
│                    │    Redis    │  ← PubSub for cross-instance messages   │
│                    │   PubSub    │                                         │
│                    └──────┬──────┘                                         │
│                           │                                                 │
│                    ┌──────▼──────┐                                         │
│                    │   Message   │  ← Persistent queue for reliability     │
│                    │    Queue    │    (Bull/BullMQ)                        │
│                    │   (Redis)   │                                         │
│                    └──────┬──────┘                                         │
│                           │                                                 │
│                    ┌──────▼──────┐                                         │
│                    │ PostgreSQL  │  ← Store notifications, users,          │
│                    │  / MongoDB  │    subscriptions                        │
│                    └─────────────┘                                         │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Implementation Steps

### Backend Setup

#### 1. **Project Structure**
```
backend/
├── src/
│   ├── websocket/
│   │   ├── server.ts          # WebSocket server setup
│   │   ├── auth.ts            # JWT authentication
│   │   ├── handlers.ts        # Message handlers
│   │   └── channels.ts        # Subscription management
│   ├── services/
│   │   ├── notification.service.ts
│   │   ├── redis-pubsub.service.ts
│   │   └── queue.service.ts   # BullMQ queue
│   ├── jobs/
│   │   ├── reminder.job.ts    # Cron jobs
│   │   └── cleanup.job.ts
│   ├── api/
│   │   └── notifications.routes.ts
│   └── models/
│       └── notification.model.ts
├── package.json
└── tsconfig.json
```

#### 2. **Package Dependencies**
```json
{
  "dependencies": {
    "express": "^4.18.2",
    "ws": "^8.14.2",
    "redis": "^4.6.10",
    "bull": "^4.11.5",
    "jsonwebtoken": "^9.0.2",
    "uuid": "^9.0.1",
    "mongoose": "^7.5.0"
  }
}
```

#### 3. **WebSocket Server with Authentication**
```typescript
// websocket/server.ts
import { WebSocketServer, WebSocket } from 'ws';
import { verifyToken } from './auth';

interface Client {
  ws: WebSocket;
  userId: string;
  deviceId: string;
  subscriptions: Set<string>;
}

const clients = new Map<string, Client>();

export const createWSServer = (server: any) => {
  const wss = new WebSocketServer({ server, path: '/ws' });

  wss.on('connection', (ws, req) => {
    const token = new URL(req.url!, `http://${req.headers.host}`)
      .searchParams.get('token');
    
    const user = verifyToken(token);
    if (!user) {
      ws.close(4001, 'Unauthorized');
      return;
    }

    const clientId = `${user.userId}:${user.deviceId}`;
    clients.set(clientId, {
      ws,
      userId: user.userId,
      deviceId: user.deviceId,
      subscriptions: new Set()
    });

    ws.on('message', (data) => handleMessage(clientId, data));
    ws.on('close', () => clients.delete(clientId));
    
    // Send offline notifications
    sendOfflineNotifications(clientId);
  });

  return wss;
};
```

#### 4. **Redis PubSub for Multi-Server Scaling**
```typescript
// services/redis-pubsub.service.ts
import Redis from 'ioredis';

const redis = new Redis(process.env.REDIS_URL);

export const publishNotification = async (event: {
  type: 'user' | 'channel';
  target: string;
  data: any;
}) => {
  await redis.publish('notifications', JSON.stringify(event));
};

// Subscribe to notifications from other servers
redis.subscribe('notifications', (err) => {
  if (!err) {
    redis.on('message', (channel, message) => {
      const event = JSON.parse(message);
      // Forward to local clients
      forwardToLocalClients(event);
    });
  }
});
```

#### 5. **BullMQ Queue for Reliable Delivery**
```typescript
// services/queue.service.ts
import { Queue, Worker } from 'bullmq';
import Redis from 'ioredis';

const connection = new Redis(process.env.REDIS_URL);

export const notificationQueue = new Queue('notifications', { connection });

// Worker processes queue jobs
new Worker('notifications', async (job) => {
  const { userId, notification } = job.data;
  
  // Try to send via WebSocket
  const client = findClient(userId);
  if (client) {
    client.send(JSON.stringify(notification));
    await markAsDelivered(notification.id);
  } else {
    // Store for offline delivery
    await storeOfflineNotification(userId, notification);
  }
}, { connection });

// Add to queue
export const queueNotification = async (userId: string, notification: any) => {
  await notificationQueue.add('send', { userId, notification }, {
    attempts: 3,
    backoff: { type: 'exponential', delay: 5000 }
  });
};
```

#### 6. **Cron Jobs for Scheduled Notifications**
```typescript
// jobs/reminder.job.ts
import cron from 'node-cron';
import { queueNotification } from '../services/queue.service';

// Daily reminder at 9 AM
cron.schedule('0 9 * * *', async () => {
  const users = await getUsersWithReminderEnabled();
  
  for (const user of users) {
    await queueNotification(user.id, {
      type: 'reminder',
      title: 'Daily Check-in',
      body: 'Time to review your tasks!'
    });
  }
});

// Weekly report on Monday
cron.schedule('0 10 * * 1', async () => {
  // Generate and send weekly reports
});
```

---

### Database Schema

```typescript
// models/notification.model.ts
import mongoose from 'mongoose';

const notificationSchema = new mongoose.Schema({
  userId: { type: String, required: true, index: true },
  type: { type: String, enum: ['info', 'warning', 'alert'] },
  title: String,
  body: String,
  data: mongoose.Schema.Types.Mixed,
  status: { 
    type: String, 
    enum: ['pending', 'delivered', 'read', 'failed'],
    default: 'pending'
  },
  deliveredAt: Date,
  readAt: Date,
  createdAt: { type: Date, default: Date.now }
});

// Offline notifications
const offlineNotificationSchema = new mongoose.Schema({
  userId: String,
  notificationId: String,
  expiresAt: Date
});

export const Notification = mongoose.model('Notification', notificationSchema);
```

---

### Frontend Implementation (React)

```typescript
// hooks/useNotifications.ts
import { useState, useEffect, useCallback } from 'react';

interface Notification {
  id: string;
  title: string;
  body: string;
  data?: any;
  timestamp: string;
}

class NotificationService {
  private ws: WebSocket | null = null;
  private listeners: Map<string, Function[]> = new Map();
  private reconnectTimer: NodeJS.Timeout | null = null;
  private pingInterval: NodeJS.Timeout | null = null;

  constructor(private baseUrl: string) {}

  connect(token: string) {
    const wsUrl = `${this.baseUrl}/ws?token=${token}`;
    this.ws = new WebSocket(wsUrl);

    this.ws.onopen = () => {
      console.log('Connected');
      this.startHeartbeat();
      this.emit('connected', null);
    };

    this.ws.onmessage = (event) => {
      const message = JSON.parse(event.data);
      this.handleMessage(message);
    };

    this.ws.onclose = () => {
      this.stopHeartbeat();
      this.scheduleReconnect(token);
    };
  }

  private handleMessage(message: any) {
    switch (message.type) {
      case 'notification':
        this.emit('notification', message.payload);
        // Send acknowledgment
        this.send({ type: 'ack', id: message.id });
        break;
      case 'pong':
        // Heartbeat response
        break;
    }
  }

  private startHeartbeat() {
    this.pingInterval = setInterval(() => {
      this.send({ type: 'ping' });
    }, 30000);
  }

  private scheduleReconnect(token: string) {
    if (this.reconnectTimer) return;
    this.reconnectTimer = setTimeout(() => {
      this.connect(token);
      this.reconnectTimer = null;
    }, 3000);
  }

  send(message: any) {
    if (this.ws?.readyState === WebSocket.OPEN) {
      this.ws.send(JSON.stringify(message));
    }
  }

  on(event: string, callback: Function) {
    if (!this.listeners.has(event)) {
      this.listeners.set(event, []);
    }
    this.listeners.get(event)!.push(callback);
  }

  private emit(event: string, data: any) {
    this.listeners.get(event)?.forEach(cb => cb(data));
  }

  disconnect() {
    this.stopHeartbeat();
    this.ws?.close();
    this.ws = null;
  }

  private stopHeartbeat() {
    if (this.pingInterval) {
      clearInterval(this.pingInterval);
      this.pingInterval = null;
    }
  }
}

// React Hook
export const useNotifications = () => {
  const [notifications, setNotifications] = useState<Notification[]>([]);
  const [isConnected, setIsConnected] = useState(false);
  const [service] = useState(() => new NotificationService(process.env.REACT_APP_WS_URL!));

  useEffect(() => {
    const token = localStorage.getItem('token');
    if (token) {
      service.connect(token);
    }

    service.on('connected', () => setIsConnected(true));
    service.on('notification', (notification: Notification) => {
      setNotifications(prev => [notification, ...prev]);
      
      // Show browser notification
      if (Notification.permission === 'granted') {
        new Notification(notification.title, {
          body: notification.body,
          icon: '/icon.png'
        });
      }
    });

    return () => {
      service.disconnect();
    };
  }, []);

  const markAsRead = useCallback(async (id: string) => {
    await fetch(`/api/notifications/${id}/read`, { method: 'POST' });
    setNotifications(prev => 
      prev.map(n => n.id === id ? { ...n, read: true } : n)
    );
  }, []);

  return {
    notifications,
    isConnected,
    markAsRead,
    send: service.send.bind(service)
  };
};

// React Component
const NotificationBell: React.FC = () => {
  const { notifications, isConnected, markAsRead } = useNotifications();
  const [isOpen, setIsOpen] = useState(false);
  const unreadCount = notifications.filter(n => !n.read).length;

  return (
    <div className="relative">
      <button 
        onClick={() => setIsOpen(!isOpen)}
        className="relative p-2"
      >
        🔔
        {unreadCount > 0 && (
          <span className="absolute top-0 right-0 bg-red-500 text-white text-xs rounded-full w-4 h-4">
            {unreadCount}
          </span>
        )}
        {!isConnected && (
          <span className="absolute bottom-0 right-0 w-2 h-2 bg-gray-400 rounded-full" />
        )}
      </button>

      {isOpen && (
        <div className="absolute right-0 mt-2 w-80 bg-white rounded-lg shadow-lg z-50">
          <div className="p-3 border-b">
            <h3 className="font-semibold">Notifications</h3>
          </div>
          <div className="max-h-96 overflow-y-auto">
            {notifications.length === 0 ? (
              <p className="p-4 text-gray-500 text-center">No notifications</p>
            ) : (
              notifications.map(notification => (
                <div 
                  key={notification.id}
                  className={`p-3 border-b hover:bg-gray-50 cursor-pointer ${!notification.read ? 'bg-blue-50' : ''}`}
                  onClick={() => markAsRead(notification.id)}
                >
                  <div className="font-medium">{notification.title}</div>
                  <div className="text-sm text-gray-600">{notification.body}</div>
                  <div className="text-xs text-gray-400 mt-1">
                    {new Date(notification.timestamp).toLocaleTimeString()}
                  </div>
                </div>
              ))
            )}
          </div>
        </div>
      )}
    </div>
  );
};
```

---

## Communication Flow

### 1. **User Connects**
```
Client                    Server
   |                         |
   |--- WS Connect + JWT --->|
   |                         |── Verify JWT
   |                         |── Load offline notifications
   |<-- Connected Confirm ---|
   |<-- Offline Notifications|
   |                         |
   |--- Ping (every 30s) --->|
   |<-- Pong ---------------|
```

### 2. **Send Notification**
```
Service                    Server                    Redis                  Client
   |                          |                        |                       |
   |--- POST /api/notify --->|                        |                       |
   |                          |── Add to Queue ──────>|                       |
   |<-- { queued: true } ----|                        |                       |
   |                          |                        |                       |
   |                          |<── Process Job ───────|                       |
   |                          |── Publish to PubSub ─>|                       |
   |                          |                        |── Broadcast ────────>|
   |                          |                        |<── ACK ──────────────|
   |                          |── Mark as delivered ──|                       |
```

### 3. **Multi-Server Scenario**
```
Server A                 Redis                  Server B               Client B
   |                       |                        |                      |
   |── Publish Event ─────>|                        |                      |
   |                       |── Forward Event ──────>|                      |
   |                       |                        |── Send to Client ──>|
   |                       |                        |<── ACK ─────────────|
```

---

## Enterprise Best Practices

### 1. **Reliability**
- ✅ Use message queues (Bull/BullMQ) for guaranteed delivery
- ✅ Implement acknowledgment system
- ✅ Store offline notifications in database
- ✅ Add retry logic with exponential backoff
- ✅ Monitor failed deliveries

### 2. **Scalability**
- ✅ Use Redis PubSub for horizontal scaling
- ✅ Implement connection pooling
- ✅ Use sticky sessions for WebSocket affinity
- ✅ Separate notification service from main API
- ✅ Auto-scale based on connection count

### 3. **Security**
- ✅ Authenticate all WebSocket connections
- ✅ Use short-lived JWT tokens
- ✅ Validate channel subscriptions
- ✅ Rate limit notifications per user
- ✅ Encrypt sensitive notification data

### 4. **Performance**
- ✅ Compress WebSocket messages
- ✅ Batch notifications when possible
- ✅ Use connection heartbeats
- ✅ Implement connection throttling
- ✅ Cache active connections in Redis

### 5. **Monitoring**
- ✅ Track connection counts per server
- ✅ Monitor queue depth and latency
- ✅ Alert on failed deliveries
- ✅ Log all notification events
- ✅ Dashboard for real-time metrics

### 6. **Offline Handling**
- ✅ Store notifications in database
- ✅ Set expiration for old notifications
- ✅ Deliver on reconnect
- ✅ Push notifications via FCM/APNS for mobile
- ✅ Email fallback for critical notifications

### 7. **Testing Strategy**
```typescript
// Integration test example
describe('Notification System', () => {
  it('should deliver notification to connected client', async () => {
    const client = await connectWebSocket(userToken);
    await sendNotification(userId, { title: 'Test' });
    
    const message = await client.waitForMessage(5000);
    expect(message.type).toBe('notification');
    expect(message.payload.title).toBe('Test');
  });
  
  it('should queue notification for offline user', async () => {
    await sendNotification(offlineUserId, { title: 'Test' });
    
    const pending = await getPendingNotifications(offlineUserId);
    expect(pending).toHaveLength(1);
    
    const client = await connectWebSocket(offlineUserToken);
    const message = await client.waitForMessage(5000);
    expect(message.payload.title).toBe('Test');
  });
});
```

---

## Quick Comparison Table

| Feature | WebSocket | SSE | Polling | Cron Jobs |
|---------|-----------|-----|---------|-----------|
| **Direction** | Bi-directional | Server → Client | Client → Server | Server → Server |
| **Real-time** | ✅ Instant | ✅ Near-instant | ❌ Delayed | ❌ Scheduled |
| **Complexity** | High | Medium | Low | Low |
| **Server Load** | High (persistent) | Medium | High (frequent) | Low |
| **Auto-reconnect** | Manual | Built-in | Manual | N/A |
| **Best For** | Chat, Games | News, Feeds | Simple apps | Reports, Reminders |

---

## When to Use Each

- **WebSockets**: Real-time chat, collaborative editing, live dashboards, gaming
- **SSE**: Live news feeds, stock tickers, progress updates, one-way notifications
- **Polling**: Simple apps with low-frequency updates, admin panels
- **Cron Jobs**: Daily reminders, weekly reports, cleanup tasks, batch processing

**Hybrid Approach**: Use WebSockets for real-time + Cron jobs for scheduled + Queue for reliability
