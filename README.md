<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Studio | Complete Project Documentation</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #020617 100%);
            color: #e2e8f0;
            line-height: 1.6;
            padding: 40px 20px;
        }

        .doc-container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(15, 23, 42, 0.75);
            backdrop-filter: blur(10px);
            border-radius: 32px;
            border: 1px solid rgba(79, 70, 229, 0.3);
            overflow: hidden;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
        }

        /* Header Section */
        .doc-header {
            background: linear-gradient(135deg, rgba(79, 70, 229, 0.3), rgba(99, 102, 241, 0.1));
            padding: 48px 40px;
            text-align: center;
            border-bottom: 1px solid rgba(79, 70, 229, 0.4);
        }

        .logo-badge {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, #4f46e5, #818cf8);
            border-radius: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 20px;
            font-size: 40px;
            box-shadow: 0 10px 25px -5px rgba(79, 70, 229, 0.5);
        }

        .doc-header h1 {
            font-size: 42px;
            font-weight: 800;
            background: linear-gradient(135deg, #fff, #a5b4fc);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 12px;
        }

        .doc-header .subtitle {
            font-size: 18px;
            color: #94a3b8;
            letter-spacing: 0.5px;
        }

        .version-badge {
            display: inline-block;
            background: rgba(16, 185, 129, 0.2);
            border: 1px solid #10b981;
            padding: 6px 16px;
            border-radius: 40px;
            font-size: 12px;
            font-weight: 600;
            margin-top: 16px;
            color: #6ee7b7;
        }

        /* Content Body */
        .doc-body {
            padding: 40px;
        }

        /* Section Styles */
        .section {
            margin-bottom: 48px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 24px;
            padding: 28px 32px;
            border: 1px solid rgba(148, 163, 184, 0.15);
            transition: transform 0.2s, border-color 0.2s;
        }

        .section:hover {
            border-color: rgba(79, 70, 229, 0.4);
        }

        .section-title {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 24px;
            display: flex;
            align-items: center;
            gap: 12px;
            color: #c7d2fe;
            border-left: 4px solid #4f46e5;
            padding-left: 18px;
        }

        .section-title i {
            font-size: 28px;
            color: #818cf8;
        }

        .section-subtitle {
            font-size: 18px;
            font-weight: 600;
            margin: 20px 0 12px 0;
            color: #a5b4fc;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Cards Grid */
        .feature-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 24px;
            margin-top: 20px;
        }

        .feature-card {
            background: rgba(15, 23, 42, 0.6);
            border-radius: 20px;
            padding: 22px;
            border: 1px solid rgba(148, 163, 184, 0.2);
            transition: all 0.25s;
        }

        .feature-card:hover {
            transform: translateY(-4px);
            background: rgba(79, 70, 229, 0.1);
            border-color: #4f46e5;
        }

        .feature-icon {
            width: 48px;
            height: 48px;
            background: linear-gradient(135deg, #4f46e5, #6366f1);
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            margin-bottom: 16px;
        }

        .feature-card h3 {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 10px;
            color: #e2e8f0;
        }

        .feature-card p {
            font-size: 14px;
            color: #94a3b8;
            line-height: 1.5;
        }

        /* Tech Stack Pills */
        .tech-stack {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin: 20px 0;
        }

        .tech-pill {
            background: rgba(79, 70, 229, 0.2);
            border: 1px solid rgba(129, 140, 248, 0.5);
            padding: 8px 18px;
            border-radius: 40px;
            font-size: 13px;
            font-weight: 500;
            color: #c7d2fe;
        }

        .tech-pill i {
            margin-right: 8px;
            color: #818cf8;
        }

        /* Workflow Steps */
        .workflow-step {
            display: flex;
            gap: 20px;
            margin-bottom: 24px;
            align-items: flex-start;
        }

        .step-number {
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, #4f46e5, #6366f1);
            border-radius: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            font-size: 18px;
            flex-shrink: 0;
        }

        .step-content {
            flex: 1;
        }

        .step-content h4 {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 6px;
            color: #cbd5e1;
        }

        .step-content p {
            font-size: 14px;
            color: #94a3b8;
        }

        /* Table Styles */
        .table-wrapper {
            overflow-x: auto;
            margin: 20px 0;
        }

        .data-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 13px;
        }

        .data-table th {
            text-align: left;
            padding: 12px 16px;
            background: rgba(79, 70, 229, 0.2);
            color: #a5b4fc;
            font-weight: 600;
            border-radius: 12px 12px 0 0;
        }

        .data-table td {
            padding: 12px 16px;
            border-bottom: 1px solid rgba(148, 163, 184, 0.2);
            color: #cbd5e1;
        }

        .data-table tr:hover td {
            background: rgba(79, 70, 229, 0.05);
        }

        /* Code Block */
        .code-block {
            background: #0a0f1c;
            border-radius: 16px;
            padding: 16px 20px;
            font-family: 'Courier New', monospace;
            font-size: 13px;
            overflow-x: auto;
            border: 1px solid rgba(79, 70, 229, 0.3);
            margin: 16px 0;
            color: #a5b4fc;
        }

        /* Footer */
        .doc-footer {
            background: rgba(0, 0, 0, 0.4);
            padding: 28px 40px;
            text-align: center;
            border-top: 1px solid rgba(148, 163, 184, 0.2);
            font-size: 13px;
            color: #64748b;
        }

        hr {
            border: none;
            height: 1px;
            background: linear-gradient(90deg, transparent, #4f46e5, transparent);
            margin: 24px 0;
        }

        .badge {
            background: rgba(245, 158, 11, 0.2);
            color: #fbbf24;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 600;
            display: inline-block;
        }

        @media (max-width: 768px) {
            .doc-body {
                padding: 24px;
            }
            .section {
                padding: 20px;
            }
            .doc-header h1 {
                font-size: 28px;
            }
        }
    </style>
</head>
<body>
<div class="doc-container">
    <!-- Header -->
    <div class="doc-header">
        <div class="logo-badge">
            <i class="fas fa-qrcode"></i>
        </div>
        <h1>QR Studio</h1>
        <div class="subtitle">Complete QR Code Management System</div>
        <div class="version-badge">
            <i class="fas fa-code-branch"></i> Version 1.0 | Production Ready
        </div>
    </div>

    <!-- Main Content -->
    <div class="doc-body">
        
        <!-- ABSTRACT Section -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-feather-alt"></i>
                <span>Abstract</span>
            </div>
            <p style="font-size: 16px; margin-bottom: 16px;">
                <strong>QR Studio</strong> is a complete, production-ready web application that empowers users to create, customize, 
                and manage professional QR codes with advanced tracking capabilities. The system combines a secure authentication 
                engine, real-time QR generation, multi-format support (text, links, Wi-Fi, email, SMS, vCard), color customization, 
                logo overlays, and a powerful admin dashboard with live analytics. Built with <strong>FastAPI</strong> (Python) 
                on the backend and responsive <strong>HTML/CSS/JavaScript</strong> on the frontend, QR Studio delivers a seamless 
                experience for both regular users and administrators.
            </p>
            <div class="tech-stack">
                <span class="tech-pill"><i class="fab fa-python"></i> FastAPI</span>
                <span class="tech-pill"><i class="fas fa-database"></i> SQLite</span>
                <span class="tech-pill"><i class="fas fa-lock"></i> JWT + bcrypt</span>
                <span class="tech-pill"><i class="fab fa-js"></i> JavaScript (Vanilla)</span>
                <span class="tech-pill"><i class="fas fa-qrcode"></i> QRCode.js</span>
                <span class="tech-pill"><i class="fas fa-chart-line"></i> Chart.js</span>
            </div>
        </div>

        <!-- KEY FEATURES Section -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-star-of-life"></i>
                <span>Key Features</span>
            </div>
            <div class="feature-grid">
                <div class="feature-card">
                    <div class="feature-icon"><i class="fas fa-shield-alt"></i></div>
                    <h3>Secure Authentication</h3>
                    <p>Email + OTP verification, bcrypt password hashing, JWT tokens with 7-day expiry, and account blocking system.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon"><i class="fas fa-palette"></i></div>
                    <h3>QR Customization</h3>
                    <p>Change colors, size (200-500px), margin, error correction (L/M/Q/H), and add logo overlay with adjustable size.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon"><i class="fas fa-chart-simple"></i></div>
                    <h3>Scan Tracking</h3>
                    <p>Every saved QR gets a unique trackable URL. Record IP, user agent, timestamp, and view scan counts.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon"><i class="fas fa-crown"></i></div>
                    <h3>Admin Dashboard</h3>
                    <p>Full user management, block/unblock, role promotion, live charts (user growth, QR types), and system stats.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon"><i class="fas fa-mobile-alt"></i></div>
                    <h3>Responsive Design</h3>
                    <p>Glass-morphism UI, mobile-friendly layouts, live QR preview, and seamless tab navigation.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon"><i class="fas fa-envelope"></i></div>
                    <h3>Multi-Format QR</h3>
                    <p>Text, URL, Wi-Fi credentials, Email (mailto), SMS, and vCard (business card) support.</p>
                </div>
            </div>
        </div>

        <!-- WORKFLOW Section -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-project-diagram"></i>
                <span>Workflow Description</span>
            </div>
            
            <div class="section-subtitle"><i class="fas fa-user-plus"></i> User Registration Flow</div>
            <div class="workflow-step">
                <div class="step-number">1</div>
                <div class="step-content"><h4>Email & Name</h4><p>User enters email and optional full name on signup form.</p></div>
            </div>
            <div class="workflow-step">
                <div class="step-number">2</div>
                <div class="step-content"><h4>OTP Verification</h4><p>System generates 6-digit code (printed in terminal for demo). User enters code within 10 minutes.</p></div>
            </div>
            <div class="workflow-step">
                <div class="step-number">3</div>
                <div class="step-content"><h4>Create Password</h4><p>User sets password (min 6 chars). Account is created with is_verified = 1.</p></div>
            </div>

            <hr>

            <div class="section-subtitle"><i class="fas fa-sign-in-alt"></i> Login & Authentication</div>
            <div class="workflow-step">
                <div class="step-number">1</div>
                <div class="step-content"><h4>Credentials</h4><p>User provides email + password. System verifies against database (bcrypt compare).</p></div>
            </div>
            <div class="workflow-step">
                <div class="step-number">2</div>
                <div class="step-content"><h4>JWT Generation</h4><p>On success, creates JWT token with user email as "sub". Token expires after 7 days.</p></div>
            </div>
            <div class="workflow-step">
                <div class="step-number">3</div>
                <div class="step-content"><h4>Role Redirect</h4><p>Regular users → dashboard.html ; Admin users → admin.html (full analytics & user mgmt).</p></div>
            </div>

            <hr>

            <div class="section-subtitle"><i class="fas fa-qrcode"></i> QR Code Creation & Tracking</div>
            <div class="workflow-step">
                <div class="step-number">1</div>
                <div class="step-content"><h4>Select Type & Input</h4><p>Choose from 6 modes (text, link, wifi, email, sms, vcard). Fill in dynamic form fields.</p></div>
            </div>
            <div class="workflow-step">
                <div class="step-number">2</div>
                <div class="step-content"><h4>Live Customization</h4><p>Adjust colors, size, margin, error correction. Add logo image (PNG/JPG) — preview updates instantly.</p></div>
            </div>
            <div class="workflow-step">
                <div class="step-number">3</div>
                <div class="step-content"><h4>Save & Track</h4><p>Save with a title → stored in database. Each QR gets unique scan endpoint (/scan/{id}). Every scan increments counter and logs IP/user-agent.</p></div>
            </div>
        </div>

        <!-- DATABASE SCHEMA Section -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-database"></i>
                <span>Database Schema (SQLite)</span>
            </div>
            <div class="table-wrapper">
                <table class="data-table">
                    <thead>
                        <tr><th>Table</th><th>Key Columns</th><th>Purpose</th></tr>
                    </thead>
                    <tbody>
                        <tr><td><span class="badge">users</span></td><td>id, email, hashed_password, is_admin, is_blocked</td><td>Stores account info, roles, block status</td></tr>
                        <tr><td><span class="badge">otp_codes</span></td><td>email, code, purpose, expires_at, is_used</td><td>Email verification & password reset OTPs</td></tr>
                        <tr><td><span class="badge">qrcodes</span></td><td>user_id, title, content, qr_type, scan_count</td><td>Saved QR codes with metadata & scan stats</td></tr>
                        <tr><td><span class="badge">qr_scans</span></td><td>qr_id, scanned_at, ip_address, user_agent</td><td>Individual scan logs for tracking</td></tr>
                        <tr><td><span class="badge">user_activity</span></td><td>user_id, action, details, created_at</td><td>Audit log (login, create QR, admin actions)</td></tr>
                    </tbody>
                </table>
            </div>
            <p style="font-size: 13px; color: #94a3b8; margin-top: 12px;"><i class="fas fa-info-circle"></i> All timestamps stored in ISO format (UTC). Foreign keys ensure data integrity on delete cascade.</p>
        </div>

        <!-- API ENDPOINTS Section -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-plug"></i>
                <span>REST API Endpoints</span>
            </div>
            <div class="feature-grid" style="grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));">
                <div class="feature-card">
                    <h3><i class="fas fa-key"></i> Auth</h3>
                    <p style="font-family: monospace;">POST /auth/request-verify-otp<br>POST /auth/verify-email<br>POST /auth/register<br>POST /auth/login<br>POST /auth/forgot-password<br>POST /auth/reset-password</p>
                </div>
                <div class="feature-card">
                    <h3><i class="fas fa-user"></i> User</h3>
                    <p style="font-family: monospace;">GET /users/me</p>
                </div>
                <div class="feature-card">
                    <h3><i class="fas fa-qrcode"></i> QR Codes</h3>
                    <p style="font-family: monospace;">POST /qrcodes<br>GET /qrcodes<br>DELETE /qrcodes/{id}<br>GET /scan/{id} (public tracking)</p>
                </div>
                <div class="feature-card">
                    <h3><i class="fas fa-chart-line"></i> Admin</h3>
                    <p style="font-family: monospace;">GET /admin/stats<br>GET /admin/users<br>POST /admin/users/{id}/block<br>POST /admin/users/{id}/unblock<br>POST /admin/users/{id}/make-admin<br>DELETE /admin/users/{id}</p>
                </div>
            </div>
        </div>

        <!-- SECURITY FEATURES Section -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-shield-virus"></i>
                <span>Security Implementation</span>
            </div>
            <div class="feature-grid">
                <div class="feature-card"><i class="fas fa-lock" style="color: #10b981; margin-right: 8px;"></i> <strong>bcrypt Hashing</strong><br>Passwords never stored in plain text.</div>
                <div class="feature-card"><i class="fas fa-hourglass-half" style="color: #f59e0b;"></i> <strong>JWT Expiry (7 days)</strong><br>Automatic session timeout.</div>
                <div class="feature-card"><i class="fas fa-clock" style="color: #f97316;"></i> <strong>OTP Validity (10 min)</strong><br>One-time use, expires quickly.</div>
                <div class="feature-card"><i class="fas fa-ban" style="color: #ef4444;"></i> <strong>Account Blocking</strong><br>Admins can block malicious users.</div>
                <div class="feature-card"><i class="fas fa-user-shield"></i> <strong>Role-Based Access</strong><br>Admin-only endpoints protected.</div>
                <div class="feature-card"><i class="fas fa-eye-slash"></i> <strong>Self-protection</strong><br>Cannot block/delete own admin account.</div>
            </div>
        </div>

        <!-- DEPLOYMENT Section -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-rocket"></i>
                <span>Deployment Instructions</span>
            </div>
            <div class="code-block">
                <i class="fas fa-terminal"></i> # Step 1: Install dependencies<br>
                pip install -r requirements.txt<br><br>
                # Step 2: Run the FastAPI server<br>
                uvicorn app.main:app --reload --host 127.0.0.1 --port 8000<br><br>
                # Step 3: Open browser → http://127.0.0.1:8000
            </div>
            <p><strong>Default Admin Credentials:</strong> <span class="badge" style="background: #4f46e5; color: white;">admin@qrstudio.com</span> / <span class="badge" style="background: #10b981;">Admin123!</span></p>
            <div class="code-block" style="margin-top: 16px;">
                <i class="fas fa-folder-tree"></i> Project Structure:<br>
                qr-studio/<br>
                ├── app/ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# (main.py, auth.py, database.py, models.py)<br>
                ├── frontend/ &nbsp;&nbsp;# (index.html, dashboard.html, admin.html, auth.js, style.css)<br>
                ├── qrstudio.db &nbsp;# auto-created SQLite database<br>
                └── requirements.txt
            </div>
        </div>

        <!-- TECHNOLOGY STACK Detailed -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-code"></i>
                <span>Technology Stack</span>
            </div>
            <div class="feature-grid">
                <div class="feature-card"><i class="fab fa-python"></i> <strong>Backend</strong><br>Python 3.8+, FastAPI, Uvicorn, SQLite3, python-jose, passlib[bcrypt], Pydantic</div>
                <div class="feature-card"><i class="fab fa-html5"></i> <strong>Frontend</strong><br>HTML5, CSS3 (Flexbox/Grid, backdrop-filter), Vanilla JS, QRCode.js, Chart.js, Font Awesome, Google Fonts (Inter)</div>
                <div class="feature-card"><i class="fas fa-shield-alt"></i> <strong>Auth & Security</strong><br>JWT (HS256), bcrypt rounds, OTP 6-digit, SQLite row-level constraints</div>
            </div>
        </div>

        <!-- CONCLUSION -->
        <div class="section">
            <div class="section-title">
                <i class="fas fa-check-circle"></i>
                <span>Conclusion</span>
            </div>
            <p style="font-size: 15px;">QR Studio delivers a full-featured, secure, and visually modern QR code management ecosystem. From user registration with OTP to real-time QR customization and admin analytics, every component is designed for real-world usage. The modular architecture (FastAPI backend + lightweight frontend) ensures easy maintenance and scalability. Whether for personal branding, business campaigns, or event management, QR Studio provides professional-grade tools to create trackable, beautiful QR codes with full control over user roles and system analytics.</p>
            <div style="margin-top: 24px; padding: 16px; background: rgba(79, 70, 229, 0.1); border-radius: 20px; text-align: center;">
                <i class="fas fa-heart" style="color: #f43f5e;"></i> Built with modern best practices | Ready for production | Fully documented
            </div>
        </div>
    </div>

    <!-- Footer -->
    <div class="doc-footer">
        <i class="fas fa-copyright"></i> QR Studio Project Documentation | Complete QR Management System | FastAPI + SQLite + JWT
    </div>
</div>
</body>
</html>
