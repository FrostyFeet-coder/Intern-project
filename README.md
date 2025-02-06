E-commerce Authentication API (Signup, Login, JWT)
Overview
This project provides a basic authentication system for an e-commerce platform, including user registration (signup), login, and JWT-based authentication. The API is built using Node.js, Express.js, MongoDB, and JWT for secure token-based authentication.

Features
User Signup: Allows new users to register with their name, email, and password.
User Login: Authenticates users using their email and password.
JWT Authentication: Secure JWT token is provided upon successful login to be used for protected routes.
Password Hashing: User passwords are securely hashed using bcryptjs before being stored in the database.
Role-based Authentication: Admin role can be assigned to users for management functionalities.
Tech Stack
Node.js: JavaScript runtime environment.
Express.js: Web framework for building the API.
MongoDB: NoSQL database to store user data.
JWT: JSON Web Token for secure user authentication.
bcryptjs: For hashing user passwords securely.
Installation & Setup
1. Clone the Repository
bash
Copy
Edit
git clone https://github.com/yourusername/ecommerce-auth-api.git
cd ecommerce-auth-api
2. Install Dependencies
Make sure you have Node.js and npm installed. Then, run:

bash
Copy
Edit
npm install
3. Set up Environment Variables
Create a .env file in the root of your project and add the following:

ini
Copy
Edit
PORT=5000
MONGO_URI=mongodb+srv://your_username:your_password@cluster.mongodb.net/ecommerce
JWT_SECRET=yoursecretkey
4. Connect to MongoDB
Ensure you have a MongoDB database setup. You can use MongoDB Atlas or a local MongoDB instance. Replace your_username, your_password, and your_database in the .env file accordingly.

5. Start the Server
Run the server using nodemon (recommended for development):

bash
Copy
Edit
npx nodemon server.js
The server will start on http://localhost:5000 by default.

API Endpoints
1. POST /api/auth/signup
Description: Registers a new user.
Request Body:
json
Copy
Edit
{
  "name": "Alice",
  "email": "alice@example.com",
  "password": "yourpassword"
}
Response:
json
Copy
Edit
{
  "message": "User registered successfully"
}
2. POST /api/auth/login
Description: Logs in a user and provides a JWT token.
Request Body:
json
Copy
Edit
{
  "email": "alice@example.com",
  "password": "yourpassword"
}
Response:
json
Copy
Edit
{
  "token": "your-jwt-token",
  "user": {
    "id": "user-id",
    "name": "Alice",
    "email": "alice@example.com"
  }
}
3. GET /api/auth/profile (Protected Route)
Description: Retrieves the user's profile data (protected route).
Authorization: Pass the JWT token in the Authorization header as Bearer token.
Response:
json
Copy
Edit
{
  "message": "Welcome user-id",
  "isAdmin": false
}
How Authentication Works
Signup:

When a user signs up, their password is hashed using bcryptjs and stored securely in the MongoDB database.
If the user already exists, a 400 status and a message are returned.
Login:

Upon login, the user is authenticated by comparing the hashed password in the database with the provided password.
If successful, a JWT token is generated with the user's ID and role (isAdmin).
The JWT token is valid for 1 day and must be sent in the Authorization header for subsequent requests to protected routes.
JWT Token:

The token is signed with the JWT_SECRET and contains the user’s id and isAdmin status.
The token expires in 1 day.
Protected Routes:

Routes like /profile are protected and require a valid JWT token to access.
The token is passed via the Authorization header: Bearer token.
Middleware
authMiddleware.js: A middleware that verifies the JWT token from the request header and ensures the user is authenticated before accessing protected routes.
Testing the API
You can test the API using Postman or curl.

Example using curl:
Register a New User:
bash
Copy
Edit
curl -X POST http://localhost:5000/api/auth/signup \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com", "password": "yourpassword"}'
Login:
bash
Copy
Edit
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "alice@example.com", "password": "yourpassword"}'
Access Profile (Protected Route):
bash
Copy
Edit
curl -X GET http://localhost:5000/api/auth/profile \
  -H "Authorization: Bearer your-jwt-token"
