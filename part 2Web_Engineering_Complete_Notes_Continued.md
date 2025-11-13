# Web Engineering - COMPLETE Notes (Continuation)

---

## UNIT II: WEB APPLICATION DESIGN (Continued from Form Organization)

### 8. Form Organization (Continued)

**Example Well-Organized Form**:
```html
<form class="registration-form">
  <!-- Personal Information Section -->
  <fieldset>
    <legend>Personal Information</legend>
    
    <div class="form-group">
      <label for="firstName">First Name *</label>
      <input type="text" id="firstName" name="firstName" required>
      <span class="error" id="firstName-error"></span>
    </div>
    
    <div class="form-group">
      <label for="lastName">Last Name *</label>
      <input type="text" id="lastName" name="lastName" required>
    </div>
    
    <div class="form-group">
      <label for="email">Email Address *</label>
      <input type="email" id="email" name="email" required>
      <small class="help-text">We'll never share your email</small>
    </div>
    
    <div class="form-group">
      <label for="phone">Phone Number</label>
      <input type="tel" id="phone" name="phone" 
             pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
             placeholder="123-456-7890">
    </div>
  </fieldset>
  
  <!-- Address Section -->
  <fieldset>
    <legend>Address</legend>
    
    <div class="form-group">
      <label for="address">Street Address *</label>
      <input type="text" id="address" name="address" required>
    </div>
    
    <div class="form-row">
      <div class="form-group half">
        <label for="city">City *</label>
        <input type="text" id="city" name="city" required>
      </div>
      
      <div class="form-group quarter">
        <label for="state">State *</label>
        <select id="state" name="state" required>
          <option value="">Select...</option>
          <option value="CA">California</option>
          <option value="NY">New York</option>
          <!-- more options -->
        </select>
      </div>
      
      <div class="form-group quarter">
        <label for="zip">ZIP Code *</label>
        <input type="text" id="zip" name="zip" 
               pattern="[0-9]{5}" required>
      </div>
    </div>
  </fieldset>
  
  <!-- Submit Section -->
  <div class="form-actions">
    <button type="submit" class="btn-primary">Submit</button>
    <button type="reset" class="btn-secondary">Clear Form</button>
  </div>
</form>
```

**Form Validation**:

**Client-Side Validation (JavaScript)**:
```javascript
const form = document.querySelector('.registration-form');

form.addEventListener('submit', function(e) {
  e.preventDefault();
  
  // Clear previous errors
  clearErrors();
  
  let isValid = true;
  
  // Validate first name
  const firstName = document.getElementById('firstName');
  if (firstName.value.trim() === '') {
    showError('firstName', 'First name is required');
    isValid = false;
  }
  
  // Validate email
  const email = document.getElementById('email');
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailPattern.test(email.value)) {
    showError('email', 'Please enter a valid email address');
    isValid = false;
  }
  
  // Validate phone (if provided)
  const phone = document.getElementById('phone');
  if (phone.value && !isValidPhone(phone.value)) {
    showError('phone', 'Phone format: 123-456-7890');
    isValid = false;
  }
  
  // If all valid, submit
  if (isValid) {
    submitForm();
  }
});

function showError(fieldId, message) {
  const errorSpan = document.getElementById(fieldId + '-error');
  errorSpan.textContent = message;
  errorSpan.style.color = 'red';
  
  const field = document.getElementById(fieldId);
  field.classList.add('error-field');
  field.setAttribute('aria-invalid', 'true');
}

function clearErrors() {
  const errors = document.querySelectorAll('.error');
  errors.forEach(error => error.textContent = '');
  
  const errorFields = document.querySelectorAll('.error-field');
  errorFields.forEach(field => {
    field.classList.remove('error-field');
    field.removeAttribute('aria-invalid');
  });
}

function isValidPhone(phone) {
  const pattern = /^\d{3}-\d{3}-\d{4}$/;
  return pattern.test(phone);
}

function submitForm() {
  const formData = new FormData(form);
  
  fetch('/api/register', {
    method: 'POST',
    body: formData
  })
  .then(response => response.json())
  .then(data => {
    if (data.success) {
      showSuccess('Registration successful!');
    } else {
      showError('form', data.message);
    }
  })
  .catch(error => {
    showError('form', 'An error occurred. Please try again.');
  });
}
```

**Progressive Enhancement**:
```html
<!-- Basic HTML form works without JavaScript -->
<form action="/register" method="POST">
  <input type="text" name="firstName" required>
  <input type="email" name="email" required>
  <button type="submit">Submit</button>
</form>

<!-- Enhanced with JavaScript -->
<script>
  // Add real-time validation
  // Add AJAX submission
  // Add autocomplete features
  // But form still works if JS fails
</script>
```

**Accessibility Best Practices**:
```html
<!-- Proper labels -->
<label for="username">Username</label>
<input type="text" id="username" name="username">

<!-- Required field indication -->
<label for="email">
  Email Address 
  <span aria-label="required">*</span>
</label>

<!-- Error messages linked to inputs -->
<input type="text" 
       id="firstName" 
       aria-describedby="firstName-error"
       aria-invalid="false">
<span id="firstName-error" role="alert"></span>

<!-- Fieldset for grouped fields -->
<fieldset>
  <legend>Shipping Address</legend>
  <!-- address fields -->
</fieldset>

<!-- ARIA live regions for dynamic messages -->
<div role="status" aria-live="polite" aria-atomic="true">
  <!-- Success/error messages appear here -->
</div>
```

---

## UNIT III: DEVICE-INDEPENDENT DEVELOPMENT CONCEPTS (Continued)

### 9. Structured Dialog for Complex Activities

**What is Structured Dialog?**

**Definition**: A systematic approach to designing user interactions that guides users through complex, multi-step processes.

**Principles of Structured Dialog**:

**1. Clear Entry and Exit Points**:
```
User knows:
- Where they are
- What they're doing
- How to proceed
- How to go back
- How to cancel

Example: Checkout Process
[Shopping Cart] → [Shipping] → [Payment] → [Review] → [Confirmation]
      ↑              ↓            ↓           ↓
   [Back]        [Next]       [Next]      [Place Order]
```

