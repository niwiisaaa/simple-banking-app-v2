# PS_GROUP6_Final-Project

### CSEC 322 - Platform Security 
### Securing an Existing Banking Application - Security Assessment and Improvement of a Web-Based Banking Application
**Date:** May 24, 2025

## 👥 Group Members

- De Silva, Mark Bryan 
- Parañal, Mary Rachel
- Valle, Nerisa

---

## 🔗 Presentation Link 

- https://drive.google.com/drive/folders/1Nnp6GYebYcruugdzz-mV4gJjWa-51IQn?usp=sharing
- https://youtu.be/vQQjlTOnSbk?feature=shared

- To view app click this link: https://nerisa.pythonanywhere.com/login?next=%2F

--- 

## 📖 Introduction

In today’s increasingly digital financial landscape, web-based banking applications are prime targets for cyber threats. This project undertakes a comprehensive security assessment and enhancement of the **Simple Banking App**, a user-friendly and responsive Flask-based banking application designed for deployment on PythonAnywhere. This application allows users to create accounts, perform simulated money transfers between accounts, view transaction history, and securely manage their credentials.

By identifying key vulnerabilities, implementing secure coding practices, and reinforcing critical components like authentication, data handling, and access control, our team aimed to elevate the app’s resilience against real-world attack vectors.

---

## 🎯 Objectives

- To identify and assess vulnerabilities within the original Simple Banking App.
- To improve authentication, session management, input validation, and error handling mechanisms.
- To conduct penetration testing and validate improvements using industry-standard tools.
- To document the assessment and improvement process following best practices.

---

## 🏛️ Original Application Features

The existing Simple Banking App already implements the following features:

1. **User Authentication**  
   - Login with username and password  
   - Registration of new users  
   - Password recovery mechanism  

2. **Account Management**  
   - Display of account balance  
   - View of transaction history  

3. **Fund Transfer**  
   - Transfer money to other registered users  
   - Confirmation screen before completing transfers  
   - Transaction history updated after transfers  

4. **User Role Management**  
   - Regular user accounts  
   - Admin users with account approval capabilities  
   - Manager users who can manage admin accounts  

5. **Location Data Integration**  
   - Philippine Standard Geographic Code (PSGC) API integration  
   - Hierarchical location data selection 

---

## 🕵️ Security Assessment Findings

The following vulnerabilities were discovered during the initial security audit:

- **Weak Password Policy:** No enforcement of complexity rules.
- **Plaintext Password Storage:** Passwords were hashed with insecure methods.
- **Session Fixation Vulnerabilities:** Sessions were not regenerated after login.
- **Insufficient Input Validation:** Several fields were susceptible to XSS and SQL injection.
- **CSRF Vulnerabilities:** Forms lacked CSRF protection.
- **Overly Permissive Role Management:** Privilege escalation was possible via direct URL access.
- **Error Disclosure:** Raw error messages exposed sensitive system information.
- **Lack of Rate Limiting:** Brute-force attacks could be executed without restriction.
- **Insecure Dependencies:** Outdated Flask and SQLAlchemy versions were used.

--- 

## 🔐 Security Improvements Implemented

The following security enhancements were applied to harden the application:

1. **Authentication and Password Security**
   - Enforced strong password policy using regex validation (uppercase, lowercase, digit, special character, min 8 chars)
   - Removed fallback to SHA-256, enforced `bcrypt` for all password storage
   - Added real-time password strength meter to registration and account creation forms

2. **Session Management**
   - Enabled secure, HTTPOnly, and SameSite cookie flags
   - Sessions are regenerated on login/logout to prevent fixation attacks

3. **Input Validation**
   - Sanitized and validated all user input fields
   - Stricter validators added on all forms (length, format, content)

4. **CSRF Protection**
   - Ensured CSRF tokens are used on all forms via `Flask-WTF`
   - Globally enabled CSRF protection for the app

5. **Authorization & Role Management**
   - Enforced strict role checks for all Admin/Manager actions
   - Prevented privilege escalation by validating user roles before sensitive actions

6. **Error Handling**
   - Added custom error handlers for 404, 500, and unhandled exceptions
   - Prevented leakage of system/internal information to the user
  
