# EventHub API - Bruno Collection

A comprehensive API testing suite for the EventHub platform built with Bruno, featuring contract-first validation using Zod schemas, automated testing, and detailed reporting.

[Allure Report](https://tadeu-dutra.github.io/eventhub-api-bruno-collection/)


## 🎯 Overview

This Bruno collection provides production-ready API testing for the EventHub platform with:

- **🔒 Contract-First Testing**: Zod schema validation for all requests and responses
- **🚀 CI/CD Integration**: GitHub Actions workflow with automated test execution and Allure reporting
- **📊 Comprehensive Coverage**: Complete CRUD operations for events, bookings, and user management
- **🔧 Dynamic Testing**: Automated data generation and environment variable management
- **📈 Interactive Reports**: Allure-based test reporting with detailed metrics and visualizations

## 🛠️ Technology Stack

- **Testing Framework**: [Bruno](https://www.usebruno.com/) - Modern API testing platform
- **Schema Validation**: [Zod](https://zod.dev/) v4.4.3 - TypeScript-first schema validation
- **CI/CD**: GitHub Actions with automated testing
- **Reporting**: Allure Test Reports with detailed visualizations
- **Environment**: Node.js 22+ with Bruno CLI

## 📋 Prerequisites

1. **Install Bruno** - Download and install [Bruno](https://www.usebruno.com/)
2. **Node.js** - Version 22 or higher (for CLI usage)
3. **Git** - For cloning and version control

## 🚀 Quick Start

### Interactive Testing (Bruno UI)

1. **Clone the repository**:
   ```bash
   git clone https://github.com/tadeu-dutra/eventhub-api-bruno-collection.git
   cd eventhub-api-bruno-collection
   ```

2. **Open in Bruno**:
   - Launch Bruno application
   - Click "Open Collection" 
   - Select the cloned folder

3. **Run Tests**:
   - Individual requests: Click the request and hit "Send"
   - Full collection: Right-click collection → "Run Collection"
   - View results in the built-in test panel

### CLI Testing (Automated)

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Install Bruno CLI**:
   ```bash
   npm install -g @usebruno/cli@3.5.0
   ```

3. **Run the test suite**:
   ```bash
   bru run --env development --sandbox developer
   ```

4. **Generate reports**:
   ```bash
   # Install Allure CLI
   npm install -g allure-commandline
   
   # Generate HTML report
   mkdir -p allure-results
   cp junit.xml allure-results/ 2>/dev/null || echo "No JUnit file found"
   allure generate allure-results --clean -o allure-report
   
   # View report
   allure open allure-report
   ```

## 📁 Project Structure

```
EventHub API/
├── 📂 Auth/                           # Authentication endpoints
│   ├── 📄 Register a new user [Zod- AuthInput + AuthResponse].yml
│   ├── 📄 Log in with existing credentials.yml
│   ├── 📄 Get the currently authenticated user [Zod- MeResponse].yml
│   └── 📄 folder.yml
├── 📂 Events/                         # Event management CRUD
│   ├── 📄 Create a new event [Zod- CreateEventInput].yml
│   ├── 📄 List all events.yml
│   ├── 📄 Get a single event by ID [Zod- Event].yml
│   ├── 📄 Update an event.yml
│   └── 📄 folder.yml
├── 📂 Bookings/                       # Booking system
│   ├── 📄 Create a booking (buy tickets) [Zod- CreateBookingInput].yml
│   ├── 📄 List all bookings.yml
│   ├── 📄 Get a single booking by ID [Zod- Booking].yml
│   ├── 📄 Look up a booking by reference code.yml
│   └── 📄 folder.yml
├── 📂 Config/                         # Configuration endpoints
│   ├── 📄 Get public feature flags.yml
│   └── 📄 folder.yml
├── 📂 Health/                         # System health
│   ├── 📄 API health check.yml
│   └── 📄 folder.yml
├── 📂 Teardown/                       # Cleanup operations
│   ├── 📄 Cancel a booking.yml
│   ├── 📄 Delete an event.yml
│   └── 📄 folder.yml
├── 📂 environments/                   # Environment configurations
│   └── 📄 development.yml
├── 📂 .github/workflows/              # CI/CD pipeline
│   └── 📄 ci.yaml
├── 📄 package.json                    # Node.js dependencies
├── 📄 opencollection.yml              # Bruno collection metadata
└── 📄 README.md                       # This documentation
```

## 🌐 API Endpoints

### Authentication Flow
| Endpoint | Method | Purpose | Schema Validation |
|----------|--------|---------|-------------------|
| `/auth/register` | `POST` | Create new user account | ✅ AuthInput ➜ AuthResponse |
| `/auth/login` | `POST` | Authenticate user | ✅ AuthInput ➜ AuthResponse |
| `/auth/me` | `GET` | Get current user profile | ✅ MeResponse |

### Event Management
| Endpoint | Method | Purpose | Schema Validation |
|----------|--------|---------|-------------------|
| `/events` | `POST` | Create new event | ✅ CreateEventInput |
| `/events` | `GET` | List all events (paginated) | - |
| `/events/{id}` | `GET` | Get event by ID | ✅ Event |
| `/events/{id}` | `PUT` | Update event details | - |

### Booking System
| Endpoint | Method | Purpose | Schema Validation |
|----------|--------|---------|-------------------|
| `/bookings` | `POST` | Purchase tickets/create booking | ✅ CreateBookingInput |
| `/bookings` | `GET` | List all user bookings | - |
| `/bookings/{id}` | `GET` | Get booking details by ID | ✅ Booking |
| `/bookings/by-reference/{referenceCode}` | `GET` | Find booking by reference code | - |
| `/bookings/{id}` | `DELETE` | Cancel booking | - |

### Configuration & Health
| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/config/feature-flags` | `GET` | Get public feature flags |
| `/health` | `GET` | API health check |

## 🔐 Authentication System

The collection implements JWT Bearer token authentication with automated token management:

1. **User Registration**: Creates new user with email/password validation
2. **Login Flow**: Exchanges credentials for JWT token
3. **Token Storage**: Automatically stores token in `TOKEN` environment variable
4. **Authorization**: All protected endpoints include `Authorization: Bearer {{TOKEN}}` header
5. **Token Refresh**: Re-login when token expires

### Authentication Variables
| Variable | Source | Lifetime |
|----------|--------|----------|
| `TOKEN` | Generated from login response | Session |
| `EMAIL` | From registration response | Session |
| `USER_ID` | Extracted from user profile | Session |

## 🧪 Contract Validation

This collection enforces API contracts using Zod schemas for both input validation and response verification.

### Input Schemas

#### AuthInput Schema
```typescript
{
  email: string.email("Invalid email format"),
  password: string.min(6, "Password must be at least 6 characters")
}
```

#### CreateEventInput Schema
```typescript
{
  title: string.min(1, "Title is required"),
  description: string.optional().default(""),
  category: string,
  venue: string,
  city: string.min(1, "City is required"),
  eventDate: string.datetime("Event date must be valid ISO datetime"),
  price: number | string,
  totalSeats: number | string,
  imageUrl: string.url("Image URL must be valid URL").optional().nullable()
}
```

#### CreateBookingInput Schema
```typescript
{
  eventId: number.int().positive(),
  customerName: string.min(2, "Name must be at least 2 characters"),
  customerEmail: string.email("Valid email required"),
  customerPhone: string.min(10, "Phone must be at least 10 characters"),
  quantity: number.int().min(1).max(10, "Max 10 tickets per order")
}
```

### Response Schemas

#### AuthResponse Schema
```typescript
{
  success: literal(true),
  token: string,
  user: {
    id: number.int().positive(),
    email: string.email()
  }
}
```

#### Event Schema
```typescript
{
  id: number,
  title: string,
  description: string,
  category: string,
  venue: string,
  city: string,
  eventDate: string,
  price: number,
  totalSeats: number,
  availableSeats: number,
  imageUrl: string | null,
  createdAt: string,
  updatedAt: string
}
```

## 🔧 Environment Variables

| Variable | Type | Description | Source |
|----------|------|-------------|---------|
| `BASE_URL` | String | API base endpoint | development.yml |
| `TOKEN` | String | JWT authentication token | Auto-generated |
| `EMAIL` | String | Registered user email | Auto-generated |
| `USER_ID` | Number | Current user ID | Auto-generated |
| `EVENT_ID` | Number | Current event ID | Auto-generated |
| `BOOKING_ID` | Number | Current booking ID | Auto-generated |
| `BOOKING_REF` | String | Current booking reference | Auto-generated |
| `FAKE_QUANTITY` | Number | Random ticket quantity | Auto-generated |
| `CUSTOM_PRICE` | Number | Random event price | Auto-generated |
| `CUSTOM_SEATS` | Number | Random event capacity | Auto-generated |

## 🧩 Dynamic Data Generation

The collection uses Bruno's built-in functions and custom scripts for realistic test data:

### Bruno Functions
- `{{$randomEmail}}` - Generates random email addresses
- `{{$randomFullName}}` - Creates realistic person names
- `{{$randomPhoneNumber}}` - Generates phone numbers
- `{{$randomCatchPhrase}}` - Business-sounding phrases
- `{{$randomWords}}` - Random text content
- `{{$randomCompanyName}}` - Company names
- `{{$randomCity}}` - City names
- `{{$randomDateFuture}}` - Future dates
- `{{$randomImageUrl}}` - Placeholder image URLs

### Custom JavaScript Generation
```javascript
// Random pricing (₹500-₹5000)
const randomPrice = Math.floor(Math.random() * (5000 - 100 + 1)) + 500;

// Random capacity (50-1000 seats)
const randomSeats = Math.floor(Math.random() * (1000 - 50 + 1)) + 50;

// Random ticket quantity (1-5)
const randomQuantity = Math.floor(Math.random() * 5) + 1;
```

## 🔄 Test Execution Workflow

### Recommended Test Sequence

1. **Authentication Setup**
   - Run `Auth → Register a new user`
   - Run `Auth → Log in with existing credentials`
   - Verify `TOKEN` is set

2. **Event Management**
   - Run `Events → Create a new event`
   - Run `Events → List all events`
   - Run `Events → Get a single event by ID`
   - Optionally: `Events → Update an event`

3. **Booking Operations**
   - Run `Bookings → Create a booking`
   - Run `Bookings → List all bookings`
   - Run `Bookings → Get a single booking by ID`
   - Run `Bookings → Look up a booking by reference code`

4. **Cleanup** (Optional)
   - Run `Teardown → Cancel a booking`
   - Run `Teardown → Delete an event`

### Running Tests in Bruno UI

1. **Individual Tests**: Click any request and hit "Send"
2. **Folder Tests**: Right-click folder → "Run"
3. **Full Collection**: Right-click collection root → "Run Collection"
4. **View Results**: Test results appear in the bottom panel

### Running Tests via CLI

```bash
# Basic execution
bru run --env development

# With reporting
bru run --env development --reporter-junit junit.xml --reporter-json results.json

# With sandbox security
bru run --env development --sandbox developer

# Run specific folder
bru run Events --env development

# Verbose output
bru run --env development --verbose
```

## 📊 Test Reporting & Analytics

### Allure Test Reports

This project generates comprehensive Allure reports with:

- **📈 Test Execution Statistics**: Pass/fail rates, duration analysis
- **🔍 Detailed Test Results**: Step-by-step execution logs
- **📋 Test Categories**: Grouped by feature area (Auth, Events, Bookings)
- **🕐 Timeline View**: Chronological execution timeline
- **📝 Error Analysis**: Detailed failure information with screenshots
- **🎯 Coverage Metrics**: API endpoint coverage analysis

### Report Formats Available

| Format | Purpose | Location |
|--------|---------|----------|
| **Allure HTML** | Interactive visual dashboard | `allure-report/index.html` |
| **JUnit XML** | CI/CD integration | `junit.xml` |
| **JSON Raw** | Machine-readable results | `results.json` |

### Viewing Reports

**Local Development**:
```bash
# Generate and view Allure report
allure generate allure-results --clean -o allure-report
allure open allure-report

# Or serve statically
python -m http.server 8080 -d allure-report
```

**CI/CD Integration**:
Reports are automatically deployed to GitHub Pages on every push/PR to the `gh-pages` branch.

### GitHub Actions CI/CD

The automated pipeline provides:

- **🚀 Automated Testing**: Runs on every push and PR
- **📊 Allure Report Generation**: Automatic HTML report creation
- **🌐 GitHub Pages Deployment**: Reports published to `gh-pages` branch
- **📦 Artifact Storage**: Test results saved as GitHub artifacts
- **☑️ Multi-Environment**: Supports development, staging, production

**CI/CD Workflow**:
1. Code pushed → GitHub Action triggered
2. Bruno CLI executes all tests
3. Allure generates interactive reports
4. Reports deployed to GitHub Pages
5. Test results saved as artifacts

### Accessing CI/CD Reports

1. **GitHub Pages**: View reports at `https://{username}.github.io/{repo}/`
2. **GitHub Artifacts**: Download from Actions tab in GitHub
3. **Pull Request Checks**: Automatic comment with test summary

## 🎯 Test Scenarios

### Authentication Tests
- ✅ User registration with valid data
- ✅ User registration with invalid email format
- ✅ User registration with short password
- ✅ Successful login with valid credentials
- ✅ Login with incorrect password
- ✅ Login with non-existent email
- ✅ Token validation and user profile retrieval

### Event Management Tests
- ✅ Event creation with all required fields
- ✅ Event creation with invalid date format
- ✅ Event creation with invalid pricing
- ✅ Event listing with pagination
- ✅ Event retrieval by valid ID
- ✅ Event retrieval with non-existent ID
- ✅ Event update with valid data
- ✅ Event update validation

### Booking System Tests
- ✅ Booking creation with valid event ID
- ✅ Booking creation with invalid quantity
- ✅ Booking creation with invalid event ID
- ✅ Booking listing for authenticated user
- ✅ Booking retrieval by ID
- ✅ Booking lookup by reference code
- ✅ Booking cancellation
- ✅ Multiple bookings for same event

### Health & Config Tests
- ✅ API health check availability
- ✅ Feature flags retrieval
- ✅ API response time validation

## 🐛 Troubleshooting

### Common Issues & Solutions

#### 1. Authentication Failures
**Problem**: 401 Unauthorized errors
```bash
# Solution: Re-run authentication flow
bru run "Auth/Register a new user" --env development
bru run "Auth/Log in with existing credentials" --env development
```

#### 2. Validation Errors
**Problem**: Zod schema validation failures
```bash
# Enable debug logging in Bruno UI:
# Settings → Developer → Console Logging

# Check detailed error messages in console
# Review schema requirements in test scripts
```

#### 3. Environment Issues
**Problem**: BASE_URL not accessible
```bash
# Verify API availability
curl -I https://api.eventhub.rahulshettyacademy.com/api/health

# Update environment if needed
# environments/development.yml
```

#### 4. CLI Test Failures
**Problem**: Bruno CLI not found
```bash
# Reinstall globally
npm uninstall -g @usebruno/cli
npm install -g @usebruno/cli@3.5.0

# Verify installation
bru --version
```

#### 5. Report Generation Issues
**Problem**: Allure report not generating
```bash
# Install Java (required for Allure)
# Windows: Use WSL or install JDK 17+
# macOS: brew install openjdk@17
# Linux: apt install openjdk-17-jdk

# Reinstall Allure
npm uninstall -g allure-commandline
npm install -g allure-commandline
```

### Debug Mode Configuration

Enable comprehensive debugging:

```yaml
# In Bruno request settings
runtime:
  scripts:
    - type: tests
      code: |
        // Enable console logging
        console.log('Request body:', JSON.stringify(req.getBody(), null, 2));
        console.log('Response body:', JSON.stringify(res.getBody(), null, 2));
        console.log('Status:', res.getStatus());
```

### Performance Optimization

For large test suites:

```bash
# Run specific folders
bru run Events --env development
bru run Bookings --env development

# Parallel execution (if supported)
bru run --parallel --env development

# Timeout configuration
bru run --timeout 30000 --env development
```

## 🚀 Advanced Usage

### Custom Environment Setup

Create new environment files for different stages:

```yaml
# environments/staging.yml
name: staging
variables:
  - name: BASE_URL
    value: https://staging-api.eventhub.example.com/api

# environments/production.yml  
name: production
variables:
  - name: BASE_URL
    value: https://api.eventhub.example.com/api
```

### Custom Test Data

Modify dynamic generation scripts:

```javascript
// Custom pricing ranges
const basePrice = Math.floor(Math.random() * (10000 - 1000 + 1)) + 1000;
const vipMultiplier = 2.5;
const regularMultiplier = 1.0;

// Custom seat allocations
const VIP_SEATS = Math.floor(Math.random() * 100) + 50;
const REGULAR_SEATS = Math.floor(Math.random() * 500) + 200;
```

### Integration with External Tools

**Hook into CI/CD pipelines**:
```bash
# Jenkins example
stage('API Testing') {
    steps {
        sh 'npm ci'
        sh 'bru run --env production --reporter-junit junit.xml'
        junit 'junit.xml'
        publishHTML([
            allowMissing: false,
            alwaysLinkToLastBuild: true,
            keepAll: true,
            reportDir: 'allure-report',
            reportFiles: 'index.html',
            reportName: 'API Test Report'
        ])
    }
}
```

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

### Development Workflow

1. **Fork the repository**
2. **Create feature branch**: `git checkout -b feature/new-endpoint`
3. **Add new tests**: Follow existing patterns with Zod validation
4. **Update documentation**: Include new endpoints in README
5. **Run tests locally**: Ensure all tests pass
6. **Submit PR**: With clear description of changes

### Code Standards

- **Naming**: Use descriptive test names with `[Zod: Schema]` suffix
- **Validation**: Always include schema validation for new endpoints
- **Documentation**: Update README and inline comments
- **Testing**: Add both positive and negative test cases
- **Error Handling**: Include meaningful error messages

### Pull Request Requirements

- [ ] All existing tests pass
- [ ] New tests include schema validation
- [ ] README updated with new endpoints
- [ ] Code follows existing patterns
- [ ] No breaking changes to existing functionality

## 📄 License

This project is licensed under the ISC License - see the package.json file for details.

## 📞 Support & Links

- **Bruno Documentation**: [https://docs.usebruno.com/](https://docs.usebruno.com/)
- **Zod Documentation**: [https://zod.dev/](https://zod.dev/)
- **Allure Reporting**: [https://allurereport.org/](https://allurereport.org/)
- **GitHub Repository**: [https://github.com/tadeu-dutra/eventhub-api-bruno-collection](https://github.com/tadeu-dutra/eventhub-api-bruno-collection)
- **API Base URL**: `https://api.eventhub.rahulshettyacademy.com/api`

---

**🎉 Happy Testing!** This collection provides everything you need for comprehensive API testing of the EventHub platform with professional-grade validation and reporting.
