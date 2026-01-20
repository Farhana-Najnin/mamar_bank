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


---

## 👤 Accounts Module

### Features
- Registration using Django auth
- Auto creation of:
  - **Bank Account** (unique account number)
  - **User Address**
- Login / Logout
- Profile update (User + Account + Address)
- Password change (session preserved)

### Key Implementation Notes
- User ↔ BankAccount uses `OneToOneField` (1 user = 1 account)
- Monetary data uses `DecimalField` for precision (no float issues)
- Profile update uses `get_or_create()` to safely handle missing related records

---

## 💳 Transactions Module

### Supported Operations
- **Deposit**
- **Withdraw**
- **Loan Request**
- **Loan Payment**
- **Money Transfer**
- **Transaction Report** (with optional date range filter)

### Transaction Logging
Each financial operation creates a **Transaction record** storing:
- Type (Deposit, Withdraw, Loan, Transfer, etc.)
- Amount
- Timestamp
- `balance_after_transaction` snapshot (audit-friendly)

---

## ✅ Business Logic Implemented (Core Logic)

This section describes the **actual rules and validations** applied in the system.

### 1) Account Creation Logic
- When a user registers:
  - A `UserBankAccount` is created
  - A `UserAddress` is created
  - Account number is generated programmatically and stored uniquely

### 2) Deposit Logic
- Deposit amount is validated (server-side)
- Minimum deposit rule enforced:
  - **Deposit must be ≥ 100**
- On success:
  - `account.balance += amount`
  - A transaction log is created
  - Email notification is sent to the user

### 3) Withdrawal Logic
- Withdrawal is validated (server-side)
- Rules enforced:
  - **Minimum withdrawal: 500**
  - **Maximum withdrawal: 20,000**
  - **Cannot withdraw more than current balance**
- On success:
  - `account.balance -= amount`
  - A transaction log is created
  - Email notification is sent

### 4) Loan Request Logic
- Loan requests are recorded as transactions
- Loan approval flow includes an approval flag (`loan_approve`)
- Business rule enforced:
  - A user cannot exceed **3 approved loans**
- Email notification is sent for loan request updates

### 5) Loan Payment Logic
- Loan can be paid only if:
  - Loan is approved
  - User has enough balance to repay
- On success:
  - User balance decreases by loan amount
  - Loan transaction status/type is updated for repayment tracking

### 6) Money Transfer Logic
- Transfer is performed using the receiver’s **account number**
- On transfer:
  - Sender balance is decreased
  - Receiver balance is increased
  - Two transaction logs are created:
    - Sender: **TRANSFER**
    - Receiver: **RECEIVED**
- Email notification is sent after transfer

### 7) Transaction Report Logic
- Users can view transaction history
- Report supports:
  - Full history listing
  - Optional filtering by date range (`start_date`, `end_date`)
  - Summarization of total amounts in a date range

---

## 📧 Email Notification System

- SMTP is configured using environment variables (`.env`)
- Transaction events trigger email alerts such as:
  - Deposit confirmation
  - Withdrawal confirmation
  - Loan request/approval updates
  - Transfer confirmation

---

## ⚙️ Technology Stack

**Backend**
- Python, Django
- Django ORM
- Django Authentication

**Database**
- SQLite (development)
- PostgreSQL-ready configuration (production)

**Frontend**
- Django Templates
- Bootstrap (Crispy Forms)

**Other**
- Django Messages
- Email (SMTP)
- Environment variables using `django-environ`

---

## 🔐 Security & Best Practices

- Secrets managed via `.env`
- Server-side validation for all financial actions
- Login protection for transaction operations
- Clear separation of concerns across apps

> Production considerations:
> - Set `DEBUG = False`
> - Restrict `ALLOWED_HOSTS`
> - Use PostgreSQL
> - Use atomic DB transactions for transfers (recommended in fintech)

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