**2. Logical Flow**:
```
Information gathered in logical order:
1. What? (Product selection)
2. Where? (Shipping address)
3. How? (Payment method)
4. Confirm (Review order)
5. Complete (Confirmation)

Progressive disclosure:
- Show only relevant information
- Don't overwhelm with all options
- Guide step-by-step
```

**3. Feedback and Confirmation**:
```
At each step:
- Show progress
- Validate input
- Provide feedback
- Confirm critical actions

Example:
Step 2 of 4: Shipping Information
✓ Contact details complete
⚠️ Address required
○ Payment pending
○ Review pending
```

**Wizard Pattern** (Step-by-Step):

**Implementation**:
```html
<div class="wizard">
  <!-- Progress indicator -->
  <div class="wizard-steps">
    <div class="step completed">
      <span class="step-number">1</span>
      <span class="step-label">Account</span>
    </div>
    <div class="step active">
      <span class="step-number">2</span>
      <span class="step-label">Profile</span>
    </div>
    <div class="step">
      <span class="step-number">3</span>
      <span class="step-label">Preferences</span>
    </div>
    <div class="step">
      <span class="step-number">4</span>
      <span class="step-label">Review</span>
    </div>
  </div>
  
  <!-- Current step content -->
  <div class="wizard-content">
    <h2>Step 2: Profile Information</h2>
    <form id="step2-form">
      <!-- Form fields for this step -->
    </form>
  </div>
  
  <!-- Navigation -->
  <div class="wizard-actions">
    <button class="btn-back">← Previous</button>
    <button class="btn-next">Next →</button>
    <button class="btn-cancel">Cancel</button>
  </div>
</div>
```

**JavaScript for Wizard**:
```javascript
class Wizard {
  constructor(container) {
    this.container = container;
    this.currentStep = 1;
    this.totalSteps = 4;
    this.data = {};
    
    this.init();
  }
  
  init() {
    // Attach event listeners
    this.container.querySelector('.btn-next')
      .addEventListener('click', () => this.nextStep());
    
    this.container.querySelector('.btn-back')
      .addEventListener('click', () => this.previousStep());
    
    this.updateUI();
  }
  
  nextStep() {
    // Validate current step
    if (!this.validateStep(this.currentStep)) {
      this.showError('Please complete all required fields');
      return;
    }
    
    // Save current step data
    this.saveStepData(this.currentStep);
    
    // Move to next step
    if (this.currentStep < this.totalSteps) {
      this.currentStep++;
      this.updateUI();
    } else {
      this.submit();
    }
  }
  
  previousStep() {
    if (this.currentStep > 1) {
      this.currentStep--;
      this.updateUI();
    }
  }
  
  validateStep(step) {
    const form = document.getElementById(`step${step}-form`);
    return form.checkValidity();
  }
  
  saveStepData(step) {
    const form = document.getElementById(`step${step}-form`);
    const formData = new FormData(form);
    
    formData.forEach((value, key) => {
      this.data[key] = value;
    });
  }
  
  updateUI() {
    // Update step indicators
    const steps = this.container.querySelectorAll('.step');
    steps.forEach((step, index) => {
      step.classList.remove('active', 'completed');
      
      if (index + 1 === this.currentStep) {
        step.classList.add('active');
      } else if (index + 1 < this.currentStep) {
        step.classList.add('completed');
      }
    });
    
    // Update buttons
    const backBtn = this.container.querySelector('.btn-back');
    const nextBtn = this.container.querySelector('.btn-next');
    
    backBtn.disabled = this.currentStep === 1;
    nextBtn.textContent = this.currentStep === this.totalSteps 
      ? 'Submit' 
      : 'Next →';
    
    // Load step content
    this.loadStepContent(this.currentStep);
  }
  
  loadStepContent(step) {
    // Load content for the current step
    // Could be AJAX or pre-loaded content
    const content = this.container.querySelector('.wizard-content');
    
    // Hide all step forms
    const forms = content.querySelectorAll('form');
    forms.forEach(form => form.style.display = 'none');
    
    // Show current step form
    const currentForm = document.getElementById(`step${step}-form`);
    currentForm.style.display = 'block';
  }
  
  submit() {
    // Submit all collected data
    fetch('/api/submit', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(this.data)
    })
    .then(response => response.json())
    .then(data => {
      if (data.success) {
        this.showConfirmation();
      } else {
        this.showError(data.message);
      }
    });
  }
  
  showConfirmation() {
    this.container.innerHTML = `
      <div class="wizard-confirmation">
        <h2>✓ Success!</h2>
        <p>Your information has been submitted.</p>
        <button class="btn-primary" onclick="window.location.href='/dashboard'">
          Go to Dashboard
        </button>
      </div>
    `;
  }
  
  showError(message) {
    const errorDiv = document.createElement('div');
    errorDiv.className = 'wizard-error';
    errorDiv.textContent = message;
    
    this.container.querySelector('.wizard-content')
      .prepend(errorDiv);
    
    setTimeout(() => errorDiv.remove(), 5000);
  }
}

// Initialize wizard
const wizard = new Wizard(document.querySelector('.wizard'));
```

**Alternative Pattern: Accordion** (for non-linear navigation):

```html
<div class="accordion">
  <div class="accordion-item">
    <button class="accordion-header" aria-expanded="true">
      <span class="icon">✓</span>
      Personal Information
    </button>
    <div class="accordion-content" aria-hidden="false">
      <!-- Content for personal info -->
    </div>
  </div>
  
  <div class="accordion-item">
    <button class="accordion-header" aria-expanded="false">
      <span class="icon">→</span>
      Address Details
    </button>
    <div class="accordion-content" aria-hidden="true">
      <!-- Content for address -->
    </div>
  </div>
  
  <div class="accordion-item disabled">
    <button class="accordion-header" disabled>
      <span class="icon">○</span>
      Payment Information
    </button>
    <div class="accordion-content" aria-hidden="true">
      <!-- Content for payment -->
    </div>
  </div>
</div>
```

### 10. Interplay with Technology and Architecture

**Technology Stack Considerations**:

