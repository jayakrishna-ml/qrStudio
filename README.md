---

# QR Studio

---

## Abstract

QR Studio Pro is a complete web application that allows users to create, customize, and track QR codes. Users can register with email verification, create different types of QR codes (text, links, Wi-Fi, email, SMS, vCard), customize colors and logos, and track how many times their QR codes are scanned. The system includes an admin panel for user management and analytics. The application uses FastAPI (Python) for the backend, SQLite for the database, and plain HTML/CSS/JavaScript for the frontend.

---

## 1. Introduction

QR Studio Pro is a full-stack web application designed for creating and managing QR codes. The system provides:

- User registration with email verification (OTP based)
- Secure login with JWT tokens
- QR code generation in 6 different formats
- Customization options (colors, logo overlay, size)
- Tracking system for QR code scans
- Admin dashboard with analytics
- User management for administrators

### 1.1 Purpose of the Document

This document provides complete technical documentation for the QR Studio Pro application, including system architecture, workflow diagrams, algorithms, hardware requirements, software modules, advantages, disadvantages, installation guide, and API documentation.

### 1.2 Target Audience

- Developers who want to understand or modify the code
- System administrators who need to deploy the application
- Students learning web development
- Project evaluators

---

## 2. System Architecture

### 2.1 High-Level Architecture Diagram

```
+-----------------------------------------------------------------------------+
|                              CLIENT (Browser)                                |
|  +-------------+  +-------------+  +-------------+  +-------------+        |
|  |  index.html |  |dashboard.html|  | admin.html  |  |  style.css  |        |
|  |  auth.js    |  |  dashboard   |  |  admin.js   |  |             |        |
|  +-------------+  +-------------+  +-------------+  +-------------+        |
+-----------------------------------+-----------------------------------------+
                                    | HTTP/HTTPS
                                    v
+-----------------------------------------------------------------------------+
|                           FASTAPI BACKEND                                    |
|  +-----------------------------------------------------------------------+   |
|  |                         main.py (API Routes)                         |   |
|  |  /auth/*    /users/*    /qrcodes/*    /admin/*    /scan/{qr_id}     |   |
|  +-----------------------------------------------------------------------+   |
|  +---------------+  +---------------+  +---------------+                    |
|  |   auth.py     |  |  database.py  |  |   models.py   |                    |
|  | (JWT, Passwords|  | (SQLite DB)   |  | (Pydantic)    |                    |
|  |  OTP, Hashing)|  |               |  |               |                    |
|  +---------------+  +---------------+  +---------------+                    |
+-----------------------------------+-----------------------------------------+
                                    | SQLite3
                                    v
+-----------------------------------------------------------------------------+
|                              DATABASE (qrstudio.db)                          |
|  +---------+ +---------+ +---------+ +---------+ +---------+                |
|  | users   | |otp_codes| | qrcodes | |qr_scans | |user_activity|            |
|  +---------+ +---------+ +---------+ +---------+ +---------+                |
+-----------------------------------------------------------------------------+
```

### 2.2 Technology Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| Frontend | HTML5, CSS3, JavaScript | User interface |
| Styling | CSS with Glassmorphism | Modern UI design |
| Charts | Chart.js | Analytics visualization |
| QR Generation | QRCode.js | Client-side QR creation |
| Backend | FastAPI (Python) | API server |
| Authentication | JWT (python-jose) | Token-based auth |
| Password Security | bcrypt (passlib) | Password hashing |
| Database | SQLite3 | Data storage |
| CORS | FastAPI CORSMiddleware | Cross-origin requests |

### 2.3 System Components Description

| Component | Description |
|-----------|-------------|
| **Frontend** | 5 HTML pages with responsive glassmorphism design |
| **Backend API** | 18 RESTful endpoints built with FastAPI |
| **Authentication Module** | JWT + bcrypt + OTP security |
| **Database Layer** | 5 tables with foreign key relationships |
| **QR Engine** | Client-side generation with 6 data types |

---

## 3. Workflow Diagrams

### 3.1 User Registration Flow

```
USER                  FRONTEND               BACKEND               DATABASE
 |                       |                      |                      |
 |  1. Enter Email       |                      |                      |
 |---------------------->|                      |                      |
 |                       |  2. POST /auth/request-verify-otp            |
 |                       |--------------------->|                      |
 |                       |                      |  3. Generate OTP     |
 |                       |                      |  (6 digits)          |
 |                       |                      |--------+             |
 |                       |                      |        |             |
 |                       |                      |<-------+             |
 |                       |                      |  4. Save OTP         |
 |                       |                      |--------------------->|
 |                       |  5. "Code sent"      |                      |
 |                       |<---------------------|                      |
 |  6. User checks email for code (printed in console)                 |
 |                       |                      |                      |
 |  7. Enter 6-digit OTP |                      |                      |
 |---------------------->|                      |                      |
 |                       |  8. POST /auth/verify-email                  |
 |                       |--------------------->|                      |
 |                       |                      |  9. Validate OTP     |
 |                       |                      |--------------------->|
 |                       |                      |<---------------------|
 |                       |  10. "Email verified"|                      |
 |                       |<---------------------|                      |
 |  11. Enter Password   |                      |                      |
 |---------------------->|                      |                      |
 |                       |  12. POST /auth/register                     |
 |                       |--------------------->|                      |
 |                       |                      |  13. Hash password   |
 |                       |                      |  14. Create user     |
 |                       |                      |--------------------->|
 |                       |  15. "Account created"|                      |
 |                       |<---------------------|                      |
 |  16. Login with email/password                                       |
 |--------------------------------------------------------------------->|
```

