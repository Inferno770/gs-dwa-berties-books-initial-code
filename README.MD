# Berties Books

Sample code for Dynamic Web Applications module.

Uses node.js with Express and EJS.

To install and run:

npm install
node index.js

## Implementation Details

### 1. Database Security & Environment Variables
To secure database credentials, I implemented the **dotenv** module.
- Sensitive information (Database Host, User, Password, Name) is stored in a local `.env` file which is included in `.gitignore` to prevent it from being uploaded to the repository.
- The application loads these variables using `require('dotenv').config()` and accesses them via `process.env.BB_USER`, `process.env.BB_PASSWORD`, etc.

### 2. User Authentication & Password Hashing
User passwords are **never stored in plain text**.
- **Registration:** When a new user registers, I use the **bcrypt** library (`bcrypt.hash`) to encrypt their password with a salt round of 10 before storing it in the `users` table.
- **Login:** During login, the application retrieves the stored hash and uses `bcrypt.compare` to securely verify the entered password against the database record.

### 3. Login Audit Trail
To track security events, I implemented an audit logging system:
- **Database:** Created an `audit_log` table to store the username, action (e.g., "Successful Login", "Failed - Wrong Password"), and a timestamp.
- **Logic:** The `/users/loggedin` route automatically inserts a record into this table for every login attempt, whether successful or failed.
- **View:** A protected route `/users/audit` displays the full history of login activity.