**Frontend Technologies**:
```
HTML5:
- Semantic elements
- Form validation
- Offline capabilities (Service Workers)
- Geolocation API
- Canvas/SVG for graphics

CSS3:
- Responsive design (Media Queries, Flexbox, Grid)
- Animations and transitions
- Custom properties (CSS Variables)
- Transform and filters

JavaScript:
- ES6+ features (Arrow functions, Promises, Async/await)
- DOM manipulation
- Event handling
- AJAX/Fetch API

Frameworks/Libraries:
- React: Component-based, Virtual DOM
- Vue: Progressive framework
- Angular: Full-featured framework
- Svelte: Compiled framework
```

**Backend Technologies**:
```
Server-Side Languages:
- Node.js (JavaScript)
- Python (Django, Flask)
- PHP (Laravel, Symfony)
- Ruby (Rails)
- Java (Spring Boot)
- C# (.NET Core)

Databases:
- Relational: MySQL, PostgreSQL, SQL Server
- NoSQL: MongoDB, Redis, Cassandra
- Graph: Neo4j
- Search: Elasticsearch

APIs:
- REST (Representational State Transfer)
- GraphQL
- WebSocket (real-time)
- gRPC
```

**Architecture Patterns**:

**1. MVC (Model-View-Controller)**:
```
Model (Data):
- Business logic
- Database interaction
- Data validation

View (Presentation):
- User interface
- Display data
- User input forms

Controller (Logic):
- Request handling
- Coordinates Model and View
- Routing

Flow:
User Request → Controller → Model (get data) → Controller → View (display)
```

**Example (Express.js + MVC)**:
```javascript
// Model (models/User.js)
class User {
  constructor(db) {
    this.db = db;
  }
  
  async findById(id) {
    return await this.db.query('SELECT * FROM users WHERE id = ?', [id]);
  }
  
  async create(userData) {
    return await this.db.query('INSERT INTO users SET ?', userData);
  }
}

// Controller (controllers/userController.js)
const userController = {
  async getUser(req, res) {
    try {
      const userId = req.params.id;
      const user = await User.findById(userId);
      
      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }
      
      res.render('user/profile', { user });
    } catch (error) {
      res.status(500).json({ error: 'Server error' });
    }
  },
  
  async createUser(req, res) {
    try {
      const userData = req.body;
      const newUser = await User.create(userData);
      
      res.status(201).json({ 
        message: 'User created',
        user: newUser 
      });
    } catch (error) {
      res.status(400).json({ error: error.message });
    }
  }
};

// Routes (routes/users.js)
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

router.get('/users/:id', userController.getUser);
router.post('/users', userController.createUser);

module.exports = router;

// View (views/user/profile.ejs)
<!DOCTYPE html>
<html>
<head>
  <title><%= user.name %> - Profile</title>
</head>
<body>
  <h1><%= user.name %></h1>
  <p>Email: <%= user.email %></p>
  <p>Member since: <%= user.createdAt %></p>
</body>
</html>
```

**2. Three-Tier Architecture**:
```
Presentation Tier (Client):
- Web browser
- Mobile app
- Desktop application
- Handles UI/UX

Application Tier (Server):
- Business logic
- Data processing
- API endpoints
- Authentication/Authorization

Data Tier (Database):
- Data storage
- Data retrieval
- Data integrity
- Backup and recovery

Benefits:
- Separation of concerns
- Independent scaling
- Technology flexibility
- Easier maintenance
```

**3. Microservices Architecture**:
```
Instead of monolithic application:

                Monolith
    [All features in one codebase]
                  |
            Single Database

Microservices:

[User Service] → [User DB]
[Product Service] → [Product DB]
[Order Service] → [Order DB]
[Payment Service] → [Payment DB]
        ↓
  API Gateway
        ↓
    Client Apps

Benefits:
- Independent deployment
- Technology diversity
- Fault isolation
- Scalability
- Team autonomy

Challenges:
- Complexity
- Distributed data
- Network latency
- Debugging
```

**4. Serverless Architecture**:
```
Traditional:
- Manage servers
- Scale infrastructure
- Pay for idle time

Serverless:
- Functions as a Service (FaaS)
- Event-driven
- Auto-scaling
- Pay per execution

Example (AWS Lambda):
Function: Process image upload
Trigger: S3 upload event
Actions:
1. Resize image
2. Generate thumbnail
3. Store in different bucket
4. Update database

Benefits:
- No server management
- Auto-scaling
- Cost-effective
- Fast deployment
```

**Technology Integration Example**:

**Modern Web Application Stack**:
```
Frontend:
- React (UI library)
- Redux (State management)
- React Router (Routing)
- Axios (HTTP client)
- Styled Components (CSS-in-JS)

Backend:
- Node.js + Express
- MongoDB (Database)
- Mongoose (ODM)
- JWT (Authentication)
- Socket.io (Real-time)

DevOps:
- Docker (Containerization)
- Kubernetes (Orchestration)
- GitHub Actions (CI/CD)
- AWS/Azure (Cloud hosting)

Monitoring:
- New Relic (APM)
- Sentry (Error tracking)
- Google Analytics (User analytics)
```

**Full-Stack Application Example**:

**Project Structure**:
```
my-web-app/
├── client/                 # Frontend
│   ├── public/
│   ├── src/
│   │   ├── components/    # React components
│   │   ├── pages/         # Page components
│   │   ├── services/      # API calls
│   │   ├── utils/         # Helper functions
│   │   └── App.js
│   └── package.json
│
├── server/                 # Backend
│   ├── controllers/       # Request handlers
│   ├── models/            # Database models
│   ├── routes/            # API routes
│   ├── middleware/        # Custom middleware
│   ├── config/            # Configuration
│   └── server.js
│
├── database/              # Database scripts
│   ├── migrations/
│   └── seeds/
│
├── docker-compose.yml     # Docker configuration
└── README.md
```

**Frontend (React Component)**:
```javascript
// client/src/components/ProductList.js
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function ProductList() {
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetchProducts();
  }, []);
  
  async function fetchProducts() {
    try {
      const response = await axios.get('/api/products');
      setProducts(response.data);
      setLoading(false);
    } catch (err) {
      setError(err.message);
      setLoading(false);
    }
  }
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div className="product-list">
      <h2>Products</h2>
      <div className="products-grid">
        {products.map(product => (
          <div key={product.id} className="product-card">
            <img src={product.image} alt={product.name} />
            <h3>{product.name}</h3>
            <p>${product.price}</p>
            <button onClick={() => addToCart(product)}>
              Add to Cart
            </button>
          </div>
        ))}
      </div>
    </div>
  );
}

export default ProductList;
```