### 3.2 QR Code Creation and Tracking Flow

```
+-------------+    +-------------+    +-------------+    +-------------+
|   USER      |    |  FRONTEND   |    |  BACKEND    |    |  DATABASE   |
+------+------+    +------+------+    +------+------+    +------+------+
       |                  |                  |                  |
       | 1. Select QR Type|                  |                  |
       |   (Text/Link/    |                  |                  |
       |    WiFi/Email/   |                  |                  |
       |    SMS/vCard)    |                  |                  |
       |----------------->|                  |                  |
       |                  |                  |                  |
       | 2. Enter content |                  |                  |
       |----------------->|                  |                  |
       |                  |                  |                  |
       | 3. Click Generate|                  |                  |
       |----------------->|                  |                  |
       |                  |                  |                  |
       |                  | 4. QRCode.toCanvas()                 |
       |                  |    (Client-side generation)          |
       |                  |                  |                  |
       | 5. Preview QR    |                  |                  |
       |<-----------------|                  |                  |
       |                  |                  |                  |
       | 6. Customize:    |                  |                  |
       |   - Colors       |                  |                  |
       |   - Logo         |                  |                  |
       |   - Size         |                  |                  |
       |----------------->|                  |                  |
       |                  |                  |                  |
       | 7. Click Save    |                  |                  |
       |----------------->|                  |                  |
       |                  | 8. POST /qrcodes |                  |
       |                  |   {title, content, type}             |
       |                  |----------------->|                  |
       |                  |                  | 9. Save QR record|
       |                  |                  |----------------->|
       |                  | 10. {id: 123}    |                  |
       |                  |<-----------------|                  |
       | 11. "Saved!"     |                  |                  |
       |<-----------------|                  |                  |
       |                  |                  |                  |
       | 12. Share trackable URL: /scan/123                       |
       |                                                         |
       |              WHEN SOMEONE SCANS THE QR CODE              |
       |                                                         |
       |  Scanner     |                  |                  |
       |       |      |                  |                  |
       |       | 13. GET /scan/123       |                  |
       |       |------------------------>|                  |
       |       |                         |                  |
       |       |                         | 14. Get QR info  |
       |       |                         |----------------->|
       |       |                         |<-----------------|
       |       |                         |                  |
       |       |                         | 15. Record scan  |
       |       |                         |  - IP address    |
       |       |                         |  - User agent    |
       |       |                         |  - Timestamp     |
       |       |                         |----------------->|
       |       |                         |                  |
       |       | 16. HTML redirect page  |                  |
       |       |     with auto-redirect  |                  |
       |       |<------------------------|                  |
       |       |                         |                  |
       | 17. Browser redirects to original URL                 |
```

### 3.3 Admin Management Flow

```
ADMIN                  FRONTEND               BACKEND               DATABASE
 |                        |                      |                      |
 |  1. Login (admin@qrstudio.com / Admin123!)    |                      |
 |----------------------->|                      |                      |
 |                        |  2. POST /auth/login |                      |
 |                        |--------------------->|                      |
 |                        |                      |  3. Verify admin flag|
 |                        |                      |--------------------->|
 |                        |  4. Token + user data|                      |
 |                        |<---------------------|                      |
 |                        |                      |                      |
 |  5. Access /admin.html |                      |                      |
 |----------------------->|                      |                      |
 |                        |                      |                      |
 |                        |  6. GET /admin/stats |                      |
 |                        |--------------------->|                      |
 |                        |                      |  7. Aggregate data   |
 |                        |                      |  - Total users       |
 |                        |                      |  - Active users      |
 |                        |                      |  - QR codes count    |
 |                        |                      |  - Total scans       |
 |                        |                      |--------------------->|
 |                        |  8. Statistics JSON  |                      |
 |                        |<---------------------|                      |
 |                        |                      |                      |
 |  9. View Dashboard     |                      |                      |
 |  (Charts & Metrics)    |                      |                      |
 |<-----------------------|                      |                      |
 |                        |                      |                      |
 |  10. Click "Block User"|                      |                      |
 |----------------------->|                      |                      |
 |                        |  11. POST /admin/users/{id}/block           |
 |                        |--------------------->|                      |
 |                        |                      |  12. Update is_blocked|
 |                        |                      |--------------------->|
 |                        |  13. "User blocked"  |                      |
 |                        |<---------------------|                      |
 |  14. User blocked      |                      |                      |
 |<-----------------------|                      |                      |
```

### 3.4 Database Schema Diagram