7. **Rate Limiting**
   - Used `Flask-Limiter` to prevent brute-force attacks and abuse on sensitive routes

8. **Dependencies**
   - Updated all outdated libraries and removed insecure fallbacks

---

## 🛡️ Penetration Testing Report

### Summary of Exploited Vulnerabilities

Penetration testing was conducted using industry-standard tools:

- **OWASP ZAP** and **Burp Suite** uncovered:
   - Reflected XSS in profile update form.
   - SQL injection on the transfer amount input.
   - Missing or weak CSP and HSTS headers.
- **Nmap** scan indicated open development ports.
- **Nikto** identified server misconfiguration and missing security headers.

### Recommendations

- Sanitize and validate all user inputs both client- and server-side.
- Use parameterized queries to prevent SQL injection.
- Implement strict Content Security Policy (CSP).
- Force HTTPS and enable HTTP Strict Transport Security (HSTS).
- Regenerate sessions upon login and logout.
- Rate-limit sensitive endpoints like login and registration.
- Enforce role-based access checks in backend routes.

---

## 🛠️ Remediation Plan

To address the vulnerabilities found, the following steps were taken:

1. **Authentication**
   - Implemented password strength checks using regex.
   - Migrated from SHA-256 to bcrypt for hashing.

2. **Session Management**
   - Set session cookies to Secure, HttpOnly, and SameSite=Strict.
   - Regenerated session IDs on login/logout.

3. **Input Validation**
   - Applied input sanitization across all forms.
   - Used WTForms validators for strict length and type checks.

4. **CSRF Protection**
   - Added CSRF tokens to all forms using Flask-WTF.
   
5. **Authorization Controls**
   - Ensured access control decorators for all sensitive views.

6. **Error Handling**
   - Replaced debug error messages with user-friendly custom error pages.

7. **Rate Limiting**
   - Introduced Flask-Limiter with tiered request caps per route.

8. **Dependency Management**
   - Updated all Python packages to the latest stable versions.

9. **Secure Deployment**
   - Configured HTTPS via PythonAnywhere and enforced SSL redirects.

---

## 🧰 Technology Stack

Updated list of technologies used:

- **Backend**: Python, Flask
- **Database**: MySQL (with SQLAlchemy ORM)
- **Frontend**: HTML, CSS, Bootstrap 5
- **Authentication**: Flask-Login, Werkzeug, Flask-Bcrypt
- **Forms**: Flask-WTF, WTForms
- **Security**: Flask-Limiter for API rate limiting, CSRF protection
- **External API**: PSGC API for Philippine geographic data

---

## ⚙️ Setup Instructions

### Prerequisites
- Python 3.7+
- pip (Python package manager)
- MySQL Server 5.7+ or MariaDB 10.2+

### Database Setup

1. Install MySQL Server or MariaDB if you haven't already:
   ```
   # For Ubuntu/Debian
   sudo apt update
   sudo apt install mysql-server
   
   # For macOS with Homebrew
   brew install mysql
   
   # For Windows
   # Download and install from the official website
   ```

2. Create a database user and set privileges:
   ```
   mysql -u root -p
   
   # In MySQL prompt
   CREATE USER 'bankapp'@'localhost' IDENTIFIED BY 'your_password';
   GRANT ALL PRIVILEGES ON *.* TO 'bankapp'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

3. Update the `.env` file with your MySQL credentials:
   ```
   DATABASE_URL=mysql+pymysql://bankapp:your_password@localhost/simple_banking
   MYSQL_USER=bankapp
   MYSQL_PASSWORD=your_password
   MYSQL_HOST=localhost
   MYSQL_PORT=3306
   MYSQL_DATABASE=simple_banking
   ```

4. Initialize the database:
   ```
   python init_db.py
   ```

### Installation

1. Clone the repository:
   ```
   git clone https://github.com/lanlanjr/simple-banking-app.git
   cd simple-banking-app
   
   # Set up your own repository
   # First, create a new repository named 'simple-banking-app-v2' on GitHub
   
   # Then configure your local repository
   git remote remove origin
   git remote add origin https://github.com/yourusername/simple-banking-app-v2.git
   git remote set-url origin https://yourusername@github.com/yourusername/simple-banking-app-v2.git
   git branch -M main
   git push -u origin main
   
   # Note: Replace 'yourusername' with your actual GitHub username
   ```

2. Install the required packages:
   ```
   pip install -r requirements.txt
   ```

3. Run the application:
   ```
   python app.py
   ```

4. Access the application at `http://localhost:5000`

### Deploying to PythonAnywhere

1. Create a PythonAnywhere account at [www.pythonanywhere.com](https://www.pythonanywhere.com)

2. Upload your code using Git:
   ```
   git clone https://github.com/yourusername/simple-banking-app-v2.git
   ```

3. Install requirements:
   ```
   cd simple-banking-app-v2
   pip install -r requirements.txt
   ```

4. Set up MySQL database on PythonAnywhere:
   - Go to the Databases tab in your PythonAnywhere dashboard
   - Create a new MySQL database
   - Note the database name, username, and password
   - Update your .env file with these credentials

5. Initialize your database on PythonAnywhere:
   ```
   python init_db.py
   ```

6. Configure a new web app via the PythonAnywhere dashboard:
   - Select "Manual configuration"
   - Choose Python 3.8
   - Set source code directory to `/home/yourusername/simple-banking-app-v2`
   - Set working directory to `/home/yourusername/simple-banking-app-v2`
   - Set WSGI configuration file to point to your Flask app

7. Add environment variables in the PythonAnywhere dashboard for security

### Usage

#### Registration
- Navigate to the registration page
- Enter username, email, and password
- Confirm your password
- Submit the form to create your account (pending admin approval)

#### Login
- Enter your username and password
- Click "Sign In"

#### Account Overview
- View your current balance
- See your recent transaction history

#### Transfer Funds
- Navigate to the Transfer page
- Enter recipient's username or account number
- Enter the amount to transfer
- Confirm the transfer details on the confirmation screen
- Complete the transfer

#### Password Reset
- Click "Forgot your password?" on the login page
- Enter your registered email address
- Follow the link in the email (simulated in this demo)
- Create a new password

#### Admin Features
- Approve new user registrations
- Activate/deactivate user accounts
- Create new user accounts
- Make over-the-counter deposits to user accounts
- Edit user details including location information

#### Manager Features
- Create new admin accounts
- Toggle admin status for users
- View all user transactions
- Monitor and audit admin activities

### User Roles

The system supports three types of user roles:

1. **Regular Users** - Can manage their own account, make transfers, and view their transaction history.

2. **Admin Users** - Have all regular user privileges plus:
   - Approve/reject new user registrations
   - Activate/deactivate user accounts
   - Create new user accounts
   - Make deposits to user accounts
   - Edit user information

3. **Manager Users** - Have all admin privileges plus:
   - Create and manage admin accounts
   - View admin transaction logs
   - Monitor all system transfers
   - System-wide oversight capabilities

### Address Management with PSGC API

The application integrates with the Philippine Standard Geographic Code (PSGC) API to provide standardized address selection for user profiles. The address system follows the Philippine geographical hierarchy:

- Region
- Province
- City/Municipality
- Barangay

This integration ensures addresses are standardized and validates location data according to the Philippine geographical structure.

### Rate Limiting

The application uses Flask-Limiter to implement API rate limiting, which protects against potential DoS attacks and abusive bot activity. The rate limits are configured as follows:

- **Login**: 10 attempts per minute
- **Registration**: 5 attempts per minute
- **Password Reset**: 5 attempts per hour
- **Money Transfer**: 20 attempts per hour
- **API Endpoints**: 30 requests per minute
- **Admin Dashboard**: 60 requests per hour
- **Admin Account Creation**: 20 accounts per hour
- **Admin Deposits**: 30 deposits per hour
- **Manager Dashboard**: 60 requests per hour
- **Admin Creation**: 10 admin accounts per hour

By default, the rate limiting data is stored in memory. For production use, it's recommended to use Redis as a storage backend for persistence across application restarts. To enable Redis storage:

1. Install Redis server on your system
2. Update the `.env` file with your Redis URL:
   ```
   REDIS_URL=redis://localhost:6379/0
   ```

If Redis is not available, the application will automatically fall back to in-memory storage.

--- 

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.