**Backend (Express API)**:
```javascript
// server/routes/products.js
const express = require('express');
const router = express.Router();
const Product = require('../models/Product');

// GET all products
router.get('/', async (req, res) => {
  try {
    const products = await Product.find()
      .sort({ createdAt: -1 })
      .limit(20);
    
    res.json(products);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// GET single product
router.get('/:id', async (req, res) => {
  try {
    const product = await Product.findById(req.params.id);
    
    if (!product) {
      return res.status(404).json({ message: 'Product not found' });
    }
    
    res.json(product);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

// POST new product (protected route)
router.post('/', authenticateToken, async (req, res) => {
  const product = new Product({
    name: req.body.name,
    description: req.body.description,
    price: req.body.price,
    image: req.body.image,
    category: req.body.category
  });
  
  try {
    const newProduct = await product.save();
    res.status(201).json(newProduct);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

// PUT update product
router.put('/:id', authenticateToken, async (req, res) => {
  try {
    const product = await Product.findById(req.params.id);
    
    if (!product) {
      return res.status(404).json({ message: 'Product not found' });
    }
    
    Object.assign(product, req.body);
    const updatedProduct = await product.save();
    
    res.json(updatedProduct);
  } catch (error) {
    res.status(400).json({ message: error.message });
  }
});

// DELETE product
router.delete('/:id', authenticateToken, async (req, res) => {
  try {
    const product = await Product.findById(req.params.id);
    
    if (!product) {
      return res.status(404).json({ message: 'Product not found' });
    }
    
    await product.remove();
    res.json({ message: 'Product deleted' });
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});

module.exports = router;
```

**Database Model (Mongoose)**:
```javascript
// server/models/Product.js
const mongoose = require('mongoose');

const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true
  },
  description: {
    type: String,
    required: true
  },
  price: {
    type: Number,
    required: true,
    min: 0
  },
  image: {
    type: String,
    required: true
  },
  category: {
    type: String,
    required: true,
    enum: ['Electronics', 'Clothing', 'Books', 'Home', 'Sports']
  },
  stock: {
    type: Number,
    required: true,
    min: 0,
    default: 0
  },
  ratings: [{
    user: {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'User'
    },
    rating: {
      type: Number,
      min: 1,
      max: 5
    },
    review: String,
    createdAt: {
      type: Date,
      default: Date.now
    }
  }]
}, {
  timestamps: true
});

// Virtual for average rating
productSchema.virtual('averageRating').get(function() {
  if (this.ratings.length === 0) return 0;
  
  const sum = this.ratings.reduce((acc, rating) => acc + rating.rating, 0);
  return (sum / this.ratings.length).toFixed(1);
});

// Index for search
productSchema.index({ name: 'text', description: 'text' });

module.exports = mongoose.model('Product', productSchema);
```

---

## UNIT IV: TESTING WEB APPLICATIONS

### 1. Introduction to Web Testing

**Why Test Web Applications?**

**Challenges Unique to Web Apps**:
```
1. Browser Compatibility:
   - Multiple browsers (Chrome, Firefox, Safari, Edge)
   - Different versions
   - Mobile browsers

2. Network Variability:
   - Slow connections
   - Intermittent connectivity
   - High latency

3. Concurrent Users:
   - Multiple simultaneous users
   - Race conditions
   - Database locking

4. Security Vulnerabilities:
   - SQL injection
   - XSS attacks
   - CSRF attacks

5. Dynamic Content:
   - AJAX requests
   - Real-time updates
   - Third-party integrations

6. Device Diversity:
   - Different screen sizes
   - Touch vs mouse
   - Various capabilities
```

**Testing Pyramid**:
```
           /\
          /E2E\         End-to-End Tests (Few)
         /----\         - Full user flows
        /Integ.\        - Slow, expensive
       /--------\       
      /  Unit    \      Integration Tests (Some)
     /------------\     - Component interactions
    /   Tests     \    
   /----------------\   Unit Tests (Many)
                        - Individual functions
                        - Fast, cheap
```

### 2. Types of Web Testing

**1. Functional Testing**:

**Unit Testing**:
```javascript
// Function to test
function calculateTax(amount, rate) {
  if (amount < 0 || rate < 0) {
    throw new Error('Amount and rate must be positive');
  }
  return amount * rate;
}

// Unit tests (using Jest)
describe('calculateTax', () => {
  test('calculates tax correctly', () => {
    expect(calculateTax(100, 0.08)).toBe(8);
  });
  
  test('handles zero amount', () => {
    expect(calculateTax(0, 0.08)).toBe(0);
  });
  
  test('throws error for negative amount', () => {
    expect(() => calculateTax(-100, 0.08))
      .toThrow('Amount and rate must be positive');
  });
  
  test('throws error for negative rate', () => {
    expect(() => calculateTax(100, -0.08))
      .toThrow('Amount and rate must be positive');
  });
});
```

**Integration Testing**:
```javascript
// Testing API integration
const request = require('supertest');
const app = require('../app');

describe('Product API', () => {
  test('GET /api/products returns product list', async () => {
    const response = await request(app)
      .get('/api/products')
      .expect('Content-Type', /json/)
      .expect(200);
    
    expect(Array.isArray(response.body)).toBe(true);
    expect(response.body.length).toBeGreaterThan(0);
  });
  
  test('POST /api/products creates new product', async () => {
    const newProduct = {
      name: 'Test Product',
      price: 99.99,
      category: 'Electronics'
    };
    
    const response = await request(app)
      .post('/api/products')
      .send(newProduct)
      .expect(201);
    
    expect(response.body).toHaveProperty('id');
    expect(response.body.name).toBe(newProduct.name);
  });
  
  test('GET /api/products/:id returns specific product', async () => {
    const response = await request(app)
      .get('/api/products/1')
      .expect(200);
    
    expect(response.body).toHaveProperty('id', 1);
  });
  
  test('GET /api/products/:id returns 404 for non-existent product', async () => {
    await request(app)
      .get('/api/products/99999')
      .expect(404);
  });
});
```

