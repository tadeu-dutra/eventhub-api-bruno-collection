# EventHub API - Bruno Collection

A comprehensive Bruno collection for testing the EventHub API with contract validation using Zod schemas.

## 📋 Overview

This Bruno collection provides automated API testing for the EventHub platform, featuring:
- **Contract Validation**: Zod schema validation for requests and responses
- **Authentication Flow**: Complete user registration and login testing
- **Event Management**: CRUD operations for events
- **Booking System**: Ticket purchasing and booking management
- **Automated Testing**: Built-in test assertions and validation

## 🚀 Getting Started

### Prerequisites

1. **Install Bruno** - Download and install [Bruno](https://www.usebruno.com/)
2. **Clone/Download** this collection to your local machine
3. **Environment Setup** - The collection is pre-configured with the base URL

### Running the Collection

1. Open Bruno
2. Click "Open Collection" and select this folder
3. The collection will load with all endpoints organized by feature area
4. Run individual requests or execute the entire collection

## 📁 Collection Structure

```
EventHub API/
├── Auth/                     # Authentication endpoints
│   ├── Register a new user
│   ├── Log in with existing credentials
│   └── Get the currently authenticated user
├── Events/                   # Event management
│   ├── Create a new event
│   ├── List all events
│   ├── Get a single event by ID
│   └── Update an event
├── Bookings/                 # Booking management
│   ├── Create a booking (buy tickets)
│   ├── List all bookings
│   ├── Get a single booking by ID
│   ├── Look up a booking by reference code
│   └── Cancel a booking
├── Config/                   # Configuration
│   └── Get public feature flags
├── Health/                   # API health check
│   └── API health check
└── Teardown/                 # Cleanup operations
    ├── Cancel a booking
    └── Delete an event
```

## 🔐 Authentication

The collection uses JWT Bearer token authentication:

1. **Register**: Create a new user account
2. **Login**: Authenticate and receive a token
3. **Token Storage**: The token is automatically stored in the `TOKEN` variable
4. **Authorization**: All protected endpoints include the Bearer token

## 📊 Available Endpoints

### Authentication
- `POST /auth/register` - Register a new user
- `POST /auth/login` - Login with credentials
- `GET /auth/me` - Get current user info

### Events
- `POST /events` - Create a new event
- `GET /events` - List all events (with pagination)
- `GET /events/{id}` - Get event by ID
- `PUT /events/{id}` - Update event details

### Bookings
- `POST /bookings` - Create booking/purchase tickets
- `GET /bookings` - List all bookings
- `GET /bookings/{id}` - Get booking by ID
- `GET /bookings/by-reference/{referenceCode}` - Find booking by reference
- `DELETE /bookings/{id}` - Cancel booking

### Configuration
- `GET /config/feature-flags` - Get public feature flags

### Health
- `GET /health` - API health check

## ✅ Contract Validation

This collection includes comprehensive Zod schema validation:

### Input Validation
- **AuthInput**: Email format, password minimum length
- **CreateBookingInput**: Event ID validation, customer info, quantity limits
- **EventInput**: Title, description, date validation

### Response Validation
- **AuthResponse**: Token presence, user data structure
- **Booking**: ID, reference code, booking details
- **Event**: Event data structure, required fields

### Test Assertions
Each request includes automated tests for:
- HTTP status codes
- Response structure validation
- Schema contract compliance
- Data type verification
- Business logic validation

## 🛠️ Environment Variables

The collection uses the following variables:

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `BASE_URL` | API base URL | `https://api.eventhub.rahulshettyacademy.com/api` |
| `TOKEN` | JWT authentication token (auto-generated) | - |
| `EVENT_ID` | Current event ID (auto-generated) | - |
| `BOOKING_ID` | Current booking ID (auto-generated) | - |
| `BOOKING_REF` | Current booking reference (auto-generated) | - |
| `FAKE_QUANTITY` | Random ticket quantity for testing | - |

## 📝 Usage Examples

### Basic Workflow

1. **Register a user**: Run `Auth → Register a new user`
2. **Create an event**: Run `Events → Create a new event`
3. **Purchase tickets**: Run `Bookings → Create a booking`
4. **Verify booking**: Run `Bookings → Get a single booking by ID`

### Running Tests

Each request includes built-in tests. To run all tests:
1. Select the collection or folder
2. Click "Run" or use the keyboard shortcut
3. View results in the test panel

### Custom Requests

You can:
- Modify request bodies with your own data
- Adjust environment variables for different API endpoints
- Add new test cases following the existing patterns

## 🔧 Advanced Features

### Dynamic Data Generation
- Random email addresses: `{{$randomEmail}}`
- Random full names: `{{$randomFullName}}`
- Random phone numbers: `{{$randomPhoneNumber}}`
- Random quantities: Generated via JavaScript

### Script Execution
- **Before Request**: Data preparation and validation
- **After Response**: Variable setting and data extraction
- **Tests**: Schema validation and assertions

### Pagination Support
Events endpoint supports paginated responses with:
- Total pages tracking
- Items per page configuration
- Page navigation

## 🐛 Troubleshooting

### Common Issues

1. **Authentication Errors**
   - Ensure you run registration/login first
   - Check if TOKEN variable is set correctly

2. **Validation Failures**
   - Review Zod schema error messages in console
   - Verify request data matches expected contracts

3. **Environment Issues**
   - Confirm BASE_URL is accessible
   - Check network connectivity

### Debug Mode

Enable console logging to see:
- Schema validation details
- Variable values
- Request/response data

## 📚 Technologies & Tools

- **Bruno**: API testing platform
- **Zod**: TypeScript-first schema validation
- **JavaScript**: Dynamic scripting and test logic
- **JSON**: Request/response format

## 🤝 Contributing

To contribute to this collection:

1. **Add new endpoints**: Follow the existing folder structure
2. **Include validations**: Add Zod schemas for contracts
3. **Write tests**: Include comprehensive test assertions
4. **Document**: Update this README with new endpoints

## 📄 License

This collection is provided as-is for API testing and validation purposes.

---

**API Base URL**: `https://api.eventhub.rahulshettyacademy.com/api`  
**Testing Tool**: [Bruno](https://www.usebruno.com/)  
**Schema Validation**: [Zod](https://zod.dev/)