# Subscription Tracker API

A comprehensive Node.js REST API for managing and tracking personal subscriptions with automated renewal reminders.

## Features

- **User Management**: User registration, authentication, and profile management
- **Subscription Tracking**: Create, read, update, and delete subscriptions
- **Automated Reminders**: Workflow-based email notifications for upcoming renewals
- **Security**: JWT authentication, Arcjet protection, and input validation
- **Database**: MongoDB with Mongoose ODM
- **Email Integration**: Nodemailer for sending subscription reminders

## Tech Stack

- **Runtime**: Node.js with ES6 modules
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose
- **Authentication**: JSON Web Tokens (JWT)
- **Security**: Arcjet for rate limiting and protection
- **Email**: Nodemailer
- **Workflow**: Upstash Workflow for automated tasks
- **Password Hashing**: bcryptjs
- **Development**: ESLint, Nodemon

## Project Structure

```
subscription-tracker/
├── app.js                          # Main application entry point
├── package.json                    # Dependencies and scripts
├── eslint.config.js               # ESLint configuration
├── config/
│   ├── env.js                     # Environment variables configuration
│   ├── arcjet.js                  # Arcjet security configuration
│   ├── nodemailer.js              # Email service configuration
│   └── upstash.js                 # Upstash workflow configuration
├── controllers/
│   ├── auth.controller.js         # Authentication logic
│   ├── subscription.controller.js # Subscription management logic
│   ├── user.controller.js         # User management logic
│   └── workflow.controller.js     # Automated workflow logic
├── database/
│   └── mongodb.js                 # MongoDB connection setup
├── middleware/
│   ├── auth.middleware.js         # JWT authentication middleware
│   ├── arcjet.middleware.js       # Security middleware
│   └── error.middleware.js        # Global error handling
├── models/
│   ├── subscription.model.js      # Subscription data model
│   └── user.model.js              # User data model
├── routes/
│   ├── auth.routes.js             # Authentication routes
│   ├── subscription.routes.js     # Subscription routes
│   ├── user.routes.js             # User routes
│   └── workflow.routes.js         # Workflow routes
└── utils/
    ├── email-template.js          # Email template generator
    └── send-email.js              # Email sending utility
```

## API Endpoints

### Authentication
- `POST /api/v1/auth/sign-up` - User registration
- `POST /api/v1/auth/sign-in` - User login
- `POST /api/v1/auth/sign-out` - User logout

### Users
- `GET /api/v1/users` - Get all users
- `GET /api/v1/users/:id` - Get user by ID
- `PUT /api/v1/users/:id` - Update user
- `DELETE /api/v1/users/:id` - Delete user

### Subscriptions
- `GET /api/v1/subscriptions` - Get all subscriptions
- `GET /api/v1/subscriptions/:id` - Get subscription by ID
- `POST /api/v1/subscriptions` - Create new subscription
- `PUT /api/v1/subscriptions/:id` - Update subscription
- `DELETE /api/v1/subscriptions/:id` - Delete subscription
- `GET /api/v1/subscriptions/user/:id` - Get user's subscriptions
- `PUT /api/v1/subscriptions/:id/cancel` - Cancel subscription

### Workflows
- `POST /api/v1/workflows/subscription/reminder` - Send reminder emails

## Data Models

### User Model
- `name` (String, required, 2-50 characters)
- `email` (String, required, unique, valid email format)
- `password` (String, required, hashed)
- `createdAt` (Date, auto-generated)
- `updatedAt` (Date, auto-generated)

### Subscription Model
- `name` (String, required, 2-100 characters)
- `price` (Number, required, minimum 0)
- `currency` (String, enum: USD, EUR, GBP, default: USD)
- `frequency` (String, enum: daily, weekly, monthly, yearly)
- `category` (String, enum: sports, news, entertainment, lifestyle, technology, finance, politics, other)
- `paymentMethod` (String, required)
- `status` (String, enum: active, inactive, default: active)
- `startDate` (Date, required, must be in the past)
- `renewalDate` (Date, must be after start date)
- `description` (String, optional)
- `userId` (ObjectId, reference to User)
- `createdAt` (Date, auto-generated)
- `updatedAt` (Date, auto-generated)

## Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd subscription-tracker
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Set up environment variables**:
   Create environment files based on your environment:
   - `.env.development.local` for development
   - `.env.production.local` for production

   Required environment variables:
   ```env
   PORT=3000
   NODE_ENV=development
   SERVER_URL=http://localhost:3000
   
   # Database
   DB_URI=mongodb://localhost:27017/subscription-tracker
   
   # JWT
   JWT_SECRET=your-jwt-secret-key
   JWT_EXPIRES_IN=7d
   
   # Arcjet Security
   ARCJET_ENV=development
   ARCJET_KEY=your-arcjet-key
   
   # Upstash Workflow
   QSTASH_TOKEN=your-qstash-token
   QSTASH_URL=your-qstash-url
   
   # Email (Nodemailer)
   EMAIL_HOST=smtp.gmail.com
   EMAIL_PORT=587
   EMAIL_USER=your-email@gmail.com
   EMAIL_PASS=your-app-password
   ```

4. **Start the application**:
   ```bash
   # Development mode with auto-reload
   npm run dev
   
   # Production mode
   npm start
   ```

## Development

- **Linting**: Run `npx eslint .` to check code quality
- **Auto-reload**: Use `npm run dev` for development with automatic restart on file changes
- **Database**: Ensure MongoDB is running locally or provide a remote connection string

## Security Features

- **JWT Authentication**: Secure user authentication with JSON Web Tokens
- **Arcjet Protection**: Rate limiting, bot detection, and security monitoring
- **Input Validation**: Mongoose schema validation for all data inputs
- **Password Security**: bcryptjs for password hashing
- **Error Handling**: Comprehensive error middleware for security and debugging

## Email Notifications

The system includes automated email reminders for subscription renewals:
- Configurable email templates
- Workflow-based automation using Upstash
- Support for multiple email providers via Nodemailer

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Support

For support and questions, please open an issue in the repository.
