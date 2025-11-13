# Web Engineering - COMPLETE Notes (Final Continuation)

---

## UNIT V: PROMOTING WEB APPLICATIONS (Continued)

### 5. Managing Quality (Continued from Testing Strategy)

**Testing Strategy** (Continued):

**Test Coverage Goals**:
```
Unit Tests:
- Target: 80%+ coverage
- Focus: Business logic
- Fast execution (<5 seconds for all tests)

Integration Tests:
- Target: Key user flows
- API endpoints
- Database interactions
- Third-party integrations

E2E Tests:
- Target: Critical paths
- Happy path scenarios
- Major user journeys
- Payment flows

Example Coverage Report:
File               | % Stmts | % Branch | % Funcs | % Lines
-------------------|---------|----------|---------|--------
auth.js            |   95.2  |   88.9   |  100    |  94.7
cart.js            |   87.5  |   75.0   |  90.0   |  86.4
payment.js         |   92.3  |   85.7   |  95.5   |  91.8
Total              |   91.7  |   83.2   |  95.2   |  91.0
```

**Performance Benchmarks**:
```javascript
// Define performance budgets
const performanceBudgets = {
  // Time to Interactive
  TTI: 3500, // 3.5 seconds
  
  // First Contentful Paint
  FCP: 1500, // 1.5 seconds
  
  // Largest Contentful Paint
  LCP: 2500, // 2.5 seconds
  
  // Total Blocking Time
  TBT: 300,  // 300ms
  
  // Cumulative Layout Shift
  CLS: 0.1,  // 0.1 or less
  
  // Bundle sizes
  javascriptSize: 200 * 1024, // 200KB
  cssSize: 50 * 1024,         // 50KB
  imageSize: 500 * 1024       // 500KB per image
};

// Automated performance testing
const lighthouse = require('lighthouse');
const chromeLauncher = require('chrome-launcher');

async function runPerformanceTest(url) {
  const chrome = await chromeLauncher.launch({chromeFlags: ['--headless']});
  const options = {
    logLevel: 'info',
    output: 'json',
    onlyCategories: ['performance'],
    port: chrome.port
  };
  
  const runnerResult = await lighthouse(url, options);
  const performanceScore = runnerResult.lhr.categories.performance.score * 100;
  
  console.log(`Performance Score: ${performanceScore}`);
  
  // Check metrics against budgets
  const metrics = runnerResult.lhr.audits;
  
  if (metrics['interactive'].numericValue > performanceBudgets.TTI) {
    throw new Error(`TTI too slow: ${metrics['interactive'].numericValue}ms`);
  }
  
  if (metrics['first-contentful-paint'].numericValue > performanceBudgets.FCP) {
    throw new Error(`FCP too slow: ${metrics['first-contentful-paint'].numericValue}ms`);
  }
  
  await chrome.kill();
}
```

**Accessibility Standards**:
```
WCAG 2.1 Levels:
- Level A: Basic accessibility
- Level AA: Industry standard (aim for this)
- Level AAA: Enhanced accessibility

Key Requirements:

1. Perceivable:
   - Alt text for images
   - Captions for videos
   - Color contrast (4.5:1 for normal text, 3:1 for large)
   - Resizable text

2. Operable:
   - Keyboard accessible
   - No keyboard traps
   - Sufficient time to read
   - No seizure-inducing content

3. Understandable:
   - Readable content
   - Predictable behavior
   - Input assistance
   - Error identification

4. Robust:
   - Valid HTML
   - Compatible with assistive technologies
   - Future-proof
```

**Accessibility Testing Tools**:
```javascript
// Automated accessibility testing with axe-core
const { AxePuppeteer } = require('@axe-core/puppeteer');
const puppeteer = require('puppeteer');

async function testAccessibility(url) {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto(url);
  
  const results = await new AxePuppeteer(page).analyze();
  
  if (results.violations.length > 0) {
    console.log('Accessibility Violations Found:');
    results.violations.forEach(violation => {
      console.log(`\n${violation.id}: ${violation.description}`);
      console.log(`Impact: ${violation.impact}`);
      console.log(`Help: ${violation.helpUrl}`);
      console.log(`Affected elements: ${violation.nodes.length}`);
    });
  } else {
    console.log('No accessibility violations found!');
  }
  
  await browser.close();
}

// Manual accessibility testing checklist
const a11yChecklist = `
□ All images have alt text
□ Form inputs have labels
□ Color contrast meets WCAG AA (4.5:1)
□ Can navigate entire site with keyboard
□ Focus indicators visible
□ Headings in logical order (h1 → h2 → h3)
□ ARIA labels where needed
□ Error messages are clear and helpful
□ No auto-playing audio/video
□ Links have descriptive text (not "click here")
□ Tables have proper headers
□ Responsive design works at 200% zoom
□ Screen reader tested (NVDA, JAWS, VoiceOver)
`;
```

