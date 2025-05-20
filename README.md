# Jobs API

A RESTful API for job listings, built with Node.js, Express, and MongoDB. This API is designed for the Sensai career coach platform where jobs are recommended using specific algorithms.

## Features

- User authentication (register, login)
- Complete CRUD operations for job listings
- Advanced filtering and searching
- Secure API with JWT authentication
- Error handling and validation
- Production-ready folder structure

## Tech Stack

- Node.js
- Express.js
- MongoDB with Mongoose
- JWT for authentication
- Express Rate Limiter for security
- Helmet for securing HTTP headers
- XSS-Clean for sanitizing user input

## Getting Started

### Prerequisites

- Node.js (v18 or later)
- MongoDB

### Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/yourusername/jobs-api.git
   cd jobs-api
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Set up environment variables:

   - Create a `.env` file in the root directory
   - Copy the content from `.env.example` and add your values:

   ```
   PORT=3000
   NODE_ENV=development
   MONGO_URI=mongodb+srv://<username>:<password>@cluster0.example.mongodb.net/jobs-api?retryWrites=true&w=majority
   JWT_SECRET=your_jwt_secret_key_here
   JWT_LIFETIME=30d
   ```

4. Start the server:

   ```bash
   # Development mode
   npm run dev

   # Production mode
   npm start
   ```

## API Endpoints

### Authentication

| Method | Endpoint              | Description         |
| ------ | --------------------- | ------------------- |
| POST   | /api/v1/auth/register | Register a new user |
| POST   | /api/v1/auth/login    | Login a user        |

### Jobs

| Method | Endpoint         | Description                 |
| ------ | ---------------- | --------------------------- |
| POST   | /api/v1/jobs     | Create a new job            |
| GET    | /api/v1/jobs     | Get all jobs with filtering |
| GET    | /api/v1/jobs/:id | Get a specific job          |
| PATCH  | /api/v1/jobs/:id | Update a job                |
| DELETE | /api/v1/jobs/:id | Delete a job                |

## API Documentation

### Authentication Endpoints

#### Register User

```
POST /api/v1/auth/register
```

Request body:

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

Response:

```json
{
  "user": {
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Login User

```
POST /api/v1/auth/login
```

Request body:

```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

Response:

```json
{
  "user": {
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Jobs Endpoints

#### Create Job

```
POST /api/v1/jobs
```

Headers:

```
Authorization: Bearer <your_token>
```

Request body:

```json
{
  "company": "Tech Corp",
  "position": "Frontend Developer",
  "status": "pending",
  "jobType": "full-time",
  "jobLocation": "New York",
  "description": "We are looking for a skilled frontend developer...",
  "requirements": "3+ years of experience with React...",
  "salary": "$80,000 - $100,000",
  "contactEmail": "hr@techcorp.com"
}
```

Response:

```json
{
  "job": {
    "_id": "60f1a5b3e89c9c3e8c6b7a1b",
    "company": "Tech Corp",
    "position": "Frontend Developer",
    "status": "pending",
    "jobType": "full-time",
    "jobLocation": "New York",
    "description": "We are looking for a skilled frontend developer...",
    "requirements": "3+ years of experience with React...",
    "salary": "$80,000 - $100,000",
    "contactEmail": "hr@techcorp.com",
    "createdBy": "60f1a5b3e89c9c3e8c6b7a1a",
    "createdAt": "2023-12-15T10:30:15.123Z",
    "updatedAt": "2023-12-15T10:30:15.123Z"
  }
}
```

#### Get All Jobs

```
GET /api/v1/jobs
```

Headers:

```
Authorization: Bearer <your_token>
```

Query parameters:

- `search`: Search term for position, company, or description
- `status`: Filter by status (pending, interview, declined)
- `jobType`: Filter by job type (full-time, part-time, remote, internship)
- `sort`: Sort results (latest, oldest, a-z, z-a)
- `page`: Page number for pagination
- `limit`: Number of jobs per page

Response:

```json
{
  "jobs": [
    {
      "_id": "60f1a5b3e89c9c3e8c6b7a1b",
      "company": "Tech Corp",
      "position": "Frontend Developer",
      "status": "pending",
      "jobType": "full-time",
      "jobLocation": "New York",
      "description": "We are looking for a skilled frontend developer...",
      "requirements": "3+ years of experience with React...",
      "salary": "$80,000 - $100,000",
      "contactEmail": "hr@techcorp.com",
      "createdBy": "60f1a5b3e89c9c3e8c6b7a1a",
      "createdAt": "2023-12-15T10:30:15.123Z",
      "updatedAt": "2023-12-15T10:30:15.123Z"
    }
    // More jobs...
  ],
  "totalJobs": 25,
  "numOfPages": 3
}
```

#### Get Single Job

```
GET /api/v1/jobs/:id
```

Headers:

```
Authorization: Bearer <your_token>
```

Response:

```json
{
  "job": {
    "_id": "60f1a5b3e89c9c3e8c6b7a1b",
    "company": "Tech Corp",
    "position": "Frontend Developer",
    "status": "pending",
    "jobType": "full-time",
    "jobLocation": "New York",
    "description": "We are looking for a skilled frontend developer...",
    "requirements": "3+ years of experience with React...",
    "salary": "$80,000 - $100,000",
    "contactEmail": "hr@techcorp.com",
    "createdBy": "60f1a5b3e89c9c3e8c6b7a1a",
    "createdAt": "2023-12-15T10:30:15.123Z",
    "updatedAt": "2023-12-15T10:30:15.123Z"
  }
}
```

#### Update Job

```
PATCH /api/v1/jobs/:id
```

Headers:

```
Authorization: Bearer <your_token>
```

Request body:

```json
{
  "company": "Tech Corp Updated",
  "position": "Senior Frontend Developer",
  "status": "interview"
}
```

Response:

```json
{
  "job": {
    "_id": "60f1a5b3e89c9c3e8c6b7a1b",
    "company": "Tech Corp Updated",
    "position": "Senior Frontend Developer",
    "status": "interview",
    "jobType": "full-time",
    "jobLocation": "New York",
    "description": "We are looking for a skilled frontend developer...",
    "requirements": "3+ years of experience with React...",
    "salary": "$80,000 - $100,000",
    "contactEmail": "hr@techcorp.com",
    "createdBy": "60f1a5b3e89c9c3e8c6b7a1a",
    "createdAt": "2023-12-15T10:30:15.123Z",
    "updatedAt": "2023-12-15T11:15:30.456Z"
  }
}
```

#### Delete Job

```
DELETE /api/v1/jobs/:id
```

Headers:

```
Authorization: Bearer <your_token>
```

Response:

```json
{
  "msg": "Success! Job removed"
}
```

## Database Schema

### User Schema

```javascript
{
  name: {
    type: String,
    required: true,
    minlength: 3,
    maxlength: 50
  },
  email: {
    type: String,
    required: true,
    match: [/email regex/],
    unique: true
  },
  password: {
    type: String,
    required: true,
    minlength: 6
  },
  role: {
    type: String,
    enum: ['admin', 'user'],
    default: 'user'
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}
```

### Job Schema

```javascript
{
  company: {
    type: String,
    required: true,
    maxlength: 50
  },
  position: {
    type: String,
    required: true,
    maxlength: 100
  },
  status: {
    type: String,
    enum: ['interview', 'declined', 'pending'],
    default: 'pending'
  },
  jobType: {
    type: String,
    enum: ['full-time', 'part-time', 'remote', 'internship'],
    default: 'full-time'
  },
  jobLocation: {
    type: String,
    required: true,
    default: 'remote'
  },
  description: {
    type: String,
    required: true,
    maxlength: 1000
  },
  requirements: {
    type: String,
    maxlength: 1000
  },
  salary: {
    type: String
  },
  contactEmail: {
    type: String,
    match: [/email regex/]
  },
  createdBy: {
    type: ObjectId,
    ref: 'User',
    required: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}
```

## Deployment

The API can be deployed to free platforms like:

### 1. Render

1. Create an account at [render.com](https://render.com/)
2. Create a new Web Service
3. Connect your GitHub repo
4. Set the build command: `npm install`
5. Set the start command: `npm start`
6. Add your environment variables in the dashboard
7. Deploy!

### 2. Railway

1. Sign up at [railway.app](https://railway.app/)
2. Create a new project
3. Add a MongoDB database
4. Deploy your GitHub repository
5. Add your environment variables
6. Deploy!

## License

This project is licensed under the MIT License.