**End-to-End Testing**:
```javascript
// Using Cypress
describe('Shopping Cart Flow', () => {
  beforeEach(() => {
    cy.visit('/');
  });
  
  it('completes full purchase flow', () => {
    // Browse products
    cy.get('[data-testid="product-list"]').should('be.visible');
    
    // Add product to cart
    cy.get('[data-testid="product-1"]').within(() => {
      cy.get('[data-testid="add-to-cart"]').click();
    });
    
    // Verify cart badge updates
    cy.get('[data-testid="cart-badge"]').should('contain', '1');
    
    // Go to cart
    cy.get('[data-testid="cart-icon"]').click();
    cy.url().should('include', '/cart');
    
    // Verify product in cart
    cy.get('[data-testid="cart-items"]').should('have.length', 1);
    
    // Proceed to checkout
    cy.get('[data-testid="checkout-button"]').click();
    
    // Fill shipping information
    cy.get('#firstName').type('John');
    cy.get('#lastName').type('Doe');
    cy.get('#email').type('john@example.com');
    cy.get('#address').type('123 Main St');
    cy.get('#city').type('New York');
    cy.get('#state').select('NY');
    cy.get('#zip').type('10001');
    
    cy.get('[data-testid="continue-to-payment"]').click();
    
    // Fill payment information (test mode)
    cy.get('#cardNumber').type('4242424242424242');
    cy.get('#expiry').type('12/25');
    cy.get('#cvv').type('123');
    
    // Submit order
    cy.get('[data-testid="place-order"]').click();
    
    // Verify confirmation page
    cy.url().should('include', '/confirmation');
    cy.get('[data-testid="order-number"]').should('be.visible');
    cy.get('[data-testid="success-message"]')
      .should('contain', 'Order placed successfully');
  });
  
  it('handles validation errors', () => {
    cy.get('[data-testid="product-1"]').within(() => {
      cy.get('[data-testid="add-to-cart"]').click();
    });
    
    cy.get('[data-testid="cart-icon"]').click();
    cy.get('[data-testid="checkout-button"]').click();
    
    // Try to submit without filling required fields
    cy.get('[data-testid="continue-to-payment"]').click();
    
    // Verify error messages
    cy.get('[data-testid="firstName-error"]')
      .should('contain', 'First name is required');
    cy.get('[data-testid="email-error"]')
      .should('contain', 'Email is required');
  });
});
```

**2. Non-Functional Testing**:

**Performance Testing**:
```javascript
// Load testing with k6
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
  stages: [
    { duration: '1m', target: 50 },   // Ramp up to 50 users
    { duration: '3m', target: 50 },   // Stay at 50 users
    { duration: '1m', target: 100 },  // Ramp up to 100 users
    { duration: '3m', target: 100 },  // Stay at 100 users
    { duration: '1m', target: 0 },    // Ramp down
  ],
  thresholds: {
    'http_req_duration': ['p(95)<500'], // 95% of requests under 500ms
    'http_req_failed': ['rate<0.01'],   // Less than 1% failures
  },
};

export default function () {
  // Homepage
  let response = http.get('https://example.com/');
  check(response, {
    'homepage status is 200': (r) => r.status === 200,
    'homepage loads in < 500ms': (r) => r.timings.duration < 500,
  });
  
  sleep(1);
  
  // Product listing
  response = http.get('https://example.com/api/products');
  check(response, {
    'products API status is 200': (r) => r.status === 200,
    'products returned': (r) => JSON.parse(r.body).length > 0,
  });
  
  sleep(1);
  
  // Product detail
  response = http.get('https://example.com/api/products/1');
  check(response, {
    'product detail status is 200': (r) => r.status === 200,
  });
  
  sleep(2);
}
```

**Security Testing**:
```javascript
// Example security tests

describe('Security Tests', () => {
  // SQL Injection Prevention
  test('prevents SQL injection in search', async () => {
    const maliciousInput = "' OR '1'='1";
    
    const response = await request(app)
      .get(`/api/search?q=${encodeURIComponent(maliciousInput)}`)
      .expect(200);
    
    // Should return empty results, not all records
    expect(response.body.length).toBe(0);
  });
  
  // XSS Prevention
  test('prevents XSS in user input', async () => {
    const xssPayload = '<script>alert("XSS")</script>';
    
    const response = await request(app)
      .post('/api/comments')
      .send({ text: xssPayload })
      .expect(201);
    
    // Payload should be escaped
    expect(response.body.text).not.toContain('<script>');
    expect(response.body.text).toContain('&lt;script&gt;');
  });
  
  // CSRF Protection
  test('requires CSRF token for state-changing operations', async () => {
    await request(app)
      .post('/api/products')
      .send({ name: 'Test' })
      .expect(403); // Forbidden without CSRF token
  });
  
  // Authentication Required
  test('requires authentication for protected routes', async () => {
    await request(app)
      .get('/api/admin/users')
      .expect(401); // Unauthorized
  });
  
  // Authorization Enforced
  test('enforces role-based access control', async () => {
    const userToken = 'user-jwt-token';
    
    await request(app)
      .delete('/api/products/1')
      .set('Authorization', `Bearer ${userToken}`)
      .expect(403); // Forbidden - user not admin
  });
});
```

**Usability Testing**:
```
Manual testing focused on user experience:

1. Navigation:
   - Can users find what they're looking for?
   - Is navigation intuitive?
   - Clear labels and links?

2. Forms:
   - Easy to complete?
   - Clear error messages?
   - Helpful validation?

3. Accessibility:
   - Keyboard navigation works?
   - Screen reader compatible?
   - Sufficient color contrast?
   - Alt text for images?

4. Performance Perception:
   - Feels fast/responsive?
   - Loading indicators?
   - Smooth transitions?

Tools:
- User testing sessions
- A/B testing
- Heatmaps (Hotjar, Crazy Egg)
- Session recordings
```

### 3. Test Automation Tools