```
+---------------------+     +---------------------+     +---------------------+
|       users         |     |     otp_codes       |     |    qrcodes          |
+---------------------+     +---------------------+     +---------------------+
| id (PK)             |     | id (PK)             |     | id (PK)             |
| email (UNIQUE)      |     | email               |     | user_id (FK --------+--+
| hashed_password     |     | code                |     | title               |  |
| full_name           |     | purpose             |     | content             |  |
| is_verified         |     | expires_at          |     | qr_type             |  |
| is_admin            |     | is_used             |     | options (JSON)      |  |
| is_blocked          |     | created_at          |     | scan_count          |  |
| created_at          |     +---------------------+     | created_at          |  |
| last_login          |                                 +---------------------+  |
+---------------------+                                                          |
         |                                                                       |
         |                                                                       |
         v                                                                       |
+---------------------+     +---------------------+                             |
|   user_activity     |     |      qr_scans       |                             |
+---------------------+     +---------------------+                             |
| id (PK)             |     | id (PK)             |                             |
| user_id (FK --------+---->| qr_id (FK) ---------+-----------------------------+
| action              |     | scanned_at          |
| details             |     | ip_address          |
| created_at          |     | user_agent          |
+---------------------+     +---------------------+

LEGEND:
----->  Foreign Key Relationship
PK = Primary Key
FK = Foreign Key
```

### 3.5 JWT Authentication Flow

```
CLIENT                    SERVER                    DATABASE
   |                         |                          |
   |  1. POST /auth/login    |                          |
   |  {email, password}      |                          |
   |------------------------>|                          |
   |                         |                          |
   |                         |  2. Query user by email  |
   |                         |------------------------->|
   |                         |<-------------------------|
   |                         |                          |
   |                         |  3. bcrypt.verify()      |
   |                         |     compare passwords    |
   |                         |                          |
   |                         |  4. Generate JWT Token   |
   |                         |     Header: {alg: HS256} |
   |                         |     Payload: {           |
   |                         |       sub: user@email.com|
   |                         |       exp: timestamp     |
   |                         |     }                    |
   |                         |     Signature: HMAC-SHA256|
   |                         |                          |
   |  5. {access_token}      |                          |
   |<------------------------|                          |
   |                         |                          |
   |  6. Store token in      |                          |
   |     localStorage        |                          |
   |                         |                          |
   |  7. Subsequent requests |                          |
   |     Authorization: Bearer <token>                  |
   |------------------------>|                          |
   |                         |                          |
   |                         |  8. Verify JWT:          |
   |                         |     - Check signature    |
   |                         |     - Check expiration  |
   |                         |     - Extract user email |
   |                         |                          |
   |  9. API Response        |                          |
   |<------------------------|                          |
   |                         |                          |
   |  Token expires after 7 days (10,080 minutes)       |
```

---

## 4. Algorithms Used

### 4.1 Password Hashing Algorithm (bcrypt)

**Purpose:** Securely store user passwords

**How it works:**

```
Plain Password: "MySecret123"
                    |
              [Salt Generation]
                    |
    Random Salt: "$2b$12$abcdefghijklmnopqrstuv"
                    |
         [bcrypt Hash Function]
                    |
    Hashed Password: "$2b$12$KxGj7YxKxLxMxNxOxPxQxRxSxTxUxVxWxXxYxZx"
                    |
              Store in Database
```

**Verification Process:**

```
User Input: "MySecret123"
    |
Retrieve stored hash from DB
    |
bcrypt.verify(input, stored_hash)
    |
Returns: True (if match) or False (if not)
```

**Time Complexity:** O(2^work_factor) - Configurable, default is 12 rounds

**Pseudocode:**

```
function hash_password(password):
    salt = generate_random_salt(rounds=12)
    hashed = bcrypt_hash(password + salt)
    return salt + hashed

function verify_password(plain_password, stored_hash):
    salt = extract_salt(stored_hash)
    computed_hash = bcrypt_hash(plain_password + salt)
    return computed_hash == stored_hash
```

---

### 4.2 JWT Token Generation Algorithm

**Purpose:** Create secure, stateless authentication tokens

**Algorithm Steps:**

**Step 1: Create Header**
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
↓ Base64Url Encode
`"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9"`

**Step 2: Create Payload**
```json
{
  "sub": "user@example.com",
  "exp": 1734567890
}
```
↓ Base64Url Encode
`"eyJzdWIiOiJ1c2VyQGV4YW1wbGUuY29tIiwiZXhwIjoxNzM0NTY3ODkwfQ"`

**Step 3: Create Signature**
```
HMACSHA256(
    base64UrlEncode(header) + "." + base64UrlEncode(payload),
    SECRET_KEY
)
```
↓
`"SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"`

**Step 4: Combine**
```
header.payload.signature
```
↓
`"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ1c2VyQGV4YW1wbGUuY29tIiwiZXhwIjoxNzM0NTY3ODkwfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"`

**Verification Process:**

```
Received Token
    |
Split into [header, payload, signature]
    |
Recompute signature using SECRET_KEY
    |
Compare with received signature
    |
If match --> Valid token
If not match --> Invalid token
    |
Check exp claim against current time
    |
If exp > now --> Token valid
If exp < now --> Token expired
```

**Pseudocode:**

```
function create_access_token(user_data):
    header = {"alg": "HS256", "typ": "JWT"}
    payload = {
        "sub": user_data.email,
        "exp": current_time + 7_days
    }
    encoded_header = base64url(JSON.stringify(header))
    encoded_payload = base64url(JSON.stringify(payload))
    signature = HMAC_SHA256(encoded_header + "." + encoded_payload, SECRET_KEY)
    return encoded_header + "." + encoded_payload + "." + signature

function verify_token(token):
    parts = token.split(".")
    if len(parts) != 3: return False
    expected_signature = HMAC_SHA256(parts[0] + "." + parts[1], SECRET_KEY)
    if expected_signature != parts[2]: return False
    payload = JSON.parse(base64url_decode(parts[1]))
    if payload.exp < current_time: return False
    return payload
```

