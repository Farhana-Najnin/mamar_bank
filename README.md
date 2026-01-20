# 🏦 Mamar Bank – Banking Web Application (Django)

A backend-focused **banking system simulation** built with Django that models real-world financial workflows such as account management, deposits, withdrawals, loans, money transfer, transaction reporting, and email notifications.

This project demonstrates **clean backend architecture**, **secure money handling logic**, and proper use of **Django’s MVT pattern**, aligned with enterprise and fintech development practices.

---

## 🎯 Project Objective

Traditional academic projects often focus only on CRUD operations.  
**Mamar Bank** goes a step further by simulating **real banking behavior**, including:

- Balance validation
- Transaction history tracking
- Loan request & repayment workflow
- Secure money transfer between users
- Audit-friendly transaction records
- Email notifications for financial activities

---

## 🧱 Architecture Overview (Django MVT)

This project follows **Django’s Model–View–Template (MVT)** architecture:

- **Model** → Database schema & business data
- **View** → Core business logic and workflows
- **Template** → User-facing UI (HTML)

Each responsibility is clearly separated for maintainability and scalability.

---

## 🗂 Application Structure

```
mamar_bank/
├── core/ # Landing page (HomeView)
├── accounts/ # Authentication, user profile, bank account & address
├── transactions/ # Deposit, Withdraw, Loan, Transfer, Reports
├── mamar_bank/ # Project settings, URLs, WSGI/ASGI
└── manage.py
```

---

## 👤 Accounts Module

### Features
- User registration using Django’s built-in authentication system
- Automatic creation of:
  - **Bank Account** (unique account number)
  - **User Address**
- Login / Logout
- Profile update (User + Account + Address)
- Secure password change (session preserved)

### Key Design Choices
- `OneToOneField` between `User` and `UserBankAccount`
- `DecimalField` used for monetary values to avoid floating-point errors
- Server-side validation using Django Forms

---

## 💳 Transactions Module

### Supported Banking Operations
- **Deposit**
  - Minimum deposit validation
  - Balance update + transaction logging
- **Withdraw**
  - Minimum & maximum limits
  - Insufficient balance protection
- **Loan Request**
  - Loan request tracking
  - Approved-loan count limitation
- **Loan Payment**
  - Balance validation before repayment
- **Money Transfer**
  - Transfer by account number
  - Sender and receiver transaction records created
- **Transaction Report**
  - Full transaction history
  - Optional date-range filtering

### Transaction Design
Each financial action creates a **Transaction record** containing:
- Transaction type (Deposit, Withdraw, Loan, Transfer, etc.)
- Amount
- Timestamp
- Balance snapshot after transaction

This enables **auditability** and **financial traceability**.

---

## 📧 Email Notification System

- Email alerts sent for:
  - Deposits
  - Withdrawals
  - Loan requests / approvals
  - Transfers
- SMTP configured using environment variables
- Credentials stored securely using `.env`

---

## ⚙️ Technology Stack

**Backend**
- Python
- Django
- Django ORM
- Django Authentication System

**Database**
- SQLite (development)
- PostgreSQL-ready configuration (production)

**Frontend**
- Django Templates
- Bootstrap (via Crispy Forms)

**Other Tools**
- Django Messages Framework
- Email (SMTP)
- Environment variables using `django-environ`

---

## 🔐 Security & Best Practices

- Secrets managed via environment variables (`.env`)
- CSRF protection enabled
- Login-protected transaction routes
- Server-side validation for all financial operations
- Separation of concerns across apps

> ⚠️ For production:
> - `DEBUG = False`
> - Restrict `ALLOWED_HOSTS`
> - Use PostgreSQL
> - Use atomic database transactions for money transfer

---

## ▶️ Run Locally




### 1️⃣ Create virtual environment
```bash
python -m venv venv
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

### 2️⃣ Activate environment
# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate

### 3️⃣ Install dependencies
pip install -r requirements.txt

### 4️⃣ Configure environment variables

Create a .env file:

SECRET_KEY=your_secret_key
EMAIL_HOST_USER=your_email@gmail.com
EMAIL_HOST_PASSWORD=your_app_password

### 5️⃣ Apply migrations
python manage.py makemigrations
python manage.py migrate


### 6️⃣ Create admin user (optional)
python manage.py createsuperuser


### 7️⃣ Run server
python manage.py runserver


Access:

Home → http://127.0.0.1:8000/

Admin → http://127.0.0.1:8000/admin/
```
---


