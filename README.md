# InventStor - Storeroom & Inventory Management System

**Sistem Pengurusan Bilik Stor dan Inventori**

A full-stack web-based inventory management system designed for government storeroom operations. Streamlines the complete lifecycle of inventory requests, approvals, stock tracking, and reporting with real-time notifications and audit trails.

> **Template Version** - This is a generic, customizable version. Configure your organization name, department, and branding via the `.env` file.

---

## Features

### Staff Portal
- **KEW.PS-8 Request System** - Browse product catalog with dual-row filters (category + brand), add items to cart, submit formal requests
- **Request Tracking** - View status, edit pending requests, receive approval/rejection updates
- **Bidirectional Remarks** - Two-way communication between staff and admin on each request
- **Smart Autocomplete** - Position field auto-suggests based on profile and history
- **Session Auto-save** - Form state persists across page navigations

### Admin Portal
- **Interactive Dashboard** - Real-time stat cards, Chart.js visualizations, quick action modals
- **Request Management** - Review, approve/reject with remarks, set approved quantities
- **Inventory CRUD** - Full product management with photo uploads and shared photo support
- **Hierarchical Categories** - Parent-child subcategory system with independent brand filtering on browse page
- **Manual Stock Adjustments** - Restock and correction entries with full audit logging
- **User & Department Management** - Staff accounts, roles, and organizational units
- **Advanced Reporting** - Department analytics, inventory reports with organization letterhead, KEW.PS-3 stock cards
- **Sortable Tables** - Click any column header to sort data

### Security
- Password hashing (bcrypt via `password_hash()`)
- CSRF token protection on all forms
- Content Security Policy (CSP) headers
- XSS prevention via output encoding
- SQL injection prevention (prepared statements)
- Session timeout with idle detection (30-minute server + client-side warning)
- Rate-limited login (5 attempts per 15 minutes)
- Role-based access control (Admin vs Staff)
- Secure session cookies (httpOnly, sameSite, secure)
- Self-approval prevention
- Row-level locking for concurrent stock updates

### Integrations
- **Telegram Bot** - Real-time notifications for new requests and low stock alerts
- **SweetAlert2** - Modern confirmation dialogs and toast notifications
- **Chart.js** - Dashboard charts for stock distribution and request trends

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | PHP 8.x |
| **Database** | MySQL / MariaDB |
| **Frontend** | Bootstrap 5.3, vanilla JavaScript, AJAX |
| **Charts** | Chart.js |
| **Icons** | Bootstrap Icons 1.11 |
| **Notifications** | Telegram Bot API, SweetAlert2 |
| **Authentication** | Session-based with bcrypt password hashing |

---

## Project Structure

```
inventstor/
├── admin_*.php              # Admin pages and process handlers
├── staff_*.php              # Staff portal pages
├── kewps8_*.php             # KEW.PS-8 request workflow (browse, form, process)
├── kewps3_*.php             # KEW.PS-3 stock card reports
├── report_*.php             # Reporting and analytics
├── request_*.php            # Request review, edit, delete
├── login.php                # Authentication entry point
├── auth_check.php           # Session validation + timeout guard
├── admin_auth_check.php     # Admin role gate
├── config.php               # Environment config loader + org helpers
├── db.php                   # Database connection
├── csrf.php                 # CSRF token utilities
├── telegram_helper.php      # Telegram notification functions
├── .env.example             # Environment config template
├── assets/
│   ├── css/                 # Stylesheets
│   └── img/                 # Logos, favicons (replace with your own)
├── uploads/
│   ├── profile_pictures/    # Staff profile photos
│   └── product_images/      # Product photos
└── md files/                # Internal documentation
```

---

## Installation

### Prerequisites
- PHP 8.0+
- MySQL 5.7+ or MariaDB 10.3+
- Apache or Nginx
- Telegram Bot Token (optional, for notifications)

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/gible717/InventStor.git
   cd InventStor
   ```

2. **Configure environment**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` with your settings:
   ```env
   # Organization Branding
   ORG_NAME="Your Organization Name"
   ORG_UNIT="Your Department / Unit"
   ORG_SHORT_NAME="ORG"

   # Database
   DB_HOST=localhost
   DB_USERNAME=root
   DB_PASSWORD=your_password
   DB_NAME=storeroom_db

   # Telegram (optional)
   TELEGRAM_BOT_TOKEN=your_bot_token
   TELEGRAM_ADMIN_CHAT_IDS=123456789

   # System URL
   SYSTEM_BASE_URL=http://localhost/inventstor
   ```

3. **Create the database**
   Import via command line:
   ```bash
   mysql -u root < database/schema.sql
   mysql -u root storeroom_db < database/seed_data.sql
   ```
   Or import both files via phpMyAdmin (Import tab).

4. **Replace branding assets** in `assets/img/`:
   - `logo.png` - Your organization logo (used in reports and login)
   - `admin-logo.png` - Welcome page logo
   - `favicon-32.png` - Browser tab icon
   - `favicon-180.png` - Apple touch icon
   - `login-bg.jpg` - Login page background
   - `background1.jpg`, `background2.jpg`, `background3.jpg` - Welcome page slideshow

5. **Configure Telegram** (optional):
   - Create a bot via [@BotFather](https://t.me/botfather)
   - Add your Bot Token and Chat IDs to `.env`

6. **Set upload permissions**
   ```bash
   chmod 755 uploads/
   chmod 755 uploads/profile_pictures/
   chmod 755 uploads/product_images/
   ```

7. **Access the system** at `http://localhost/inventstor/`

### Default Accounts

| Role | ID | Password |
|------|-----|----------|
| Admin | A001 | User123 |
| Staff | S001 | User123 |

> Change these passwords immediately after first login.

---

## Database Schema

7 core tables:

| Table | Purpose |
|-------|---------|
| `staf` | User accounts with roles (Admin/Staff) |
| `jabatan` | Organizational departments |
| `KATEGORI` | Product categories with parent-child hierarchy |
| `barang` | Product/inventory master data |
| `permohonan` | Request headers |
| `permohonan_barang` | Request line items |
| `transaksi_stok` | Stock transaction audit log |

---

## Key Workflows

**Request Flow (Staff)**
```
Browse Catalog  ->  Add to Cart  ->  Submit KEW.PS-8 Form  ->  Telegram Notification  ->  Await Approval
```

**Approval Flow (Admin)**
```
View Pending  ->  Review Details  ->  Set Quantities  ->  Approve/Reject  ->  Stock Auto-Deducted  ->  Transaction Logged
```

**Stock Management (Admin)**
```
Select Product  ->  Enter Adjustment  ->  Submit  ->  Stock Updated  ->  Transaction Logged
```

---

## Reporting

- **Department Analytics** - Top departments by volume, approval rates, monthly trends
- **Inventory Reports** - Stock levels by category, movements (IN/OUT), previous balance, total value, printable with organization letterhead
- **KEW.PS-3 Stock Card** - Individual product transaction history with running balance, date filtering, print-ready format

---

## Language

- **Interface:** Bahasa Malaysia (Malay)
- **Code & Comments:** English
- **Database columns:** English

---

## License

Open-source inventory management template for government and organizational use.

---

*Built with PHP, MySQL, Bootstrap 5, Chart.js, and Telegram Bot API.*