**Security Quality Gates**:
```javascript
// Security scanning in CI/CD pipeline

// 1. Dependency scanning
const depcheck = require('depcheck');

const options = {
  ignoreBinPackage: false,
  skipMissing: false,
  ignoreDirs: ['dist', 'build', 'node_modules']
};

depcheck('/path/to/project', options, (unused) => {
  console.log('Unused dependencies:', unused.dependencies);
  console.log('Unused devDependencies:', unused.devDependencies);
  console.log('Missing dependencies:', unused.missing);
});

// 2. npm audit for vulnerabilities
// In package.json scripts:
{
  "scripts": {
    "security-check": "npm audit --audit-level=moderate",
    "security-fix": "npm audit fix"
  }
}

// 3. OWASP Dependency Check
// Checks for known vulnerabilities in dependencies

// 4. Security headers check
const headers = {
  'Content-Security-Policy': "default-src 'self'; script-src 'self' 'unsafe-inline'",
  'X-Content-Type-Options': 'nosniff',
  'X-Frame-Options': 'SAMEORIGIN',
  'X-XSS-Protection': '1; mode=block',
  'Strict-Transport-Security': 'max-age=31536000; includeSubDomains',
  'Referrer-Policy': 'strict-origin-when-cross-origin'
};

// 5. Code scanning with SAST tools
// - SonarQube
// - Checkmarx
// - Snyk
// - GitHub Advanced Security
```

**Documentation Standards**:
```markdown
# Project Documentation Structure

## README.md
- Project overview
- Installation instructions
- Quick start guide
- Links to detailed docs

## CONTRIBUTING.md
- How to contribute
- Code style guide
- Pull request process
- Development setup

## API Documentation
- Endpoints list
- Request/response examples
- Authentication
- Error codes

Example API Doc (OpenAPI/Swagger):

```yaml
openapi: 3.0.0
info:
  title: E-commerce API
  version: 1.0.0
  description: API for managing products and orders

paths:
  /api/products:
    get:
      summary: List all products
      parameters:
        - name: category
          in: query
          schema:
            type: string
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Product'
    
    post:
      summary: Create a new product
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ProductInput'
      responses:
        '201':
          description: Product created
        '401':
          description: Unauthorized

components:
  schemas:
    Product:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        price:
          type: number
        category:
          type: string
        createdAt:
          type: string
          format: date-time
    
    ProductInput:
      type: object
      required:
        - name
        - price
        - category
      properties:
        name:
          type: string
        price:
          type: number
          minimum: 0
        category:
          type: string
        description:
          type: string

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
```

## Code Comments
```javascript
/**
 * Calculates the total price of items in cart
 * including tax and shipping
 * 
 * @param {Array<CartItem>} items - Array of cart items
 * @param {Object} options - Calculation options
 * @param {number} options.taxRate - Tax rate (e.g., 0.08 for 8%)
 * @param {number} options.shippingCost - Flat shipping cost
 * @returns {Object} Price breakdown
 * @returns {number} return.subtotal - Sum of item prices
 * @returns {number} return.tax - Tax amount
 * @returns {number} return.shipping - Shipping cost
 * @returns {number} return.total - Grand total
 * 
 * @example
 * const items = [
 *   { price: 100, quantity: 2 },
 *   { price: 50, quantity: 1 }
 * ];
 * const result = calculateTotal(items, { taxRate: 0.08, shippingCost: 10 });
 * // result.total = 280 (250 + 20 tax + 10 shipping)
 */
function calculateTotal(items, options = {}) {
  const { taxRate = 0, shippingCost = 0 } = options;
  
  const subtotal = items.reduce(
    (sum, item) => sum + (item.price * item.quantity),
    0
  );
  
  const tax = subtotal * taxRate;
  const total = subtotal + tax + shippingCost;
  
  return {
    subtotal: parseFloat(subtotal.toFixed(2)),
    tax: parseFloat(tax.toFixed(2)),
    shipping: parseFloat(shippingCost.toFixed(2)),
    total: parseFloat(total.toFixed(2))
  };
}
```
```

### 6. Tracking the Project

**Project Metrics and KPIs**:

**Development Metrics**:
```
Velocity:
- Story points completed per sprint
- Trend over time
- Used for sprint planning

Example:
Sprint 1: 25 points
Sprint 2: 30 points
Sprint 3: 28 points
Sprint 4: 32 points
Average: 29 points (use for planning)

Burndown Chart:
Work Remaining (Story Points)
 ^
 |  *
 |   *  *
 |     *  *
 |       *  *
 |         *  *  *
 |_________________> Time (Days)
  1  2  3  4  5  6  7

Lead Time:
Time from request to deployment
- Feature requested: Jan 1
- Development started: Jan 5
- Deployed: Jan 15
- Lead time: 15 days

Cycle Time:
Time from start to completion
- Started: Jan 5
- Completed: Jan 15
- Cycle time: 10 days

Code Churn:
Lines of code added, modified, deleted
High churn = potential quality issues
```

**Quality Metrics**:
```
Defect Density:
Defects / KLOC (thousands of lines of code)

Example:
50 bugs / 10,000 lines = 5 defects per KLOC

Defect Removal Efficiency:
(Defects found before release / Total defects) × 100

Example:
90 found in testing / 100 total = 90% efficiency

Test Coverage:
(Lines executed / Total lines) × 100

Code Review Coverage:
(PRs reviewed / Total PRs) × 100
Target: 100%

Technical Debt:
SonarQube debt ratio
Time to fix all code smells
```

**Business Metrics**:
```
User Acquisition:
- New users per week/month
- Source of users (organic, paid, referral)
- Cost per acquisition (CPA)

User Engagement:
- Daily Active Users (DAU)
- Monthly Active Users (MAU)
- DAU/MAU ratio (stickiness)
- Session duration
- Pages per session

Conversion Rate:
(Conversions / Total visitors) × 100

Example:
500 signups / 10,000 visitors = 5% conversion

Churn Rate:
(Users lost / Total users at start) × 100

Example:
50 cancellations / 1000 users = 5% monthly churn

Customer Lifetime Value (CLV):
Average revenue per user × Average lifetime

Example:
$50/month × 24 months = $1,200 CLV

Net Promoter Score (NPS):
"How likely are you to recommend us?" (0-10)
- Promoters (9-10): +1
- Passives (7-8): 0
- Detractors (0-6): -1

NPS = % Promoters - % Detractors
```

**Monitoring and Alerting**:
```javascript
// Example monitoring setup with Winston and Slack

const winston = require('winston');
const SlackHook = require('winston-slack-webhook-transport');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    // Log to file
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' }),
    
    // Log to console
    new winston.transports.Console({
      format: winston.format.simple()
    }),
    
    // Send critical errors to Slack
    new SlackHook({
      webhookUrl: process.env.SLACK_WEBHOOK_URL,
      level: 'error',
      formatter: (info) => {
        return {
          text: `🚨 Error in ${process.env.APP_NAME}`,
          attachments: [{
            color: 'danger',
            fields: [{
              title: 'Message',
              value: info.message
            }, {
              title: 'Stack Trace',
              value: info.stack || 'N/A'
            }, {
              title: 'Environment',
              value: process.env.NODE_ENV
            }]
          }]
        };
      }
    })
  ]
});

// Application Performance Monitoring (APM)
const newrelic = require('newrelic');

// Custom metrics
app.use((req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    const duration = Date.now() - start;
    
    // Log slow requests
    if (duration > 1000) {
      logger.warn('Slow request', {
        method: req.method,
        url: req.url,
        duration: duration,
        statusCode: res.statusCode
      });
    }
    
    // Send custom metric to New Relic
    newrelic.recordMetric('Custom/ResponseTime', duration);
  });
  
  next();
});

// Error tracking with Sentry
const Sentry = require('@sentry/node');

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: 0.1 // 10% of transactions
});

app.use(Sentry.Handlers.requestHandler());
app.use(Sentry.Handlers.errorHandler());

// Health check endpoint
app.get('/health', async (req, res) => {
  const health = {
    uptime: process.uptime(),
    timestamp: Date.now(),
    status: 'OK'
  };
  
  try {
    // Check database connection
    await db.query('SELECT 1');
    health.database = 'Connected';
    
    // Check Redis connection
    await redis.ping();
    health.cache = 'Connected';
    
    // Check external API
    const apiResponse = await fetch('https://api.example.com/health');
    health.externalApi = apiResponse.ok ? 'OK' : 'Degraded';
    
  } catch (error) {
    health.status = 'Degraded';
    health.error = error.message;
    
    logger.error('Health check failed', { error: error.message });
    
    return res.status(503).json(health);
  }
  
  res.json(health);
});

// Uptime monitoring
// Use services like:
// - Pingdom
// - UptimeRobot
// - StatusCake
// - DataDog

// Alert rules example
const alertRules = {
  errorRate: {
    threshold: 5, // 5% error rate
    window: 300,  // 5 minutes
    action: 'page-oncall'
  },
  responseTime: {
    threshold: 1000, // 1 second
    percentile: 95,  // 95th percentile
    action: 'slack-channel'
  },
  availability: {
    threshold: 99.9, // 99.9% uptime
    window: 86400,   // 24 hours
    action: 'page-oncall'
  }
};
```

