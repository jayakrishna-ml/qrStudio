<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>QR Studio - Project Documentation</title>
</head>
<body>

<h1 align="center">QR STUDIO</h1>
<h2 align="center">Complete QR Code Management System</h2>
<h3 align="center">Project Documentation & Technical Report</h3>

<hr>

<h2>ABSTRACT</h2>

<p>QR Studio is a complete web application that allows users to create, customize, and manage QR codes with advanced features. The system provides a secure authentication system, QR code generation with customization options, tracking capabilities, and an administrative dashboard for user management and analytics. Built with modern web technologies including FastAPI for the backend and responsive HTML/CSS/JavaScript for the frontend, QR Studio offers a professional solution for businesses and individuals needing QR code management with scan tracking and user analytics.</p>

<hr>

<h2>1. SYSTEM OVERVIEW</h2>

<p>QR Studio is built as a full-stack web application using FastAPI for the backend and pure HTML, CSS, and JavaScript for the frontend. The application follows a client-server architecture where the backend handles data storage, authentication, and business logic, while the frontend provides an intuitive user interface for interacting with the system.</p>

<p>The system uses SQLite as the database, JWT tokens for authentication, bcrypt for password hashing, and QRCode.js for generating QR codes. The application runs locally but can be deployed to any cloud platform supporting Python applications.</p>

<hr>

<h2>2. KEY FEATURES</h2>

<h3>2.1 User Authentication and Security</h3>
<ul>
    <li>Email-based registration with OTP verification (6-digit code)</li>
    <li>Secure password hashing using bcrypt algorithm</li>
    <li>JWT token-based authentication with 7-day expiration</li>
    <li>Password reset functionality via email OTP</li>
    <li>Session management with automatic token validation</li>
    <li>Account blocking system for administrators</li>
</ul>

<h3>2.2 QR Code Management</h3>
<ul>
    <li>Create multiple types of QR codes including text, links, Wi-Fi credentials, email, SMS, and vCard</li>
    <li>Customize QR code appearance with colors, size, margin, and error correction levels</li>
    <li>Add logo overlay to QR codes with adjustable size</li>
    <li>Save QR codes to personal collection with custom titles</li>
    <li>Track scan statistics for each QR code</li>
    <li>Edit and delete saved QR codes</li>
    <li>Download QR codes as PNG images</li>
</ul>

<h3>2.3 Admin Dashboard</h3>
<ul>
    <li>System statistics and analytics dashboard with key metrics</li>
    <li>User management with search, block, and unblock functionality</li>
    <li>Role management (promote/demote admin users)</li>
    <li>User deletion capability with all associated data</li>
    <li>Visual charts for user growth and QR code distribution using Chart.js</li>
    <li>Activity logging for audit purposes</li>
</ul>

<hr>

<h2>3. TECHNICAL ARCHITECTURE</h2>

<h3>3.1 Backend Components</h3>
<ul>
    <li><strong>FastAPI Framework:</strong> Provides RESTful API endpoints with automatic OpenAPI documentation at /docs</li>
    <li><strong>SQLite Database:</strong> Lightweight file-based database storing users, QR codes, scan records, and OTP codes</li>
    <li><strong>JWT Authentication:</strong> Secure token-based user authentication with HS256 algorithm</li>
    <li><strong>Password Hashing:</strong> bcrypt algorithm for secure password storage with salt rounds</li>
    <li><strong>Pydantic Models:</strong> Data validation and serialization for API requests/responses</li>
</ul>

<h3>3.2 Frontend Components</h3>
<ul>
    <li><strong>Responsive Design:</strong> Mobile-friendly interface using CSS Grid and Flexbox layouts</li>
    <li><strong>Dynamic Forms:</strong> Interactive forms for different QR code types that change based on selection</li>
    <li><strong>Live Preview:</strong> Real-time QR code generation as users type or change settings</li>
    <li><strong>Dashboard Interface:</strong> Separate panels for regular users and administrators with role-based access</li>
    <li><strong>Glass Morphism Styling:</strong> Modern UI with backdrop blur effects and gradient backgrounds</li>
</ul>

<hr>

<h2>4. WORKFLOW DESCRIPTION</h2>

<h3>4.1 User Registration Process</h3>
<ol>
    <li>User enters email address and optional full name on registration form</li>
    <li>System sends 6-digit OTP to email (displayed in terminal for demo purposes)</li>
    <li>User enters the 6-digit OTP code for verification</li>
    <li>System validates OTP and checks expiration (10 minute validity)</li>
    <li>User creates password (minimum 6 characters) and confirms it</li>
    <li>Account is created in database with is_verified = 1</li>
    <li>User can now login with email and password</li>
</ol>

<h3>4.2 Login Process</h3>
<ol>
    <li>User provides email and password on login form</li>
    <li>System validates credentials against database</li>
    <li>System checks if account is blocked (is_blocked flag)</li>
    <li>Upon success, JWT token is generated with user email as subject</li>
    <li>Token is stored in localStorage on browser</li>
    <li>Last login timestamp is updated in database</li>
    <li>User activity is logged (action: "login")</li>
    <li>User is redirected to dashboard (admin users go to admin panel)</li>
</ol>

<h3>4.3 QR Code Creation Workflow</h3>
<ol>
    <li>User selects QR code type from options: Text, Link, Wi-Fi, Email, SMS, or vCard</li>
    <li>User fills in the required information for selected type</li>
    <li>QR code is generated automatically and displayed in real-time preview</li>
    <li>User can customize appearance: size (200-500px), margin (0-4), error correction (L/M/Q/H)</li>
    <li>User can change colors: foreground (dot color) and background color using color picker</li>
    <li>User can upload logo image (PNG/JPG) to overlay on QR code center (size adjustable 10-35%)</li>
    <li>User can save QR code with a title to personal collection</li>
    <li>User can download QR code as PNG image file</li>
    <li>User can create trackable QR code that records scan statistics via unique URL</li>
</ol>

<h3>4.4 QR Code Tracking Workflow</h3>
<ol>
    <li>Each saved QR code gets a unique tracking URL: http://localhost:8000/scan/{qr_id}</li>
    <li>When someone scans and visits the tracking URL, the system records:
        <ul>
            <li>Scan timestamp (datetime.utcnow())</li>
            <li>IP address from request client</li>
            <li>User agent from request headers</li>
        </ul>
    </li>
    <li>QR code scan_count is incremented by 1 in database</li>
    <li>Scan record is added to qr_scans table</li>
    <li>User is redirected to the actual content URL after 0 seconds with visual redirect page</li>
    <li>User can view scan count for each QR code in collection on dashboard</li>
</ol>

<h3>4.5 Admin Management Workflow</h3>
<ol>
    <li>Admin user logs in and is automatically redirected to admin dashboard (admin.html)</li>
    <li>Dashboard displays statistics: total users, active users (last 7 days), blocked users, total QR codes, total scans</li>
    <li>Charts show user growth over last 7 days and QR code distribution by type</li>
    <li>Admin can view all users in table with search functionality by name or email</li>
    <li>Admin can block or unblock any user (except themselves)</li>
    <li>Admin can promote regular users to admin or demote admins to regular users</li>
    <li>Admin can delete user accounts (except themselves) - deletes all associated QR codes and activity</li>
    <li>All admin actions are logged in user_activity table</li>
</ol>

<hr>

<h2>5. DATABASE SCHEMA</h2>

<h3>5.1 Users Table</h3>
<pre>
| Column           | Type    | Description                              |
|------------------|---------|------------------------------------------|
| id               | INTEGER | Primary key, auto-increment              |
| email            | TEXT    | Unique email address (lowercase)         |
| hashed_password  | TEXT    | bcrypt hashed password                   |
| full_name        | TEXT    | Optional display name                    |
| is_verified      | INTEGER | 0 or 1, email verification status        |
| is_admin         | INTEGER | 0 or 1, admin privilege flag             |
| is_blocked       | INTEGER | 0 or 1, account blocked status           |
| created_at       | TEXT    | ISO format timestamp of account creation |
| last_login       | TEXT    | ISO format timestamp of last login       |
</pre>

<h3>5.2 OTP Codes Table</h3>
<pre>
| Column      | Type    | Description                              |
|-------------|---------|------------------------------------------|
| id          | INTEGER | Primary key, auto-increment              |
| email       | TEXT    | Email address associated with OTP        |
| code        | TEXT    | 6-digit numeric code                     |
| purpose     | TEXT    | "verify_email" or "reset_password"       |
| expires_at  | TEXT    | ISO timestamp when code expires (10 min) |
| is_used     | INTEGER | 0 or 1, prevents code reuse              |
| created_at  | TEXT    | ISO timestamp of code creation           |
</pre>

<h3>5.3 QR Codes Table</h3>
<pre>
| Column      | Type    | Description                              |
|-------------|---------|------------------------------------------|
| id          | INTEGER | Primary key, auto-increment              |
| user_id     | INTEGER | Foreign key to users.id                  |
| title       | TEXT    | User-provided title for QR code          |
| content     | TEXT    | Actual QR code payload/content           |
| qr_type     | TEXT    | text, link, wifi, email, sms, vcard      |
| options     | TEXT    | JSON string of customization options     |
| scan_count  | INTEGER | Total number of scans (default 0)        |
| created_at  | TEXT    | ISO timestamp of QR creation             |
</pre>

<h3>5.4 QR Scans Table</h3>
<pre>
| Column      | Type    | Description                              |
|-------------|---------|------------------------------------------|
| id          | INTEGER | Primary key, auto-increment              |
| qr_id       | INTEGER | Foreign key to qrcodes.id                |
| scanned_at  | TEXT    | ISO timestamp of scan event              |
| ip_address  | TEXT    | Scanner's IP address                     |
| user_agent  | TEXT    | Scanner's browser/user agent string      |
</pre>

<h3>5.5 User Activity Table</h3>
<pre>
| Column      | Type    | Description                              |
|-------------|---------|------------------------------------------|
| id          | INTEGER | Primary key, auto-increment              |
| user_id     | INTEGER | Foreign key to users.id                  |
| action      | TEXT    | login, create_qr, delete_qr, block_user |
| details     | TEXT    | Optional additional information          |
| created_at  | TEXT    | ISO timestamp of activity                |
</pre>

<hr>

<h2>6. SECURITY FEATURES</h2>

<ul>
    <li><strong>Password Hashing:</strong> bcrypt algorithm prevents plain text password storage with automatic salting</li>
    <li><strong>JWT Token Expiration:</strong> Tokens expire after 7 days (ACCESS_TOKEN_EXPIRE_MINUTES = 60*24*7)</li>
    <li><strong>OTP Expiration:</strong> Verification codes expire after 10 minutes to prevent reuse</li>
    <li><strong>OTP One-Time Use:</strong> Used OTP codes are marked as is_used=1 and cannot be reused</li>
    <li><strong>Account Blocking:</strong> Blocked users (is_blocked=1) cannot access any authenticated endpoints</li>
    <li><strong>Admin Protection:</strong> Admin-only endpoints require is_admin=1 and return 403 for regular users</li>
    <li><strong>Self-Protection:</strong> Users cannot block, delete, or change their own admin status</li>
    <li><strong>Email Normalization:</strong> All emails are stored in lowercase and trimmed to prevent case-sensitive duplicates</li>
    <li><strong>Authorization Header:</strong> All protected endpoints require valid Bearer token in Authorization header</li>
</ul>

<hr>

<h2>7. API ENDPOINTS</h2>

<h3>7.1 Authentication Endpoints</h3>
<pre>
POST   /auth/request-verify-otp    - Request email verification code
POST   /auth/verify-email          - Verify email with OTP code
POST   /auth/register              - Create new user account
POST   /auth/login                 - Authenticate and get JWT token
POST   /auth/forgot-password       - Request password reset code
POST   /auth/reset-password        - Reset password with OTP
</pre>

<h3>7.2 User Endpoints</h3>
<pre>
GET    /users/me                   - Get current authenticated user profile
</pre>

<h3>7.3 QR Code Endpoints</h3>
<pre>
POST   /qrcodes                    - Create new QR code
GET    /qrcodes                    - Get all QR codes for current user
DELETE /qrcodes/{id}               - Delete specific QR code
GET    /scan/{id}                  - Public tracking endpoint (no auth required)
</pre>

<h3>7.4 Admin Endpoints</h3>
<pre>
GET    /admin/stats                - Get system statistics and chart data
GET    /admin/users                - Get all users in system
POST   /admin/users/{id}/block     - Block a user account
POST   /admin/users/{id}/unblock   - Unblock a user account
POST   /admin/users/{id}/make-admin - Promote user to admin
POST   /admin/users/{id}/remove-admin - Demote admin to regular user
DELETE /admin/users/{id}           - Delete user and all associated data
</pre>

<hr>

<h2>8. FRONTEND PAGES</h2>

<h3>8.1 Login/Signup Page (index.html)</h3>
<ul>
    <li>Tabbed interface for Login and Registration</li>
    <li>Multi-step registration with email → OTP verification → password creation</li>
    <li>Password reset functionality with email OTP</li>
    <li>Password visibility toggle on all password fields</li>
    <li>Responsive design with glass morphism styling</li>
    <li>OTP input boxes with automatic focus advancement</li>
    <li>Timer display for OTP expiration</li>
</ul>

<h3>8.2 User Dashboard (dashboard.html)</h3>
<ul>
    <li>Two main tabs: "Create QR" and "My QR Codes"</li>
    <li>QR code generator with live preview (updates on every keystroke)</li>
    <li>Multiple QR code type support with dynamic forms</li>
    <li>Color picker for foreground and background colors</li>
    <li>Size control slider (200-500px) with real-time preview</li>
    <li>Margin control slider (0-4) with real-time preview</li>
    <li>Error correction level selector (L/M/Q/H)</li>
    <li>Logo upload functionality with preview and size adjustment</li>
    <li>Save QR code to collection with custom title</li>
    <li>Download PNG button for direct image download</li>
    <li>Trackable QR button for creating scan-tracked QR codes</li>
    <li>Saved QR codes grid with search by title and filter by type</li>
    <li>Edit, share, and delete options for each saved QR code</li>
    <li>Modal popup for viewing and downloading QR codes</li>
</ul>

<h3>8.3 Admin Dashboard (admin.html)</h3>
<ul>
    <li>Statistics cards showing total users, active users, blocked users, total QR codes, total scans</li>
    <li>Line chart for user growth over last 7 days using Chart.js</li>
    <li>Doughnut chart for QR code distribution by type</li>
    <li>User management table with search functionality</li>
    <li>Status badges for verified, blocked, and admin roles</li>
    <li>Action buttons for block/unblock, promote/demote, and delete</li>
    <li>Toast notifications for action confirmations</li>
    <li>Admin-only access control (redirects regular users)</li>
</ul>

<hr>

<h2>9. TECHNOLOGY STACK</h2>

<h3>9.1 Backend Technologies</h3>
<ul>
    <li><strong>Python 3.8+</strong> - Programming language</li>
    <li><strong>FastAPI 0.104.1</strong> - Modern web framework for building APIs</li>
    <li><strong>Uvicorn 0.24.0</strong> - ASGI server for running FastAPI</li>
    <li><strong>SQLite3</strong> - Lightweight file-based database (no separate server)</li>
    <li><strong>python-jose 3.3.0</strong> - JWT token creation and verification</li>
    <li><strong>passlib 1.7.4</strong> - Password hashing library with bcrypt</li>
    <li><strong>python-multipart 0.0.6</strong> - Form data parsing</li>
    <li><strong>Pydantic 2.5.0</strong> - Data validation and settings management</li>
</ul>

<h3>9.2 Frontend Technologies</h3>
<ul>
    <li><strong>HTML5</strong> - Page structure and semantics</li>
    <li><strong>CSS3</strong> - Styling with Flexbox, Grid, animations, backdrop-filter</li>
    <li><strong>Vanilla JavaScript</strong> - No frameworks, pure DOM manipulation</li>
    <li><strong>QRCode.js 1.5.1</strong> - QR code generation library</li>
    <li><strong>Chart.js</strong> - Data visualization for admin analytics</li>
    <li><strong>Font Awesome 6.4.0</strong> - Icon library for UI elements</li>
    <li><strong>Google Fonts (Inter)</strong> - Modern sans-serif font family</li>
</ul>

<hr>

<h2>10. DEPLOYMENT INSTRUCTIONS</h2>

<h3>10.1 Prerequisites</h3>
<ul>
    <li>Python 3.8 or higher installed on system</li>
    <li>pip package manager</li>
    <li>Git (optional, for cloning repository)</li>
</ul>

<h3>10.2 Installation Steps</h3>
<ol>
    <li>Extract or clone the project to a folder</li>
    <li>Open terminal/command prompt in project root directory</li>
    <li>Install required Python packages:
        <pre>pip install -r requirements.txt</pre>
    </li>
    <li>Run the FastAPI application:
        <pre>uvicorn app.main:app --reload --host 127.0.0.1 --port 8000</pre>
    </li>
    <li>Access the application at: <strong>http://127.0.0.1:8000</strong></li>
    <li>Default admin credentials:
        <ul>
            <li>Email: admin@qrstudio.com</li>
            <li>Password: Admin123!</li>
        </ul>
    </li>
</ol>

<h3>10.3 Project Structure</h3>
<pre>
qr-studio/
├── app/
│   ├── __init__.py          # Package initializer
│   ├── main.py              # FastAPI application (routes, startup)
│   ├── auth.py              # Authentication functions (JWT, password, OTP)
│   ├── database.py          # Database operations (SQLite queries)
│   └── models.py            # Pydantic models for request/response
├── frontend/
│   ├── index.html           # Login and signup page
│   ├── dashboard.html       # User dashboard for QR management
│   ├── admin.html           # Admin dashboard with analytics
│   ├── auth.js              # Frontend authentication logic
│   └── style.css            # Global CSS styles
├── qrstudio.db              # SQLite database file (auto-created)
└── requirements.txt         # Python dependencies list
</pre>

<hr>

<h2>11. DATABASE INITIALIZATION</h2>

<p>On first startup, the system automatically:</p>
<ol>
    <li>Creates all five tables if they don't exist (users, otp_codes, qrcodes, qr_scans, user_activity)</li>
    <li>Creates default admin user with email admin@qrstudio.com and password Admin123!</li>
    <li>Ensures admin user has is_admin=1 and is_verified=1 flags set</li>
    <li>Prints confirmation messages to console for debugging</li>
</ol>

<hr>

<h2>12. ERROR HANDLING</h2>

<ul>
    <li><strong>401 Unauthorized:</strong> Missing or invalid JWT token</li>
    <li><strong>403 Forbidden:</strong> Blocked account or insufficient permissions (admin required)</li>
    <li><strong>404 Not Found:</strong> User, QR code, or resource doesn't exist</li>
    <li><strong>400 Bad Request:</strong> Invalid input, expired OTP, password too short, duplicate email</li>
    <li><strong>422 Unprocessable Entity:</strong> Request validation failed (Pydantic)</li>
    <li><strong>500 Internal Server Error:</strong> Database or unexpected errors (logged to console)</li>
</ul>

<hr>

<h2>13. FUTURE ENHANCEMENTS</h2>

<ul>
    <li>Email integration (SMTP) for sending actual OTP emails instead of terminal output</li>
    <li>Bulk QR code generation from CSV/Excel files</li>
    <li>Custom domain support for tracking URLs</li>
    <li>Advanced analytics with geographic location of scans</li>
    <li>QR code expiration dates and password-protected QR codes</li>
    <li>API rate limiting to prevent abuse</li>
    <li>Two-factor authentication (2FA) for admin accounts</li>
    <li>Export analytics reports as PDF or CSV</li>
    <li>Dynamic QR codes (editable content without changing QR image)</li>
    <li>Team/organization accounts with shared QR code libraries</li>
</ul>

<hr>

<h2>14. CONCLUSION</h2>

<p>QR Studio provides a complete, production-ready solution for creating, managing, and tracking QR codes. The system combines ease of use with powerful features including customizable designs, scan tracking, user management, and administrative analytics. The modular architecture allows for easy extension and maintenance. Built with modern web technologies and following security best practices, QR Studio is suitable for both individual users and business environments requiring professional QR code management with tracking capabilities.</p>

<p>The application successfully demonstrates integration of FastAPI backend with responsive frontend, JWT authentication, OTP verification, real-time QR generation, database operations, and role-based access control. All core features are fully functional and tested.</p>

<hr>

<h3 align="center">END OF DOCUMENTATION</h3>

</body>
</html>
