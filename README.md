# User Authentication & Management API

A full-stack Node.js application for user authentication and management with MongoDB integration. This project implements secure user registration, login, and profile management with JWT-based authentication.

## Project Overview

This application is built as part of an internship project at GenoConnect. It provides a complete backend API for user authentication with features including user registration, login, profile management, and CSV export functionality.

## Features

- **User Registration**: Create new user accounts with encrypted passwords
- **User Login**: Secure authentication with JWT tokens
- **Profile Management**: View and update user profiles
- **User List**: Retrieve all registered users (authenticated route)
- **CSV Export**: Automatically exports user data to CSV file
- **Cookie-based Authentication**: Secure token storage using HTTP-only cookies
- **Password Encryption**: Uses bcryptjs for secure password hashing

## Technologies & Dependencies

### Core Technologies
- **Node.js**: JavaScript runtime
- **Express.js** (v5.2.1): Web application framework
- **MongoDB**: NoSQL database
- **Mongoose** (v9.1.1): MongoDB object modeling

### Authentication & Security
- **jsonwebtoken** (v9.0.3): JWT token generation and verification
- **bcryptjs** (v3.0.3): Password hashing
- **cookie-parser** (v1.4.7): Parse cookie headers
- **dotenv** (v17.2.3): Environment variable management

### Additional Libraries
- **fs** (v0.0.1-security): File system operations for CSV export
- **path** (v0.12.7): File path utilities
- **nodemon** (v3.1.11): Development auto-restart

## Database Connection

### MongoDB Connection
The application connects to MongoDB using Mongoose with the following configuration:

- **Connection File**: `database.js`
- **Connection Method**: Async/await pattern
- **Environment Variable**: `MONGODB_URI` (stored in `.env` file)
- **Features**: 
  - Automatic connection on startup
  - Error handling with process exit on failure
  - Success logging

## Project Structure

```
Intership/
├── database.js          # MongoDB connection configuration
├── login.js             # Main application file with all routes
├── users.js             # User schema definition
├── package.json         # Project dependencies
├── package-lock.json    # Locked dependency versions
├── .gitignore          # Git ignore rules
├── .env                # Environment variables (not tracked)
├── public/             # Static files directory
└── users.csv           # Generated CSV file with user data
```

## API Endpoints

### Public Routes

#### POST /register
Register a new user account
- **Body**: `{ FirstName, LastName, Email, Password }`
- **Returns**: User object with JWT token
- **Creates**: User in database and exports to CSV

#### POST /login
Authenticate existing user
- **Body**: `{ Email, Password }`
- **Returns**: User object with JWT token
- **Sets**: HTTP-only cookie with token (4-day expiry)

### Protected Routes (Requires Authentication)

#### GET /users
Retrieve all registered users
- **Middleware**: preAuth, auth, postAuth
- **Returns**: Array of users (passwords excluded)

#### GET /profile
Get current user's profile
- **Middleware**: auth
- **Returns**: Current user's data

#### PUT /profile
Update current user's profile
- **Middleware**: auth
- **Body**: `{ FirstName?, LastName?, Email?, Password? }`
- **Returns**: Updated user object
- **Note**: Generates new token if email is updated

## Environment Variables

Create a `.env` file in the root directory with:

```
MONGODB_URI= mongodb_connection_string
JWT_SECRET=jwt_secret_key
PORT=3000
```

## Installation & Setup

1. Clone the repository:
```bash
git clone https://github.com/Thesageof69/Intership.git
cd Intership
```

2. Install dependencies:
```bash
npm install
```

3. Create `.env` file with required environment variables

4. Start the server:
```bash
node login.js
```

Or use nodemon for development:
```bash
nodemon login.js
```

## User Schema

The User model (`users.js`) includes:

- **FirstName**: String (required, trimmed)
- **LastName**: String (required, trimmed)
- **Email**: String (required, unique, lowercase, trimmed)
- **Password**: String (required, encrypted)
- **token**: String (JWT token)
- **Timestamps**: Automatically added (createdAt, updatedAt)

## Security Features

1. **Password Encryption**: All passwords are hashed using bcryptjs (10 salt rounds)
2. **JWT Authentication**: Tokens expire after 2 hours
3. **HTTP-only Cookies**: Prevents XSS attacks
4. **Environment Variables**: Sensitive data stored securely
5. **Password Exclusion**: Passwords never returned in API responses

## Development

The application runs on port 3000 by default (configurable via PORT environment variable).

Static files are served from the `public` directory.

## CSV Export Feature

User data is automatically exported to `users.csv` upon registration with the following fields:
- User ID
- First Name
- Last Name
- Email

## Author

- GitHub: [@Thesageof69](https://github.com/Thesageof69)
- Repository: [Intership](https://github.com/Thesageof69/Intership)

## License

This project is part of an internship program at GenoConnect.