**Dashboard Example (Grafana)**:
```
┌─────────────────────────────────────────────────────┐
│ Application Dashboard - Production                   │
├─────────────────────────────────────────────────────┤
│                                                       │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐            │
│  │ 99.98%   │ │ 245ms    │ │ 1,234    │            │
│  │ Uptime   │ │ Avg Time │ │ Req/sec  │            │
│  └──────────┘ └──────────┘ └──────────┘            │
│                                                       │
│  ┌─────────────────────────────────────────────┐   │
│  │ Response Time (Last 24h)                     │   │
│  │  1000ms ┤      ╭╮                            │   │
│  │   750ms ┤   ╭╮ ││╭╮                          │   │
│  │   500ms ┤ ╭╮││╭╯╰╯│╭╮                        │   │
│  │   250ms ┤─╯╰╯╰╯   ╰╯╰────────────────────   │   │
│  │      0ms└──────────────────────────────────  │   │
│  └─────────────────────────────────────────────┘   │
│                                                       │
│  ┌────────────────┐  ┌────────────────┐            │
│  │ Error Rate      │  │ Active Users   │            │
│  │  0.5%  ↓       │  │  1,234  ↑      │            │
│  └────────────────┘  └────────────────┘            │
│                                                       │
│  ┌─────────────────────────────────────────────┐   │
│  │ Top Endpoints by Traffic                     │   │
│  │ /api/products        ████████████ 45%       │   │
│  │ /api/users           ████████     30%       │   │
│  │ /api/orders          ████         15%       │   │
│  │ /api/auth            ██           10%       │   │
│  └─────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

### 7. Introduction to Node.js - Web Sockets

**What are WebSockets?**

**Traditional HTTP** (Request-Response):
```
Client                    Server
  |                          |
  |--- Request (GET) ------> |
  |                          |
  | <----- Response -------- |
  |                          |
  |--- New Request --------> |
  |                          |
  | <----- Response -------- |
  
Limitations:
- Client must initiate
- Overhead for each request
- Polling for updates is inefficient
```

**WebSockets** (Full-Duplex):
```
Client                    Server
  |                          |
  |--- Handshake ---------> |
  | <-- Upgrade Confirm --- |
  |                          |
  | <====== Connected =====> |
  |                          |
  | <--- Message ----------- |
  | ---- Message ---------> |
  | <--- Message ----------- |
  |                          |
  
Benefits:
- Persistent connection
- Real-time, bidirectional
- Low latency
- Less overhead
```

**WebSocket Use Cases**:
```
1. Chat Applications:
   - Instant messaging
   - Group chats
   - Notifications

2. Live Updates:
   - Sports scores
   - Stock prices
   - News feeds

3. Collaborative Tools:
   - Google Docs-like editing
   - Whiteboarding
   - Code collaboration

4. Gaming:
   - Multiplayer games
   - Real-time interactions

5. IoT:
   - Sensor data streaming
   - Device control
   - Monitoring dashboards

6. Live Dashboards:
   - Analytics
   - Monitoring systems
   - Admin panels
```

**Basic WebSocket (Client-Side)**:
```javascript
// Create WebSocket connection
const socket = new WebSocket('ws://localhost:3000');

// Connection opened
socket.addEventListener('open', (event) => {
  console.log('Connected to WebSocket server');
  
  // Send message to server
  socket.send(JSON.stringify({
    type: 'greeting',
    message: 'Hello Server!'
  }));
});

// Listen for messages
socket.addEventListener('message', (event) => {
  console.log('Message from server:', event.data);
  
  const data = JSON.parse(event.data);
  
  // Handle different message types
  switch(data.type) {
    case 'chat':
      displayChatMessage(data);
      break;
    case 'notification':
      showNotification(data);
      break;
    case 'update':
      updateUI(data);
      break;
  }
});

// Connection closed
socket.addEventListener('close', (event) => {
  console.log('Disconnected from server');
  
  // Attempt to reconnect
  setTimeout(() => {
    console.log('Attempting to reconnect...');
    connectWebSocket();
  }, 3000);
});

// Connection error
socket.addEventListener('error', (error) => {
  console.error('WebSocket error:', error);
});

// Send message
function sendMessage(message) {
  if (socket.readyState === WebSocket.OPEN) {
    socket.send(JSON.stringify(message));
  } else {
    console.error('WebSocket is not connected');
  }
}
```

**WebSocket Server (Node.js with ws library)**:
```javascript
const WebSocket = require('ws');
const http = require('http');
const express = require('express');

const app = express();
const server = http.createServer(app);

// Create WebSocket server
const wss = new WebSocket.Server({ server });

// Store connected clients
const clients = new Set();

// Handle new connections
wss.on('connection', (ws, req) => {
  console.log('New client connected');
  
  // Add to clients set
  clients.add(ws);
  
  // Send welcome message
  ws.send(JSON.stringify({
    type: 'system',
    message: 'Welcome to the server!'
  }));
  
  // Broadcast user count
  broadcast({
    type: 'userCount',
    count: clients.size
  });
  
  // Handle incoming messages
  ws.on('message', (message) => {
    console.log('Received:', message);
    
    try {
      const data = JSON.parse(message);
      
      // Handle different message types
      switch(data.type) {
        case 'chat':
          // Broadcast chat message to all clients
          broadcast({
            type: 'chat',
            user: data.user,
            message: data.message,
            timestamp: new Date().toISOString()
          });
          break;
          
        case 'private':
          // Send to specific client (implementation needed)
          sendToUser(data.targetId, data.message);
          break;
          
        default:
          ws.send(JSON.stringify({
            type: 'error',
            message: 'Unknown message type'
          }));
      }
    } catch (error) {
      console.error('Error parsing message:', error);
    }
  });
  
  // Handle client disconnect
  ws.on('close', () => {
    console.log('Client disconnected');
    clients.delete(ws);
    
    // Broadcast updated user count
    broadcast({
      type: 'userCount',
      count: clients.size
    });
  });
  
  // Handle errors
  ws.on('error', (error) => {
    console.error('WebSocket error:', error);
  });
  
  // Heartbeat to detect broken connections
  ws.isAlive = true;
  ws.on('pong', () => {
    ws.isAlive = true;
  });
});

// Broadcast to all connected clients
function broadcast(data) {
  const message = JSON.stringify(data);
  
  clients.forEach(client => {
    if (client.readyState === WebSocket.OPEN) {
      client.send(message);
    }
  });
}

// Heartbeat interval to clean up broken connections
const heartbeat = setInterval(() => {
  wss.clients.forEach(ws => {
    if (ws.isAlive === false) {
      return ws.terminate();
    }
    
    ws.isAlive = false;
    ws.ping();
  });
}, 30000); // Every 30 seconds

// Cleanup on server shutdown
wss.on('close', () => {
  clearInterval(heartbeat);
});

// Start server
const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Server listening on port ${PORT}`);
});
```

**Socket.IO (Enhanced WebSocket Library)**:

**Why Socket.IO?**
```
Benefits over raw WebSockets:
- Automatic reconnection
- Fallback to HTTP long-polling
- Room/namespace support
- Broadcasting helpers
- Binary data support
- Better browser compatibility
```

**Socket.IO Server**:
```javascript
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIO(server, {
  cors: {
    origin: "http://localhost:3001",
    methods: ["GET", "POST"]
  }
});

// Serve static files
app.use(express.static('public'));

// Middleware for authentication
io.use((socket, next) => {
  const token = socket.handshake.auth.token;
  
  if (isValidToken(token)) {
    // Attach user info to socket
    socket.userId = getUserIdFromToken(token);
    socket.username = getUsernameFromToken(token);
    next();
  } else {
    next(new Error('Authentication error'));
  }
});

// Handle connections
io.on('connection', (socket) => {
  console.log(`User connected: ${socket.username} (${socket.id})`);
  
  // Join user to their personal room
  socket.join(`user:${socket.userId}`);
  
  // Broadcast to all except sender
  socket.broadcast.emit('user-joined', {
    userId: socket.userId,
    username: socket.username
  });
  
  // Listen for chat messages
  socket.on('chat-message', (data) => {
    // Emit to all clients in a room
    io.to(data.room).emit('chat-message', {
      user: socket.username,
      message: data.message,
      timestamp: new Date().toISOString()
    });
    
    // Save to database
    saveMessage(data.room, socket.userId, data.message);
  });
  
  // Private messaging
  socket.on('private-message', (data) => {
    // Send to specific user's room
    io.to(`user:${data.targetUserId}`).emit('private-message', {
      from: socket.username,
      message: data.message,
      timestamp: new Date().toISOString()
    });
  });
  
  // Join a room
  socket.on('join-room', (roomId) => {
    socket.join(roomId);
    
    // Notify room members
    socket.to(roomId).emit('user-joined-room', {
      username: socket.username,
      roomId: roomId
    });
    
    // Send room history
    getRoomHistory(roomId).then(messages => {
      socket.emit('room-history', messages);
    });
  });
  
  // Leave a room
  socket.on('leave-room', (roomId) => {
    socket.leave(roomId);
    
    socket.to(roomId).emit('user-left-room', {
      username: socket.username,
      roomId: roomId
    });
  });
  
  // Typing indicator
  socket.on('typing', (data) => {
    socket.to(data.room).emit('user-typing', {
      username: socket.username
    });
  });
  
  socket.on('stop-typing', (data) => {
    socket.to(data.room).emit('user-stop-typing', {
      username: socket.username
    });
  });
  
  // Handle disconnect
  socket.on('disconnect', (reason) => {
    console.log(`User disconnected: ${socket.username} (${reason})`);
    
    // Broadcast to all
    io.emit('user-left', {
      userId: socket.userId,
      username: socket.username
    });
  });
  
  // Error handling
  socket.on('error', (error) => {
    console.error('Socket error:', error);
  });
});

// Emit to specific user from server-side
function notifyUser(userId, data) {
  io.to(`user:${userId}`).emit('notification', data);
}

// Broadcast to all connected clients
function broadcastUpdate(data) {
  io.emit('update', data);
}

// Example: Broadcast stock price update every second
setInterval(() => {
  const stockPrice = {
    symbol: 'AAPL',
    price: (Math.random() * 200).toFixed(2),
    change: (Math.random() * 10 - 5).toFixed(2)
  };
  
  io.emit('stock-update', stockPrice);
}, 1000);

server.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

**Socket.IO Client**:
```javascript
// Connect to server
const socket = io('http://localhost:3000', {
  auth: {
    token: localStorage.getItem('authToken')
  },
  reconnection: true,
  reconnectionAttempts: 5,
  reconnectionDelay: 1000
});

// Connection events
socket.on('connect', () => {
  console.log('Connected to server:', socket.id);
  updateConnectionStatus(true);
});

socket.on('disconnect', (reason) => {
  console.log('Disconnected:', reason);
  updateConnectionStatus(false);
  
  if (reason === 'io server disconnect') {
    // Server forcefully disconnected, reconnect manually
    socket.connect();
  }
});

socket.on('connect_error', (error) => {
  console.error('Connection error:', error.message);
  showError('Failed to connect to server');
});

// Listen for events
socket.on('chat-message', (data) => {
  displayMessage(data);
});

socket.on('user-joined', (data) => {
  showNotification(`${data.username} joined`);
});

socket.on('notification', (data) => {
  showNotification(data.message);
});

socket.on('stock-update', (data) => {
  updateStockPrice(data);
});

// Emit events
function sendMessage(room, message) {
  socket.emit('chat-message', {
    room: room,
    message: message
  });
}

function joinRoom(roomId) {
  socket.emit('join-room', roomId);
}

function sendPrivateMessage(targetUserId, message) {
  socket.emit('private-message', {
    targetUserId: targetUserId,
    message: message
  });
}

// Typing indicator with debounce
let typingTimeout;

messageInput.addEventListener('input', () => {
  socket.emit('typing', { room: currentRoom });
  
  clearTimeout(typingTimeout);
  typingTimeout = setTimeout(() => {
    socket.emit('stop-typing', { room: currentRoom });
  }, 1000);
});
```

**Real-World Example: Live Chat Application**:

```html
<!-- chat.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Live Chat</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    
    body {
      font-family: Arial, sans-serif;
      display: flex;
      height: 100vh;
    }
    
    .sidebar {
      width: 250px;
      background: #2c3e50;
      color: white;
      padding: 20px;
    }
    
    .room-list {
      list-style: none;
      margin-top: 20px;
    }
    
    .room-item {
      padding: 10px;
      margin: 5px 0;
      background: #34495e;
      border-radius: 5px;
      cursor: pointer;
    }
    
    .room-item:hover {
      background: #1abc9c;
    }
    
    .room-item.active {
      background: #16a085;
    }
    
    .chat-container {
      flex: 1;
      display: flex;
      flex-direction: column;
    }
    
    .chat-header {
      padding: 20px;
      background: #ecf0f1;
      border-bottom: 1px solid #bdc3c7;
    }
    
    .messages {
      flex: 1;
      padding: 20px;
      overflow-y: auto;
      background: #f8f9fa;
    }
    
    .message {
      margin: 10px 0;
      padding: 10px 15px;
      background: white;
      border-radius: 10px;
      max-width: 70%;
    }
    
    .message.own {
      margin-left: auto;
      background: #3498db;
      color: white;
    }
    
    .message .username {
      font-weight: bold;
      margin-bottom: 5px;
      font-size: 0.9em;
    }
    
    .message .text {
      word-wrap: break-word;
    }
    
    .message .timestamp {
      font-size: 0.8em;
      opacity: 0.7;
      margin-top: 5px;
    }
    
    .typing-indicator {
      padding: 10px 20px;
      font-style: italic;
      color: #7f8c8d;
    }
    
    .input-container {
      padding: 20px;
      background: white;
      border-top: 1px solid #bdc3c7;
      display: flex;
      gap: 10px;
    }
    
    .message-input {
      flex: 1;
      padding: 12px;
      border: 1px solid #bdc3c7;
      border-radius: 25px;
      outline: none;
      font-size: 14px;
    }
    
    .send-button {
      padding: 12px 30px;
      background: #3498db;
      color: white;
      border: none;
      border-radius: 25px;
      cursor: pointer;
      font-size: 14px;
    }
    
    .send-button:hover {
      background: #2980b9;
    }
    
    .connection-status {
      padding: 5px 10px;
      border-radius: 5px;
      font-size: 0.9em;
      margin-bottom: 10px;
    }
    
    .connection-status.connected {
      background: #2ecc71;
    }
    
    .connection-status.disconnected {
      background: #e74c3c;
    }
  </style>
</head>
<body>
  <div class="sidebar">
    <div class="connection-status" id="status">Connecting...</div>
    <h3>Rooms</h3>
    <ul class="room-list" id="roomList">
      <li class="room-item active" data-room="general">General</li>
      <li class="room-item" data-room="tech">Tech Talk</li>
      <li class="room-item" data-room="random">Random</li>
    </ul>
    <div style="margin-top: 20px;">
      <strong>Online Users:</strong>
      <div id="userCount">0</div>
    </div>
  </div>
  
  <div class="chat-container">
    <div class="chat-header">
      <h2 id="roomName">General</h2>
    </div>
    
    <div class="messages" id="messages"></div>
    
    <div class="typing-indicator" id="typingIndicator"></div>
    
    <div class="input-container">
      <input 
        type="text" 
        class="message-input" 
        id="messageInput" 
        placeholder="Type a message..."
        autocomplete="off"
      >
      <button class="send-button" id="sendButton">Send</button>
    </div>
  </div>
  
  <script src="/socket.io/socket.io.js"></script>
  <script>
    // Get username (in real app, from login)
    const username = prompt('Enter your name:') || 'Anonymous';
    
    // Connect to server
    const socket = io('http://localhost:3000', {
      auth: { token: 'dummy-token' } // In real app, use actual auth token
    });
    
    let currentRoom = 'general';
    
    // DOM elements
    const statusEl = document.getElementById('status');
    const messagesEl = document.getElementById('messages');
    const messageInput = document.getElementById('messageInput');
    const sendButton = document.getElementById('sendButton');
    const roomList = document.getElementById('roomList');
    const roomName = document.getElementById('roomName');
    const userCount = document.getElementById('userCount');
    const typingIndicator = document.getElementById('typingIndicator');
    
    // Connection status
    socket.on('connect', () => {
      statusEl.textContent = 'Connected';
      statusEl.className = 'connection-status connected';
      socket.emit('join-room', currentRoom);
    });
    
    socket.on('disconnect', () => {
      statusEl.textContent = 'Disconnected';
      statusEl.className = 'connection-status disconnected';
    });
    
    // Receive messages
    socket.on('chat-message', (data) => {
      displayMessage(data);
    });
    
    // Room history
    socket.on('room-history', (messages) => {
      messagesEl.innerHTML = '';
      messages.forEach(msg => displayMessage(msg));
    });
    
    // User count
    socket.on('userCount', (data) => {
      userCount.textContent = data.count;
    });
    
    // Typing indicators
    const typingUsers = new Set();
    
    socket.on('user-typing', (data) => {
      typingUsers.add(data.username);
      updateTypingIndicator();
    });
    
    socket.on('user-stop-typing', (data) => {
      typingUsers.delete(data.username);
      updateTypingIndicator();
    });
    
    function updateTypingIndicator() {
      if (typingUsers.size > 0) {
        const users = Array.from(typingUsers).join(', ');
        typingIndicator.textContent = `${users} ${typingUsers.size === 1 ? 'is' : 'are'} typing...`;
      } else {
        typingIndicator.textContent = '';
      }
    }
    
    // Send message
    function sendMessage() {
      const message = messageInput.value.trim();
      
      if (message) {
        socket.emit('chat-message', {
          room: currentRoom,
          message: message
        });
        
        messageInput.value = '';
        socket.emit('stop-typing', { room: currentRoom });
      }
    }
    
    sendButton.addEventListener('click', sendMessage);
    
    messageInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        sendMessage();
      }
    });
    
    // Typing indicator
    let typingTimeout;
    messageInput.addEventListener('input', () => {
      socket.emit('typing', { room: currentRoom });
      
      clearTimeout(typingTimeout);
      typingTimeout = setTimeout(() => {
        socket.emit('stop-typing', { room: currentRoom });
      }, 1000);
    });
    
    // Display message
    function displayMessage(data) {
      const messageDiv = document.createElement('div');
      messageDiv.className = `message ${data.user === username ? 'own' : ''}`;
      
      messageDiv.innerHTML = `
        ${data.user !== username ? `<div class="username">${data.user}</div>` : ''}
        <div class="text">${escapeHtml(data.message)}</div>
        <div class="timestamp">${formatTime(data.timestamp)}</div>
      `;
      
      messagesEl.appendChild(messageDiv);
      messagesEl.scrollTop = messagesEl.scrollHeight;
    }
    
    // Switch rooms
    roomList.addEventListener('click', (e) => {
      if (e.target.classList.contains('room-item')) {
        const newRoom = e.target.dataset.room;
        
        if (newRoom !== currentRoom) {
          // Leave current room
          socket.emit('leave-room', currentRoom);
          
          // Update UI
          document.querySelector('.room-item.active').classList.remove('active');
          e.target.classList.add('active');
          
          // Join new room
          currentRoom = newRoom;
          roomName.textContent = e.target.textContent;
          messagesEl.innerHTML = '';
          
          socket.emit('join-room', currentRoom);
        }
      }
    });
    
    // Helper functions
    function escapeHtml(text) {
      const div = document.createElement('div');
      div.textContent = text;
      return div.innerHTML;
    }
    
    function formatTime(timestamp) {
      const date = new Date(timestamp);
      return date.toLocaleTimeString('en-US', { 
        hour: '2-digit', 
        minute: '2-digit' 
      });
    }
  </script>
</body>
</html>
```

---

## EXAM PREPARATION GUIDE

### Important Topics Summary

**Unit I - Introduction**:
- ✅ Web Engineering motivation and definition
- ✅ Categories of web applications
- ✅ Web application characteristics
- ✅ Difference between web design and development
- ✅ API basics and applications

**Unit II - Design**:
- ✅ Information design principles
- ✅ Software design as programming activity
- ✅ Merging information and software design
- ✅ Problems in integrated design
- ✅ Presentation design principles
- ✅ Form organization and validation

**Unit III - Device Independence**:
- ✅ Device-independent development concepts
- ✅ Interaction design principles
- ✅ User interface organization
- ✅ Structured dialog for complex activities
- ✅ Technology and architecture interplay
- ✅ MVC, Three-tier, Microservices architectures

**Unit IV - Testing**:
- ✅ Types of web testing (Unit, Integration, E2E)
- ✅ Test automation tools (Jest, Selenium, Cypress)
- ✅ Performance testing
- ✅ Security testing
- ✅ Accessibility testing
- ✅ Test-Driven Development (TDD)

**Unit V - Promotion**:
- ✅ Web application promotion strategies
- ✅ Content management and strategy
- ✅ Usage analysis (Google Analytics)
- ✅ A/B testing
- ✅ Project management (Agile, Waterfall)
- ✅ Quality management
- ✅ Project tracking metrics
- ✅ WebSockets and Socket.IO

### Key Concepts to Remember

**Architecture Patterns**:
```
MVC:
Model (Data) ↔ Controller (Logic) ↔ View (UI)

Three-Tier:
Presentation ↔ Application ↔ Data

Microservices:
Independent services communicating via APIs
```

**Testing Pyramid**:
```
       E2E (Few)
      /    \
   Integration (Some)
  /            \
Unit Tests (Many)
```

**Web Security**:
```
Common Vulnerabilities:
- SQL Injection
- XSS (Cross-Site Scripting)
- CSRF (Cross-Site Request Forgery)
- Insecure authentication
- Data exposure

Protections:
- Input validation
- Prepared statements
- HTTPS/SSL
- Security headers
- Authentication tokens (JWT)
```

**Performance Optimization**:
```
Frontend:
- Minimize HTTP requests
- Compress assets
- Lazy loading
- Code splitting
- Caching

Backend:
- Database indexing
- Query optimization
- Caching (Redis)
- Load balancing
- CDN usage
```

### Practice Questions

**Question 1**: Explain the difference between MVC and Microservices architecture with examples.

**Question 2**: What are the key differences between REST and WebSockets? When would you use each?

**Question 3**: Describe the complete testing strategy for a web application including unit, integration, and E2E tests.

**Question 4**: How would you implement user authentication and authorization in a web application?

**Question 5**: Explain responsive web design and how to implement it using CSS media queries.

**Question 6**: What metrics would you track to measure the success of a web application?

**Question 7**: Describe the Agile development process and how it differs from Waterfall.

**Question 8**: How do you ensure web accessibility (WCAG compliance)?

**Question 9**: Explain Content Security Policy (CSP) and why it's important.

**Question 10**: How would you optimize a slow-loading web page?

### Final Checklist

```
□ Understand web engineering principles
□ Know different web application types
□ Can explain MVC architecture
□ Understand REST API concepts
□ Know responsive design techniques
□ Familiar with form validation
□ Understand testing types and tools
□ Know security best practices (OWASP)
□ Understand Agile methodology
□ Can explain WebSocket usage
□ Know performance optimization techniques
□ Understand accessibility requirements
□ Familiar with analytics and tracking
□ Know project management concepts
□ Can write basic HTML/CSS/JavaScript
□ Understand database basics
□ Know version control (Git) basics
```

---

**All the Best for Your Web Engineering Exam! 🌐💻🎓**

You now have COMPLETE notes covering:
- All 5 units in comprehensive detail
- Practical code examples
- Architecture patterns
- Testing strategies
- Real-world implementations
- Security practices
- Performance optimization
- Project management
- WebSockets and real-time communication
- Exam preparation tips