---

### 4.3 OTP (One-Time Password) Generation

**Purpose:** Email verification and password reset

**Algorithm:**

```
function generate_otp(length = 6):
    digits = "0123456789"
    otp = ""
    for i in range(length):
        random_index = random.randint(0, 9)
        otp = otp + digits[random_index]
    return otp

Example output: "483729"
```

**Validation Logic:**

```
1. Query database for OTP matching email + code + purpose
2. Check is_used flag = 0
3. Check expires_at > current_time
4. If all true --> OTP is valid
5. Mark OTP as used after validation
```

**Pseudocode:**

```
function save_otp(email, code, purpose, validity_minutes=10):
    invalidate_previous_otps(email, purpose)
    expires_at = current_time + (validity_minutes * 60)
    INSERT INTO otp_codes (email, code, purpose, expires_at, is_used, created_at)
    VALUES (email, code, purpose, expires_at, 0, current_time)

function get_valid_otp(email, code, purpose):
    result = SELECT * FROM otp_codes
             WHERE email = email AND code = code AND purpose = purpose
             AND is_used = 0 AND expires_at > current_time
             ORDER BY id DESC LIMIT 1
    return result
```

---

### 4.4 QR Code Generation Algorithm

**Purpose:** Convert text/data into QR code matrix

**Simplified Process:**

```
Input Data: "https://example.com"
         |
Step 1: Data Analysis
    - Determine encoding mode (alphanumeric)
    - Calculate required version
         |
Step 2: Data Encoding
    - Convert to bit stream
    - Add mode indicator and character count
         |
Step 3: Error Correction
    - Add Reed-Solomon error correction codes
    - Based on selected level (L, M, Q, H)
         |
Step 4: Module Placement
    - Place finder patterns (corners)
    - Place alignment patterns
    - Place timing patterns
    - Place data modules
         |
Step 5: Masking
    - Apply mask patterns to optimize readability
    - Choose best mask (lowest penalty score)
         |
Step 6: Output Matrix
    - 2D array of black/white modules
    - Final QR code ready for rendering
```

**Error Correction Levels:**

| Level | Recovery Capacity | Use Case |
|-------|------------------|----------|
| L (Low) | 7% | Clean environment, no logo |
| M (Medium) | 15% | Default, slight damage |
| Q (Quartile) | 25% | Logo overlay possible |
| H (High) | 30% | Maximum logo size |

**Reed-Solomon Error Correction Example:**

For a QR code with version 3 and error correction level M:
- Total data capacity: 123 characters (numeric mode)
- Error correction codewords: 34 bytes
- Can recover from up to 15% damage

---

### 4.5 Password Strength Validation

**Algorithm Steps:**

```
function validate_password(password):
    min_length = 6
    checks = {
        "length": len(password) >= min_length,
        "has_digit": any(char.isdigit() for char in password),
        "has_letter": any(char.isalpha() for char in password)
    }
    
    if not checks["length"]:
        return "Password too short"
    
    return "Valid"
```

---

## 5. Hardware Requirements

### 5.1 Server Requirements

#### Minimum Requirements

| Component | Specification |
|-----------|---------------|
| Processor | 1.5 GHz dual-core |
| RAM | 2 GB |
| Storage | 500 MB free space |
| Network | 10 Mbps internet |
| Operating System | Windows 10 / macOS 11 / Linux (Ubuntu 20.04+) |

#### Recommended Requirements

| Component | Specification |
|-----------|---------------|
| Processor | 2.5 GHz quad-core |
| RAM | 4 GB or more |
| Storage | 1 GB SSD |
| Network | 50 Mbps internet |
| Operating System | Windows 11 / macOS 13 / Linux (Ubuntu 22.04+) |

### 5.2 Client Requirements (Browser)

| Requirement | Minimum |
|-------------|---------|
| Browser | Chrome 90+, Firefox 88+, Safari 14+, Edge 90+ |
| JavaScript | Enabled |
| Local Storage | Enabled |
| Screen Resolution | 1024 x 768 or higher |
| Internet Speed | 5 Mbps (for optimal experience) |

### 5.3 Network Requirements

| Requirement | Specification |
|-------------|---------------|
| Port | 8000 (default, configurable) |
| Protocol | HTTP/HTTPS |
| Firewall | Allow incoming connections on port 8000 |
| Domain | Optional for production deployment |

---

## 6. Software Modules

### 6.1 Module 1: Authentication Module (auth.py)

**Purpose:** Handle user authentication and security

**Functions Table:**

| Function Name | Parameters | Return Type | Description |
|---------------|------------|-------------|-------------|
| hash_password | password: str | str | Converts plain password to bcrypt hash |
| verify_password | plain: str, hashed: str | bool | Verifies password against stored hash |
| create_access_token | data: dict | str | Creates JWT token with 7-day expiry |
| generate_otp | length: int = 6 | str | Creates numeric OTP for verification |
| get_current_user | authorization: str | dict | Validates JWT and returns user data |
| get_current_admin | current_user: dict | dict | Ensures user has admin privileges |

**Module Diagram:**

```
+-----------------------------------------------------------------+
|                    AUTHENTICATION MODULE                         |
+-----------------------------------------------------------------+
|                                                                 |
|  hash_password(password) ----------> hashed_string              |
|                                                                 |
|  verify_password(plain, hashed) ---> boolean                    |
|                                                                 |
|  create_access_token(data) --------> jwt_string                 |
|                                                                 |
|  generate_otp(length) -------------> string (6 digits)         |
|                                                                 |
|  get_current_user(token) ----------> user_dict                  |
|                                                                 |
|  get_current_admin(user) ----------> user_dict (admin only)     |
|                                                                 |
+-----------------------------------------------------------------+
```

### 6.2 Module 2: Database Module (database.py)

**Purpose:** Handle all database operations

**Tables and Functions:**

| Table Name | Purpose | Key Functions |
|------------|---------|---------------|
| users | Store user accounts | get_user_by_email(), create_user(), get_all_users() |
| otp_codes | Store verification codes | save_otp(), get_valid_otp(), mark_otp_used() |
| qrcodes | Store QR code data | save_qr_code(), get_user_qrcodes(), delete_qrcode() |
| qr_scans | Track scan events | record_scan() |
| user_activity | Log user actions | log_user_activity() |

**Database Functions List:**

```
Database Module Functions:
├── get_db() - Context manager for database connections
├── init_db() - Creates all tables if not exists
├── get_user_by_email(email) - Fetch user by email
├── get_user_by_id(user_id) - Fetch user by ID
├── create_user(email, password, name) - Create new user
├── update_user_last_login(user_id) - Update login timestamp
├── update_user_block_status(user_id, status) - Block/unblock user
├── update_user_admin_status(user_id, status) - Grant/remove admin
├── delete_user(user_id) - Delete user and all related data
├── get_all_users() - Fetch all users
├── save_otp(email, code, purpose, minutes) - Save OTP code
├── get_valid_otp(email, code, purpose) - Validate OTP
├── mark_otp_used(otp_id) - Mark OTP as used
├── save_qr_code(user_id, title, content, type, options) - Save QR
├── get_user_qrcodes(user_id) - Get user's QR codes
├── delete_qrcode(qr_id, user_id) - Delete QR code
├── record_scan(qr_id, ip, user_agent) - Record scan event
├── log_user_activity(user_id, action, details) - Log activity
├── get_qr_code_by_id(qr_id) - Get QR by ID
└── get_admin_stats() - Get system statistics
```

### 6.3 Module 3: API Routes Module (main.py)

**Purpose:** Define all API endpoints

**Route Categories:**

```
+-----------------------------------------------------------------+
|                      API ROUTES                                  |
+-----------------------------------------------------------------+
|                                                                 |
|  AUTHENTICATION ROUTES (6 endpoints)                            |
|  ├── POST /auth/request-verify-otp  --> Send OTP for email     |
|  ├── POST /auth/verify-email        --> Verify OTP code        |
|  ├── POST /auth/register            --> Create new account     |
|  ├── POST /auth/login               --> Authenticate user      |
|  ├── POST /auth/forgot-password     --> Request reset code     |
|  └── POST /auth/reset-password      --> Reset password         |
|                                                                 |
|  USER ROUTES (1 endpoint)                                       |
|  └── GET /users/me                  --> Get current user       |
|                                                                 |
|  QR CODE ROUTES (3 endpoints)                                   |
|  ├── POST /qrcodes                  --> Save QR code           |
|  ├── GET /qrcodes                   --> List user's QR codes   |
|  └── DELETE /qrcodes/{id}           --> Delete QR code         |
|                                                                 |
|  PUBLIC ROUTES (1 endpoint)                                     |
|  └── GET /scan/{id}                 --> Redirect & track scan  |
|                                                                 |
|  ADMIN ROUTES (7 endpoints)                                     |
|  ├── GET /admin/stats               --> System statistics      |
|  ├── GET /admin/users               --> List all users         |
|  ├── POST /admin/users/{id}/block   --> Block user             |
|  ├── POST /admin/users/{id}/unblock --> Unblock user           |
|  ├── POST /admin/users/{id}/make-admin --> Grant admin         |
|  ├── POST /admin/users/{id}/remove-admin --> Remove admin      |
|  └── DELETE /admin/users/{id}       --> Delete user            |
|                                                                 |
|  TOTAL: 18 API Endpoints                                         |
+-----------------------------------------------------------------+
```

### 6.4 Module 4: Frontend Modules

**4.1 Authentication Frontend (index.html + auth.js)**

| Component | Purpose | Key Functions |
|-----------|---------|---------------|
| Login Panel | Email/password authentication | login() |
| Registration Panel | Multi-step signup with OTP | sendOTP(), verifyOTP(), register() |
| Password Reset | Forgot password recovery | forgotPassword(), resetPassword() |
| OTP Input | 6-digit code entry with auto-focus | handleOTPInput() |
| Toast Notifications | User feedback messages | showToast() |

**4.2 User Dashboard (dashboard.html)**

| Component | Purpose |
|-----------|---------|
| QR Creator | Form for generating QR codes |
| Mode Selector | Choose QR type (text, link, WiFi, email, SMS, vCard) |
| Customization Panel | Colors, size, logo, error correction |
| Live Preview | Real-time QR code display |
| Collection View | List of saved QR codes |
| Search/Filter | Find QR codes by title or type |
| Export Options | Download PNG, create trackable QR |

