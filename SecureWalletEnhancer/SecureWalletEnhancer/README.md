# Crypto Wallet Simulation - CY4053 Secure FinTech Application

A comprehensive Flask-based secure crypto wallet application demonstrating advanced cybersecurity concepts for academic purposes.

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Flask](https://img.shields.io/badge/Flask-3.0.0-green)
![Security](https://img.shields.io/badge/Security-bcrypt%20%2B%20Fernet-red)

## 🎯 Project Overview

This is a fully functional secure crypto wallet simulation built for the CY4053 Secure FinTech Application assignment. The application demonstrates all major cybersecurity principles including authentication, authorization, encryption, input validation, session management, audit logging, and secure error handling.

## ✨ Key Features

### 🔐 Security Features
- **bcrypt Password Hashing**: All passwords are securely hashed before storage
- **Fernet Encryption**: Wallet balances and transactions are encrypted in the database
- **SQL Injection Prevention**: Comprehensive input sanitization
- **XSS Attack Prevention**: HTML entity encoding and pattern detection
- **Session Management**: 5-minute automatic logout for inactive users
- **Audit Logging**: Complete activity trail in `audit_log.txt`
- **File Upload Validation**: Only .jpg and .png files allowed
- **Error Handling**: Generic user-friendly messages (no stack traces)

### 💼 Application Features
- User registration with password strength validation
- Secure login/logout system
- Wallet dashboard with encrypted balance display
- Decrypt balance feature (temporary 5-second view)
- Add funds functionality with validation
- Withdraw funds with balance checking
- Profile management with email and display name
- Profile picture upload (secure file handling)
- Recent transactions history
- Personal audit log viewer

## 🛠️ Technical Stack

- **Backend**: Flask 3.0.0
- **Security**: bcrypt 4.1.2, cryptography 41.0.7
- **Database**: SQLite3
- **Frontend**: Bootstrap 5.3.0, Font Awesome 6.4.0
- **Templating**: Jinja2
- **File Handling**: Werkzeug 3.0.1

## 📁 Project Structure

```
/
├── main.py                 # Main Flask application
├── requirements.txt        # Python dependencies
├── README.md              # This file
├── TEST_CASES.md          # 40+ security test cases
├── replit.md              # Project documentation
├── .gitignore             # Git ignore rules
├── encryption.key         # Fernet encryption key (auto-generated)
├── wallet.db              # SQLite database (auto-created)
├── audit_log.txt          # Activity logs (auto-created)
├── templates/             # HTML templates
│   ├── login.html
│   ├── register.html
│   ├── dashboard.html
│   ├── add_funds.html
│   ├── withdraw.html
│   ├── profile.html
│   └── error.html
├── static/                # Static assets
│   └── style.css
└── uploads/               # User profile pictures (auto-created)
```

## 🚀 Getting Started

### Prerequisites
- Python 3.11 or higher
- pip (Python package manager)

### Installation

1. **Clone or download this project**

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the application**:
   ```bash
   python main.py
   ```

4. **Access the application**:
   - Open your browser and navigate to `http://localhost:5000`
   - Or use the Replit webview

## 📝 Usage Guide

### First Time Setup

1. **Register an Account**:
   - Click "Register here" on the login page
   - Enter a username (min 3 characters)
   - Enter an email (optional)
   - Create a strong password meeting these requirements:
     - Minimum 8 characters
     - At least one uppercase letter
     - At least one lowercase letter
     - At least one digit
     - At least one special character (!@#$%^&*...)

2. **Login**:
   - Enter your username and password
   - You'll be redirected to the dashboard

### Using the Wallet

1. **Add Funds**:
   - Click "Add Funds" on the dashboard
   - Enter a numeric amount (e.g., 1000)
   - Click "Add Funds"

2. **Withdraw Funds**:
   - Click "Withdraw" on the dashboard
   - Enter amount (must be ≤ current balance)
   - Click "Withdraw Funds"

3. **View Balance**:
   - Balance is encrypted by default (shown as ********)
   - Click "Decrypt Balance" to view for 5 seconds
   - Balance automatically re-encrypts

4. **Update Profile**:
   - Click "Profile" in navigation
   - Update email or display name
   - Upload profile picture (.jpg or .png only)
   - Click "Update Profile"

## 🧪 Security Testing

This application includes **40+ comprehensive test cases** covering:

1. Password strength validation
2. SQL injection prevention
3. XSS attack prevention
4. Input validation
5. Session management
6. Authentication & authorization
7. Data encryption
8. Audit logging
9. File upload security
10. Error handling

**See TEST_CASES.md for complete testing guide with step-by-step instructions.**

## 🔒 Password Requirements

Passwords must meet ALL of these criteria:
- ✅ Minimum 8 characters
- ✅ At least one uppercase letter (A-Z)
- ✅ At least one lowercase letter (a-z)
- ✅ At least one digit (0-9)
- ✅ At least one special character (!@#$%^&*(),.?":{}|<>)

**Examples of Valid Passwords**:
- `SecurePass123!`
- `MyWallet2024@`
- `Crypto$ecure99`

## 📊 Database Schema

### Users Table
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    email TEXT,
    display_name TEXT,
    balance TEXT NOT NULL,           -- Encrypted with Fernet
    profile_picture TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Transactions Table
```sql
CREATE TABLE transactions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    transaction_type TEXT NOT NULL,  -- 'Deposit' or 'Withdrawal'
    amount TEXT NOT NULL,            -- Encrypted with Fernet
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users (id)
);
```

## 🛡️ Security Implementation Details

### 1. Password Security
- **Hashing Algorithm**: bcrypt with automatic salt generation
- **Work Factor**: Default bcrypt rounds (computationally expensive)
- **Storage**: Only hashed passwords stored, never plain text

### 2. Data Encryption
- **Algorithm**: Fernet (symmetric encryption)
- **Key Management**: Persistent key stored in `encryption.key` file
- **Encrypted Fields**: Balance, transaction amounts
- **Key Persistence**: Same key used across restarts

### 3. Input Validation
```python
# Dangerous patterns blocked:
- <script>, </script>
- javascript:
- ' OR 1=1, " OR 1=1
- --, ;--
- DROP TABLE, INSERT INTO, DELETE FROM
- UPDATE...SET
```

### 4. Session Security
- **Session Type**: Server-side Flask sessions
- **Timeout**: 5 minutes of inactivity
- **Activity Tracking**: Last activity timestamp checked on each request
- **Auto-logout**: Automatic session clear after timeout

### 5. File Upload Security
- **Allowed Extensions**: .jpg, .jpeg, .png only
- **Size Limit**: 2MB maximum
- **Filename Sanitization**: Werkzeug secure_filename()
- **Storage**: Separate uploads directory
- **Naming**: Unique timestamps + user ID prefix

## 📈 Audit Logging

All user activities are logged to `audit_log.txt`:

**Format**: `[YYYY-MM-DD HH:MM:SS] User: username | Action: action_description`

**Logged Actions**:
- User registration
- Successful login
- Failed login attempts
- Logout
- Funds added
- Funds withdrawn
- Profile updates
- Balance decryption
- Security threat blocks

**Example Log Entry**:
```
[2025-10-31 15:00:00] User: secureuser | Action: Logged in successfully
[2025-10-31 15:01:23] User: secureuser | Action: Added funds: 1000.00 crypto
[2025-10-31 15:02:45] User: secureuser | Action: Withdrew funds: 250.00 crypto
```

## ⚠️ Important Notes

### Critical Files (DO NOT DELETE)
- `encryption.key` - Contains Fernet key for decrypting all balances/transactions
- `wallet.db` - Contains all user data and transactions
- `audit_log.txt` - Complete activity history

### For Production Use
This is an educational demonstration. For production deployment:
1. Use environment variables for encryption key
2. Use PostgreSQL instead of SQLite
3. Implement HTTPS/SSL
4. Add rate limiting
5. Use production WSGI server (Gunicorn)
6. Implement proper backup strategies
7. Add email verification
8. Implement two-factor authentication

## 📚 Documentation Files

- **README.md** (this file): Project overview and usage guide
- **TEST_CASES.md**: 40+ security test cases with screenshots guide
- **replit.md**: Detailed project architecture and implementation notes

## 🎓 Assignment Deliverables

This project fulfills all CY4053 assignment requirements:

✅ User registration & login with bcrypt  
✅ Password strength validation  
✅ Duplicate username prevention  
✅ Secure session management  
✅ Dashboard with encrypted balance  
✅ Recent transactions display  
✅ Audit log viewer  
✅ Add/withdraw funds with validation  
✅ SQL injection prevention  
✅ XSS attack prevention  
✅ Fernet encryption for sensitive data  
✅ Decrypt balance feature  
✅ Profile update functionality  
✅ Comprehensive error handling  
✅ Activity audit logging  
✅ 5-minute session expiry  
✅ File upload validation  
✅ 20+ manual test cases documented  

## 🐛 Troubleshooting

### Application won't start
- Ensure Python 3.11+ is installed
- Install all dependencies: `pip install -r requirements.txt`
- Check if port 5000 is available

### Can't decrypt balance
- Ensure `encryption.key` file exists
- Don't delete or modify `encryption.key`
- Check browser console for errors

### Session expires too quickly
- Default timeout is 5 minutes (as per requirements)
- This is intentional for security demonstration
- Can be adjusted in main.py if needed for testing

### Database errors
- Delete `wallet.db` to reset database
- Note: This will delete all users and transactions
- Encryption key will remain valid

## 👨‍💻 Development

### Code Structure
- **main.py**: Single-file Flask application (550+ lines)
- **Clean separation**: Database, encryption, validation, routes
- **Security-first design**: Input validation on all user inputs
- **Error handling**: Try-except blocks throughout

### Adding New Features
1. Follow existing code patterns
2. Add input validation for all user inputs
3. Update audit logging for new actions
4. Add test cases to TEST_CASES.md
5. Update documentation

## 📄 License

This project is created for educational purposes as part of the CY4053 course assignment.

## 🤝 Support

For questions or issues:
1. Review TEST_CASES.md for testing guidance
2. Check replit.md for architecture details
3. Review code comments in main.py

## 📸 Screenshots

Take screenshots of:
- Login page
- Registration with password validation
- Dashboard with encrypted balance
- Add/withdraw funds
- Profile page
- Audit logs
- Error messages for test cases

---

**Built with ❤️ for CY4053 Secure FinTech Application Assignment**

*Demonstrating comprehensive cybersecurity principles in a practical FinTech application*