**1. Jest (JavaScript Testing)**:
```javascript
// Component testing (React)
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import '@testing-library/jest-dom';
import LoginForm from './LoginForm';

describe('LoginForm Component', () => {
  test('renders login form', () => {
    render(<LoginForm />);
    
    expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/password/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /login/i })).toBeInTheDocument();
  });
  
  test('shows error for invalid email', async () => {
    render(<LoginForm />);
    
    const emailInput = screen.getByLabelText(/email/i);
    const submitButton = screen.getByRole('button', { name: /login/i });
    
    fireEvent.change(emailInput, { target: { value: 'invalid-email' } });
    fireEvent.click(submitButton);
    
    expect(await screen.findByText(/valid email/i)).toBeInTheDocument();
  });
  
  test('submits form with valid credentials', async () => {
    const mockSubmit = jest.fn();
    render(<LoginForm onSubmit={mockSubmit} />);
    
    fireEvent.change(screen.getByLabelText(/email/i), {
      target: { value: 'user@example.com' }
    });
    fireEvent.change(screen.getByLabelText(/password/i), {
      target: { value: 'password123' }
    });
    
    fireEvent.click(screen.getByRole('button', { name: /login/i }));
    
    expect(mockSubmit).toHaveBeenCalledWith({
      email: 'user@example.com',
      password: 'password123'
    });
  });
});
```

**2. Selenium (Browser Automation)**:
```javascript
const { Builder, By, Key, until } = require('selenium-webdriver');

describe('Search Functionality', function() {
  let driver;
  
  this.timeout(30000); // Increase timeout for browser tests
  
  before(async function() {
    driver = await new Builder().forBrowser('chrome').build();
  });
  
  after(async function() {
    await driver.quit();
  });
  
  it('performs search and displays results', async function() {
    // Navigate to page
    await driver.get('https://example.com');
    
    // Find search input
    const searchBox = await driver.findElement(By.name('q'));
    
    // Enter search term
    await searchBox.sendKeys('laptop', Key.RETURN);
    
    // Wait for results
    await driver.wait(until.elementLocated(By.css('.search-results')), 10000);
    
    // Verify results
    const results = await driver.findElements(By.css('.search-result-item'));
    expect(results.length).toBeGreaterThan(0);
    
    // Check first result contains search term
    const firstResult = await results[0].getText();
    expect(firstResult.toLowerCase()).toContain('laptop');
  });
  
  it('shows "no results" message for invalid search', async function() {
    await driver.get('https://example.com');
    
    const searchBox = await driver.findElement(By.name('q'));
    await searchBox.sendKeys('xyzabc123nonexistent', Key.RETURN);
    
    await driver.wait(until.elementLocated(By.css('.no-results')), 10000);
    
    const noResultsMessage = await driver.findElement(By.css('.no-results')).getText();
    expect(noResultsMessage).toContain('No results found');
  });
});
```

**3. Cypress (Modern E2E Testing)**:
```javascript
// cypress/integration/checkout.spec.js

describe('Checkout Process', () => {
  beforeEach(() => {
    // Setup: Login before each test
    cy.login('user@example.com', 'password');
    cy.visit('/products');
  });
  
  it('completes checkout with valid card', () => {
    // Add product to cart
    cy.get('[data-product-id="123"]')
      .find('[data-testid="add-to-cart"]')
      .click();
    
    // Navigate to cart
    cy.get('[data-testid="cart-icon"]').click();
    
    // Proceed to checkout
    cy.get('[data-testid="checkout-btn"]').click();
    
    // Fill shipping form
    cy.get('#shipping-address').type('123 Main St');
    cy.get('#shipping-city').type('New York');
    cy.get('#shipping-zip').type('10001');
    cy.get('[data-testid="continue-payment"]').click();
    
    // Fill payment (using Stripe test card)
    cy.get('iframe[name*="stripe"]').then($iframe => {
      const $body = $iframe.contents().find('body');
      cy.wrap($body)
        .find('input[name="cardnumber"]')
        .type('4242424242424242');
      cy.wrap($body)
        .find('input[name="exp-date"]')
        .type('12/25');
      cy.wrap($body)
        .find('input[name="cvc"]')
        .type('123');
    });
    
    // Submit order
    cy.get('[data-testid="place-order"]').click();
    
    // Verify success
    cy.url().should('include', '/order-confirmation');
    cy.get('[data-testid="order-number"]').should('exist');
    
    // Verify email sent (using Cypress email plugin)
    cy.task('getLastEmail').then(email => {
      expect(email.subject).to.include('Order Confirmation');
      expect(email.html).to.include('Thank you for your order');
    });
  });
  
  it('shows error for declined card', () => {
    cy.get('[data-product-id="123"]')
      .find('[data-testid="add-to-cart"]')
      .click();
    
    cy.get('[data-testid="cart-icon"]').click();
    cy.get('[data-testid="checkout-btn"]').click();
    
    // Skip shipping (assume pre-filled)
    cy.get('[data-testid="continue-payment"]').click();
    
    // Use declined test card
    cy.get('iframe[name*="stripe"]').then($iframe => {
      const $body = $iframe.contents().find('body');
      cy.wrap($body)
        .find('input[name="cardnumber"]')
        .type('4000000000000002'); // Declined card
      cy.wrap($body)
        .find('input[name="exp-date"]')
        .type('12/25');
      cy.wrap($body)
        .find('input[name="cvc"]')
        .type('123');
    });
    
    cy.get('[data-testid="place-order"]').click();
    
    // Verify error message
    cy.get('[data-testid="payment-error"]')
      .should('be.visible')
      .and('contain', 'declined');
  });
  
  // Custom command for login
  Cypress.Commands.add('login', (email, password) => {
    cy.request({
      method: 'POST',
      url: '/api/login',
      body: { email, password }
    }).then(response => {
      window.localStorage.setItem('authToken', response.body.token);
    });
  });
});
```

### 4. Test-Driven Development (TDD)

**TDD Cycle**:
```
1. Red: Write failing test
     ↓
2. Green: Write minimal code to pass
     ↓
3. Refactor: Improve code while keeping tests green
     ↓
   Repeat
```

**Example - TDD for Shopping Cart**:

```javascript
// Step 1: Write failing test
describe('Shopping Cart', () => {
  test('starts empty', () => {
    const cart = new ShoppingCart();
    expect(cart.getItemCount()).toBe(0);
  });
});

// Step 2: Minimal implementation
class ShoppingCart {
  getItemCount() {
    return 0;
  }
}

// Step 3: Add more tests
test('adds item to cart', () => {
  const cart = new ShoppingCart();
  const product = { id: 1, name: 'Laptop', price: 999 };
  
  cart.addItem(product);
  
  expect(cart.getItemCount()).toBe(1);
  expect(cart.getItems()).toContain(product);
});

// Step 4: Implement
class ShoppingCart {
  constructor() {
    this.items = [];
  }
  
  getItemCount() {
    return this.items.length;
  }
  
  addItem(product) {
    this.items.push(product);
  }
  
  getItems() {
    return this.items;
  }
}

// Step 5: More tests
test('removes item from cart', () => {
  const cart = new ShoppingCart();
  const product = { id: 1, name: 'Laptop', price: 999 };
  
  cart.addItem(product);
  cart.removeItem(product.id);
  
  expect(cart.getItemCount()).toBe(0);
});

test('calculates total price', () => {
  const cart = new ShoppingCart();
  cart.addItem({ id: 1, price: 999 });
  cart.addItem({ id: 2, price: 499 });
  
  expect(cart.getTotal()).toBe(1498);
});

test('handles quantity', () => {
  const cart = new ShoppingCart();
  const product = { id: 1, price: 100 };
  
  cart.addItem(product, 3);
  
  expect(cart.getItemCount()).toBe(3);
  expect(cart.getTotal()).toBe(300);
});

// Step 6: Full implementation
class ShoppingCart {
  constructor() {
    this.items = [];
  }
  
  addItem(product, quantity = 1) {
    const existingItem = this.items.find(item => item.id === product.id);
    
    if (existingItem) {
      existingItem.quantity += quantity;
    } else {
      this.items.push({ ...product, quantity });
    }
  }
  
  removeItem(productId) {
    this.items = this.items.filter(item => item.id !== productId);
  }
  
  getItemCount() {
    return this.items.reduce((count, item) => count + item.quantity, 0);
  }
  
  getItems() {
    return this.items;
  }
  
  getTotal() {
    return this.items.reduce(
      (total, item) => total + (item.price * item.quantity),
      0
    );
  }
  
  clear() {
    this.items = [];
  }
}
```

---

## UNIT V: PROMOTING WEB APPLICATIONS

### 1. Introduction to Web Application Promotion

**Why Promote Web Applications?**

```
Build it and they will come ❌

Build it → Promote it → They will come ✓
```

**Promotion Goals**:
```
1. Awareness: Make people know you exist
2. Acquisition: Get users to try your app
3. Activation: Get users to first key action
4. Retention: Keep users coming back
5. Revenue: Convert to paying customers
6. Referral: Get users to bring others
```

**AARRR Metrics (Pirate Metrics)**:
```
Awareness   →  How many people know about you?
Acquisition →  How many visit your site?
Activation  →  How many have a good first experience?
Retention   →  How many come back?
Revenue     →  How many pay?
Referral    →  How many refer others?
```

### 2. Content Management

**What is Content Management?**

**Definition**: The process of creating, editing, organizing, and publishing content on your web application.

**Content Management System (CMS)**:
```
Popular CMS:
- WordPress (most popular)
- Drupal (enterprise)
- Joomla
- Ghost (blogging)
- Contentful (headless CMS)
- Strapi (headless CMS)
```

**Content Strategy**:

**1. Content Planning**:
```
Content Pillars (Main Topics):
Example for E-commerce App:
- Product guides and reviews
- How-to tutorials
- Industry news
- Customer stories
- Company updates

Content Calendar:
Week 1: Blog post on "Top 10 Products"
Week 2: Video tutorial "How to use feature X"
Week 3: Customer success story
Week 4: Product launch announcement
```

**2. Content Types**:
```
Blog Posts:
- Educational content
- SEO-friendly
- Builds authority
- Example: "Complete Guide to Web Security"

Videos:
- Engaging
- High retention
- Demonstrations
- Example: Product tours, tutorials

Infographics:
- Visual data
- Shareable
- Simplifies complex info
- Example: "Web Performance Statistics"

Case Studies:
- Prove value
- Build trust
- Show results
- Example: "How Company X increased conversions by 40%"

Ebooks/Whitepapers:
- In-depth resources
- Lead generation
- Establishes expertise
- Example: "The Complete SEO Guide"

Podcasts:
- Audio content
- Interview format
- Thought leadership
- Example: "Web Development Trends 2024"

Webinars:
- Live presentations
- Interactive
- Lead generation
- Example: "Live demo of our new feature"
```

**3. Content Creation Best Practices**:
```
Quality over Quantity:
- One great post > five mediocre posts
- Research thoroughly
- Provide unique value
- Back up claims with data

SEO Optimization:
- Keyword research
- Optimize titles and headers
- Meta descriptions
- Internal linking
- Alt text for images

Readability:
- Short paragraphs (2-3 sentences)
- Subheadings every 200-300 words
- Bullet points and lists
- Images and visual breaks
- Simple language

Call-to-Action (CTA):
- Every piece needs a CTA
- Clear next step
- Example: "Try for free", "Read more", "Subscribe"
```

**4. Content Distribution**:
```
Owned Media:
- Your blog
- Email newsletter
- Your social profiles

Earned Media:
- Press coverage
- Guest posts
- Mentions and shares
- Reviews

Paid Media:
- Sponsored content
- Social media ads
- Google Ads
- Influencer partnerships
```

### 3. Usage Analysis

**Web Analytics**:

**Google Analytics Setup**:
```html
<!-- Add to <head> of every page -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

**Key Metrics to Track**:

**1. Acquisition Metrics**:
```
Traffic Sources:
- Organic Search: From Google, Bing, etc.
- Direct: Typed URL or bookmarked
- Referral: From other websites
- Social: From social media
- Email: From email campaigns
- Paid: From ads

Metrics:
- Sessions: Number of visits
- Users: Unique visitors
- New vs Returning: User type
- Bounce Rate: Single-page sessions (%)
- Pages/Session: Average pages viewed
```

**2. Behavior Metrics**:
```
Page Performance:
- Pageviews: Total page loads
- Unique Pageviews: Deduplicated
- Avg. Time on Page: Engagement indicator
- Exit Rate: % leaving from this page

Top Pages:
- Most viewed pages
- Landing pages (entry points)
- Exit pages (where users leave)