**4.3 Admin Panel (admin.html + admin.js)**

| Component | Purpose |
|-----------|---------|
| Stats Cards | Display key metrics (users, QR codes, scans) |
| User Growth Chart | Line chart of new users over 7 days |
| QR Types Chart | Doughnut chart of QR code distribution |
| User Table | List all users with action buttons |
| Search Box | Filter users by name or email |
| Action Buttons | Block, unblock, promote, delete |

---

## 7. Advantages

### 7.1 Security Features

| Advantage | Description |
|-----------|-------------|
| Password Hashing | bcrypt algorithm with salt protects user passwords |
| JWT Authentication | Stateless token-based security with 7-day expiry |
| OTP Verification | Email verification before account creation |
| Admin Protection | Role-based access control for admin routes |
| Input Validation | Pydantic models validate all API inputs |
| SQL Injection Prevention | Parameterized queries throughout |

### 7.2 User Experience

| Advantage | Description |
|-----------|-------------|
| Glassmorphism UI | Modern, attractive design with blur effects |
| Real-time Preview | QR code updates instantly as user types |
| Responsive Design | Works on desktop, tablet, and mobile |
| Toast Notifications | Clear feedback for all user actions |
| Multi-step Registration | Guided flow with OTP verification |
| Dark Theme | Eye-friendly dark color scheme |

### 7.3 QR Code Features

| Advantage | Description |
|-----------|-------------|
| 6 QR Types | Text, Link, WiFi, Email, SMS, vCard |
| Customization | Colors, logo overlay, size adjustment |
| Error Correction | Multiple levels (L, M, Q, H) for logo compatibility |
| Trackable QR | Scan counting and analytics per QR code |
| Local Storage | QR codes saved in database for later access |
| Easy Sharing | Copy trackable URL or download PNG |

### 7.4 Admin Capabilities

| Advantage | Description |
|-----------|-------------|
| Complete User Management | Block, delete, promote users |
| Analytics Dashboard | Charts for user growth and QR types |
| Activity Tracking | Logs of all user actions |
| Scan Statistics | Total scans and trends over time |
| Top Users | Leaderboard of most active users |

### 7.5 Technical Benefits

| Advantage | Description |
|-----------|-------------|
| Lightweight | SQLite database, no heavy dependencies |
| Easy Deployment | Single command to start server |
| Cross-Platform | Works on Windows, Mac, Linux |
| Open Source | Can be modified and extended freely |
| Minimal Dependencies | Only 6 core Python packages |
| Fast Performance | FastAPI async capabilities |

### 7.6 Cost Benefits

| Advantage | Description |
|-----------|-------------|
| Free Technologies | All software is open source |
| Low Hardware Requirements | Runs on basic hardware (2GB RAM) |
| No External API Costs | Everything is self-contained |
| No Monthly Fees | One-time setup, no recurring costs |
| Zero Licensing | No commercial licenses needed |

---

## 8. Disadvantages

### 8.1 Security Limitations

| Disadvantage | Description | Impact |
|--------------|-------------|--------|
| No HTTPS by Default | Requires manual SSL configuration | Medium |
| Secret Key in Code | Should be moved to environment variables | High |
| No Rate Limiting | Vulnerable to brute force attacks | Medium |
| No Session Management | Cannot revoke individual tokens | Low |
| OTP via Console Only | No actual email sending configured | Medium |

### 8.2 Performance Limitations

| Disadvantage | Description | Impact |
|--------------|-------------|--------|
| SQLite Concurrency | Limited concurrent write operations | Medium |
| No Caching | Repeated database queries for same data | Medium |
| Client-side QR Generation | Slower on low-end devices | Low |
| No Pagination | Large user lists may load slowly | Low |
| No Database Indexing | Some queries may be slow with large data | Medium |

### 8.3 Feature Limitations

| Disadvantage | Description |
|--------------|-------------|
| No Social Login | Only email/password authentication |
| No QR Code Editing | Cannot modify existing QR codes |
| No Bulk Operations | Cannot delete multiple QR codes at once |
| No Export Formats | Only PNG download, no SVG or PDF |
| No Dynamic QR Codes | Cannot update content without regenerating |
| No Password Strength Meter | Basic password validation only |

### 8.4 Deployment Limitations

| Disadvantage | Description |
|--------------|-------------|
| No Docker Support | Not containerized by default |
| No Database Migration | Schema changes require manual updates |
| No Backup System | No automated database backup |
| No Logging System | Console-only logging |
| No Environment Config | Hardcoded configuration values |

### 8.5 Scalability Issues

| Disadvantage | Description |
|--------------|-------------|
| Single Server Only | Cannot scale horizontally |
| No Load Balancing | All traffic to one instance |
| No Redis Integration | Cannot share sessions across servers |
| File-based Storage | Cannot use cloud storage for QR images |
| SQLite Limitations | Not suitable for high-traffic production |

### 8.6 User Experience Gaps

| Disadvantage | Description |
|--------------|-------------|
| No Mobile App | Web-only solution |
| No Dark Mode Toggle | Fixed dark theme |
| No Keyboard Shortcuts | Mouse-only navigation |
| No Offline Support | Requires internet connection |
| No Accessibility Features | Limited screen reader support |

---

## 9. Installation Guide

### 9.1 Prerequisites

```
1. Python 3.8 or higher installed
2. pip package manager
3. Internet connection for downloading dependencies
4. Git (optional, for cloning)
```

### 9.2 Step-by-Step Installation

**Step 1: Create Project Directory**
```bash
mkdir qrstudio
cd qrstudio
```

**Step 2: Create Virtual Environment (Recommended)**
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Mac/Linux
python3 -m venv venv
source venv/bin/activate
```

**Step 3: Create requirements.txt**
```txt
fastapi==0.104.1
uvicorn==0.24.0
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
python-multipart==0.0.6
pydantic==2.5.0
pydantic[email]
```

**Step 4: Install Dependencies**
```bash
pip install -r requirements.txt
```

**Step 5: Create Project Structure**
```
qrstudio/
├── backend/
│   ├── __init__.py
│   ├── main.py
│   ├── auth.py
│   ├── database.py
│   └── models.py
├── frontend/
│   ├── index.html
│   ├── dashboard.html
│   ├── admin.html
│   ├── auth.js
│   └── style.css
└── requirements.txt
```

**Step 6: Start the Server**
```bash
cd backend
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

**Step 7: Access the Application**
- Open browser: http://127.0.0.1:8000
- Default admin: admin@qrstudio.com / Admin123!

### 9.3 Troubleshooting Common Issues

| Issue | Solution |
|-------|----------|
| Port 8000 already in use | Change port: `--port 8001` |
| Module not found | Run `pip install -r requirements.txt` |
| Database locked | Close other connections to qrstudio.db |
| CORS error | Check CORS middleware configuration |
| Token invalid | Clear localStorage and login again |

---

## 10. API Documentation

### 10.1 Authentication Endpoints

#### POST /auth/request-verify-otp
**Description:** Request OTP for email verification

**Request Body:**
```json
{
    "email": "user@example.com"
}
```

**Response (200 OK):**
```json
{
    "message": "Code sent"
}
```

**Error Responses:**
| Status Code | Description |
|-------------|-------------|
| 400 | User already exists |

---

#### POST /auth/verify-email
**Description:** Verify email with OTP code

**Request Body:**
```json
{
    "email": "user@example.com",
    "code": "123456"
}
```

**Response (200 OK):**
```json
{
    "message": "Email verified"
}
```

**Error Responses:**
| Status Code | Description |
|-------------|-------------|
| 400 | Invalid code or code expired |

---

#### POST /auth/register
**Description:** Create new user account

**Request Body:**
```json
{
    "email": "user@example.com",
    "password": "password123",
    "full_name": "John Doe"
}
```

**Response (200 OK):**
```json
{
    "message": "Account created"
}
```

**Error Responses:**
| Status Code | Description |
|-------------|-------------|
| 400 | Password too short or user exists |

---

#### POST /auth/login
**Description:** Authenticate user and get JWT token

**Request Body:**
```json
{
    "email": "user@example.com",
    "password": "password123"
}
```

**Response (200 OK):**
```json
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "bearer",
    "user": {
        "id": 1,
        "email": "user@example.com",
        "full_name": "John Doe",
        "is_verified": true,
        "is_admin": false
    }
}
```

**Error Responses:**
| Status Code | Description |
|-------------|-------------|
| 401 | Invalid credentials |
| 403 | Account blocked |

---

#### POST /auth/forgot-password
**Description:** Request password reset OTP

**Request Body:**
```json
{
    "email": "user@example.com"
}
```

**Response (200 OK):**
```json
{
    "message": "Reset code sent"
}
```

---

#### POST /auth/reset-password
**Description:** Reset password using OTP

**Request Body:**
```json
{
    "email": "user@example.com",
    "code": "123456",
    "new_password": "newpassword123"
}
```

**Response (200 OK):**
```json
{
    "message": "Password reset"
}
```

---

### 10.2 User Endpoints

#### GET /users/me
**Description:** Get current authenticated user

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "id": 1,
    "email": "user@example.com",
    "full_name": "John Doe",
    "is_verified": true,
    "is_admin": false,
    "created_at": "2024-01-15T10:30:00"
}
```

---

### 10.3 QR Code Endpoints

#### POST /qrcodes
**Description:** Save a QR code to user's collection

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```json
{
    "title": "My Website",
    "content": "https://example.com",
    "qr_type": "link",
    "options": {}
}
```

**Response (200 OK):**
```json
{
    "id": 123,
    "message": "Saved"
}
```

---

#### GET /qrcodes
**Description:** Get all QR codes for current user

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
[
    {
        "id": 123,
        "title": "My Website",
        "content": "https://example.com",
        "qr_type": "link",
        "options": {},
        "scan_count": 42,
        "created_at": "2024-01-15T10:30:00"
    }
]
```

---

#### DELETE /qrcodes/{qr_id}
**Description:** Delete a QR code

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "message": "Deleted"
}
```

---

### 10.4 Public Endpoints

#### GET /scan/{qr_id}
**Description:** Redirect to QR content and record scan

**Response:** HTML page with auto-redirect

---

### 10.5 Admin Endpoints

#### GET /admin/stats
**Description:** Get system statistics (admin only)

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "total_users": 150,
    "active_users": 45,
    "blocked_users": 3,
    "total_qrcodes": 1200,
    "total_scans": 5000,
    "users_by_day": [
        {"date": "2024-01-15", "count": 5}
    ],
    "qrs_by_type": [
        {"type": "link", "count": 800}
    ],
    "scans_by_day": [
        {"date": "2024-01-15", "count": 150}
    ],
    "top_users": [
        {"email": "user@example.com", "full_name": "John Doe", "qr_count": 50}
    ]
}
```

---

#### GET /admin/users
**Description:** Get all users (admin only)

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
[
    {
        "id": 1,
        "email": "user@example.com",
        "full_name": "John Doe",
        "is_verified": true,
        "is_admin": false,
        "is_blocked": false,
        "created_at": "2024-01-15T10:30:00",
        "last_login": "2024-01-20T15:45:00"
    }
]
```

---

#### POST /admin/users/{user_id}/block
**Description:** Block a user (admin only)

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "message": "User blocked"
}
```

---

#### POST /admin/users/{user_id}/unblock
**Description:** Unblock a user (admin only)

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "message": "User unblocked"
}
```

---

#### POST /admin/users/{user_id}/make-admin
**Description:** Grant admin privileges (admin only)

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "message": "User is now admin"
}
```

---

#### POST /admin/users/{user_id}/remove-admin
**Description:** Remove admin privileges (admin only)

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "message": "Admin role removed"
}
```

---

#### DELETE /admin/users/{user_id}
**Description:** Delete a user (admin only)

**Headers:**
```
Authorization: Bearer <token>
```

**Response (200 OK):**
```json
{
    "message": "User deleted"
}
```

---

## 11. Security Features

### 11.1 Password Security

| Feature | Implementation |
|---------|----------------|
| Hashing Algorithm | bcrypt with 12 rounds |
| Salt | Automatic per-password salt |
| Minimum Length | 6 characters |
| Storage | Never stored in plain text |

### 11.2 Authentication Security

| Feature | Implementation |
|---------|----------------|
| Token Type | JWT (JSON Web Token) |
| Token Expiry | 7 days (10,080 minutes) |
| Algorithm | HS256 (HMAC-SHA256) |
| Storage | Client-side localStorage |

### 11.3 Data Validation

| Feature | Implementation |
|---------|----------------|
| Input Validation | Pydantic models |
| Email Validation | EmailStr type |
| SQL Injection | Parameterized queries |
| XSS Protection | HTML escaping in templates |

### 11.4 Access Control

| Feature | Implementation |
|---------|----------------|
| Role-Based Access | Admin vs Regular user |
| Blocked Users | Cannot authenticate |
| Protected Routes | JWT verification required |

---

## 12. Conclusion

QR Studio Pro is a complete, feature-rich web application for creating and managing QR codes. It provides:

- **Full authentication system** with email verification and JWT tokens
- **6 types of QR codes** with extensive customization options
- **Tracking capabilities** to monitor QR code scans
- **Admin dashboard** for user management and analytics
- **Modern UI** with glassmorphism design

The application is suitable for:
- Small businesses needing QR code management
- Educational purposes for learning full-stack development
- Personal use for creating and tracking QR codes

**Future Improvements:**
- Add email service integration (SMTP)
- Implement rate limiting
- Add QR code editing functionality
- Create mobile application
- Add more export formats (SVG, PDF)
- Implement database backups
- Add Docker containerization

---

## Appendix A: File Structure

```
qrstudio/
│
├── backend/
│   ├── __init__.py          # Package initializer
│   ├── main.py              # FastAPI application & routes
│   ├── auth.py              # Authentication functions
│   ├── database.py          # Database operations
│   └── models.py            # Pydantic data models
│
├── frontend/
│   ├── index.html           # Login/Register page
│   ├── dashboard.html       # User dashboard
│   ├── admin.html           # Admin panel
│   ├── auth.js              # Frontend authentication
│   └── style.css            # Global styles
│
├── qrstudio.db              # SQLite database (auto-generated)
└── requirements.txt         # Python dependencies
```

---

## Appendix B: Default Admin Credentials

| Field | Value |
|-------|-------|
| Email | admin@qrstudio.com |
| Password | Admin123! |
| Role | Super Admin |

---

## Appendix C: QR Type Examples

| Type | Format Example |
|------|----------------|
| Text | "Hello World" |
| Link | "https://example.com" |
| WiFi | "WIFI:T:WPA;S:MyNetwork;P:password123;;" |
| Email | "mailto:user@example.com?subject=Hello&body=Message" |
| SMS | "smsto:+1234567890:Hello" |
| vCard | "BEGIN:VCARD\nVERSION:3.0\nFN:John Doe\nEND:VCARD" |

---

## Appendix D: Glossary

| Term | Definition |
|------|------------|
| API | Application Programming Interface |
| bcrypt | Password hashing function |
| CORS | Cross-Origin Resource Sharing |
| FastAPI | Modern Python web framework |
| JWT | JSON Web Token |
| OTP | One-Time Password |
| QR Code | Quick Response code |
| SQLite | Lightweight database engine |
| vCard | Electronic business card format |

---

**Document Version:** 1.0  
**Last Updated:** 15/04/2026 
**Author:** Radhika Pochampally

---

*End of Documentation*