Site Search:
- Search terms used
- Results clicked
- Refinements made
```

**3. Conversion Metrics**:
```
Goals:
- Sign-ups
- Purchases
- Downloads
- Contact form submissions

Conversion Rate:
(Conversions / Total Visitors) × 100

Example:
100 purchases / 10,000 visitors = 1% conversion rate

Funnel Analysis:
Step 1: Product page → 1000 users
Step 2: Add to cart → 300 users (30%)
Step 3: Checkout → 150 users (50% of step 2)
Step 4: Payment → 100 users (67% of step 3)

Drop-off: Identify where users leave
```

**4. User Flow**:
```
Visualize user paths:
Homepage → Products → Product Detail → Cart → Checkout

Identify:
- Common paths
- Unexpected paths
- Dead ends
- Optimization opportunities
```

**Event Tracking**:
```javascript
// Track custom events
gtag('event', 'button_click', {
  'event_category': 'engagement',
  'event_label': 'cta_button',
  'value': 1
});

// Track downloads
gtag('event', 'file_download', {
  'event_category': 'downloads',
  'event_label': 'ebook.pdf'
});

// Track video plays
gtag('event', 'video_play', {
  'event_category': 'video',
  'event_label': 'tutorial_video',
  'value': 60 // seconds
});

// Track scroll depth
let scrollDepth = 0;
window.addEventListener('scroll', () => {
  let depth = Math.round((window.scrollY / document.body.scrollHeight) * 100);
  if (depth > scrollDepth && depth % 25 === 0) {
    scrollDepth = depth;
    gtag('event', 'scroll_depth', {
      'event_category': 'engagement',
      'event_label': `${depth}%`,
      'value': depth
    });
  }
});
```

**A/B Testing**:
```javascript
// Example: Test two different headlines

// Randomly assign variant
const variant = Math.random() < 0.5 ? 'A' : 'B';

if (variant === 'A') {
  document.getElementById('headline').textContent = 
    'Get Started with Our Amazing App';
} else {
  document.getElementById('headline').textContent = 
    'Transform Your Business Today';
}

// Track which variant user saw
gtag('event', 'ab_test_view', {
  'event_category': 'experiment',
  'event_label': 'headline_test',
  'variant': variant
});

// Track conversions by variant
document.getElementById('signup-btn').addEventListener('click', () => {
  gtag('event', 'ab_test_conversion', {
    'event_category': 'experiment',
    'event_label': 'headline_test',
    'variant': variant
  });
});
```

**Heatmaps and Session Recordings**:
```
Tools:
- Hotjar
- Crazy Egg
- Microsoft Clarity (free)
- Mouseflow

Insights:
- Where users click
- How far they scroll
- What they ignore
- Mobile vs desktop behavior
- Rage clicks (frustration)
- Dead clicks (nothing happens)
```

### 4. Web Project Management

**Project Management Methodologies**:

**1. Waterfall** (Traditional):
```
Requirements → Design → Implementation → Testing → Deployment → Maintenance

Characteristics:
- Sequential phases
- Each phase completes before next
- Detailed upfront planning
- Less flexible to changes

When to Use:
- Fixed requirements
- Regulatory projects
- Clear, stable scope
```

**2. Agile** (Iterative):
```
Sprint 1: Plan → Develop → Test → Review → Deploy
Sprint 2: Plan → Develop → Test → Review → Deploy
Sprint 3: Plan → Develop → Test → Review → Deploy
...

Characteristics:
- Iterative development
- 2-4 week sprints
- Continuous feedback
- Adaptive to changes
- Daily standups

When to Use:
- Evolving requirements
- Need quick releases
- User feedback important
- Most web projects
```

**Agile Ceremonies**:
```
Sprint Planning (Start of sprint):
- What to build this sprint?
- How to build it?
- Commit to sprint goal

Daily Standup (Every day, 15 min):
- What did I do yesterday?
- What will I do today?
- Any blockers?

Sprint Review (End of sprint):
- Demo completed work
- Gather feedback
- Update product backlog

Sprint Retrospective (After review):
- What went well?
- What didn't go well?
- What to improve next sprint?
```

**Project Management Tools**:
```
Jira:
- Issue tracking
- Sprint planning
- Agile boards
- Reporting

Trello:
- Kanban boards
- Simple and visual
- Task management
- Team collaboration

Asana:
- Task and project management
- Timeline view
- Workload management
- Team communication

Monday.com:
- Work OS platform
- Customizable workflows
- Automations
- Integrations

Linear:
- Modern issue tracking
- Fast interface
- Developer-focused
- Keyboard shortcuts
```

**Project Tracking Example (Jira)**:
```
Epic: User Authentication
  ↓
Story: As a user, I want to sign up with email
  - Task: Design signup form
  - Task: Implement frontend form
  - Task: Create API endpoint
  - Task: Set up email verification
  - Task: Write tests
  - Task: Deploy to staging

Story Points: 8
Priority: High
Sprint: Sprint 15
Assignee: @developer1
Status: In Progress
```

**Risk Management**:
```
Identify Risks:
1. Technical Risks:
   - New technology learning curve
   - Integration challenges
   - Performance issues
   - Security vulnerabilities

2. Resource Risks:
   - Key person unavailable
   - Budget constraints
   - Time constraints
   - Skill gaps

3. External Risks:
   - Vendor dependencies
   - Regulatory changes
   - Market changes
   - Third-party API changes

Risk Matrix:
Impact →  Low    Medium    High
 ↓
Low     [Green] [Yellow] [Yellow]
Medium  [Yellow][Yellow] [Red]
High    [Yellow][Red]    [Red]

Mitigation Strategies:
- Avoid: Change plan to eliminate risk
- Transfer: Insurance, outsource
- Mitigate: Reduce likelihood or impact
- Accept: Acknowledge and monitor
```

### 5. Managing Quality

**Quality Assurance Process**:

**Code Quality**:
```
Code Reviews:
- Peer review before merge
- Check for best practices
- Identify bugs early
- Knowledge sharing

Code Standards:
- Linting (ESLint, Prettier)
- Naming conventions
- Documentation requirements
- Consistent formatting

Static Analysis:
- SonarQube: Code quality metrics
- CodeClimate: Automated code review
- Security scanning
- Technical debt tracking
```

**Testing Strategy**: