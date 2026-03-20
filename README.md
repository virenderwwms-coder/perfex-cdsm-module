# 📋 Customer Document Management System (CDSM) - PerfexCRM Module

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/virenderwwms-coder/perfex-cdsm-module/releases)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![PerfexCRM](https://img.shields.io/badge/PerfexCRM-2.0%2B-brightgreen.svg)](https://perfexcrm.com)
[![PHP](https://img.shields.io/badge/PHP-5.6%2B-blue.svg)](https://www.php.net)
[![MySQL](https://img.shields.io/badge/MySQL-5.6%2B-blue.svg)](https://www.mysql.com)
[![GitHub](https://img.shields.io/badge/GitHub-virenderwwms--coder-black.svg)](https://github.com/virenderwwms-coder)

> **A comprehensive, feature-rich PerfexCRM module designed to manage customer documents with automatic expiry tracking, smart reminders, and intuitive status monitoring.**

---

## 📑 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage Guide](#usage-guide)
- [Configuration](#configuration)
- [Database Schema](#database-schema)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Support](#support)

---

## 🎯 Overview

**Customer Document Management System (CDSM)** is a powerful PerfexCRM module that streamlines document lifecycle management. Track customer documents, receive automated reminders at critical intervals, and ensure compliance with expiry dates.

### Why CDSM?

✅ **Automated Workflows** - Set and forget reminders  
✅ **Visual Status Tracking** - Color-coded indicators (Green, Yellow, Orange, Red)  
✅ **Multi-level Alerts** - 30, 15, 7 days before expiry + expiry date  
✅ **Email Notifications** - Keep your team and customers informed  
✅ **Dashboard Analytics** - Track documents at a glance  
✅ **Permission-based** - Granular access control  
✅ **Zero Configuration** - Works out of the box  

---

## ✨ Features

### 📄 Document Management
- ✅ **Full CRUD Operations** - Add, edit, view, delete documents
- ✅ **Multiple Document Types** - Passport, License, Insurance, Contract, Certificate, Permit, Visa, Custom
- ✅ **Rich Metadata** - Issue date, expiry date, document type, notes
- ✅ **Customer Linking** - Attach documents to customer profiles
- ✅ **Bulk Management** - Manage multiple documents per customer

### ⏰ Smart Reminder System
- ✅ **Automated Scheduling** - Reminders at 30, 15, 7 days before expiry
- ✅ **Expiry Alerts** - Notification on expiry date
- ✅ **Color-Coded Status**:
  - 🟢 **Green** - Valid (>30 days remaining)
  - 🟡 **Yellow** - Follow-up (15-30 days)
  - 🟠 **Orange** - Expiring Soon (7-15 days)
  - 🔴 **Red** - Nearly Expiring/Expired (<7 days)
- ✅ **Days Counter** - Visual countdown for each document
- ✅ **Pending Tracker** - See all pending actions at a glance

### 📧 Email Notification System
- ✅ **Staff Notifications** - Alert team members about upcoming expirations
- ✅ **Customer Notifications** - Inform customers on critical dates
- ✅ **HTML Email Templates** - Professional, branded emails
- ✅ **SMTP Integration** - Works with any email provider
- ✅ **Email Queue Management** - Track sent communications
- ✅ **Customizable Templates** - Edit templates to match your brand

### 📊 Dashboard & Analytics
- ✅ **Statistics Cards** - Valid, Follow-up, Critical, Expired counts
- ✅ **Priority Queue** - Organized by urgency (30, 15, 7 days, expired)
- ✅ **Comprehensive Table** - All reminders at a glance
- ✅ **Filter & Sort** - Find documents quickly
- ✅ **Visual Hierarchy** - Most critical at top
- ✅ **Quick Actions** - Mark done, view details inline

### 🔒 Security & Permissions
- ✅ **Role-Based Access** - Admin, Staff, Client roles
- ✅ **Permission Checking** - View, Create, Edit, Delete
- ✅ **Activity Logging** - Track all changes
- ✅ **SQL Injection Protection** - Parameterized queries
- ✅ **XSS Prevention** - HTML escaping throughout
- ✅ **CSRF Protection** - Token validation on all forms

### ⚙️ Automation
- ✅ **Cron Job Processing** - Automated reminder dispatch
- ✅ **Email Sending** - Auto-send at configured intervals
- ✅ **Batch Operations** - Process multiple reminders efficiently
- ✅ **Error Handling** - Graceful failure with logging
- ✅ **Cleanup Jobs** - Archive old notifications
- ✅ **Zero Maintenance** - Set and forget

### 🌐 Client Portal
- ✅ **Customer Self-Service** - View own documents
- ✅ **Status Overview** - See expiry status
- ✅ **Document History** - Track all documents
- ✅ **Responsive Design** - Works on all devices

---

## 🚀 Installation

### System Requirements

| Component | Version | Required |
|-----------|---------|----------|
| PerfexCRM | 2.0+ | ✅ Yes |
| PHP | 5.6+ | ✅ Yes |
| MySQL | 5.6+ | ✅ Yes |
| SMTP | Any | ✅ For emails |

### Installation Steps

#### Step 1: Download Module
```bash
# Navigate to PerfexCRM modules directory
cd /path/to/perfex/modules

# Clone the repository
git clone https://github.com/virenderwwms-coder/perfex-cdsm-module.git cdsm

# Or download ZIP and extract to 'cdsm' folder
```

#### Step 2: Set Permissions
```bash
# Set directory permissions
chmod -R 755 cdsm

# Set file permissions
chmod 644 cdsm/*.php
chmod 644 cdsm/controllers/*.php
chmod 644 cdsm/models/*.php
```

#### Step 3: Activate Module
1. Open PerfexCRM Admin Panel
2. Navigate: **Setup → Modules**
3. Find: **Customer Document Management (CDSM)**
4. Click: **Install** button
5. Success! Tables created automatically ✓

#### Step 4: Configure Email (Optional)
1. Go: **Setup → Email Settings**
2. Configure SMTP credentials
3. Click: **Send Test Email**
4. Verify: Email received successfully

#### Step 5: Setup Cron Job
```bash
# Edit crontab
crontab -e

# Add this line (every 2 hours):
0 */2 * * * /usr/bin/php /path/to/perfex/index.php cdsm process_reminders > /dev/null 2>&1

# Save and exit
```

#### Step 6: Configure Settings
1. Go: **CDSM → Settings**
2. Set reminder intervals (default: 30, 15, 7 days)
3. Enable/disable email notifications
4. Click: **Save Settings**

### Verification Checklist
- [ ] Module appears in left menu
- [ ] Can access **CDSM → Documents**
- [ ] Can access **CDSM → Reminders**
- [ ] Can access **CDSM → Settings**
- [ ] Database tables exist (check MySQL)
- [ ] Test email sends successfully
- [ ] Cron job added to server

---

## 📖 Quick Start

### 5-Minute Setup

```bash
# 1. Clone (1 min)
cd /path/to/perfex/modules
git clone https://github.com/virenderwwms-coder/perfex-cdsm-module.git cdsm

# 2. Activate (1 min)
# - Setup → Modules → Install CDSM

# 3. Email Config (1 min)
# - Setup → Email → Configure SMTP
# - CDSM → Settings → Enable Email Reminders

# 4. Cron Job (1 min)
# crontab -e
# 0 */2 * * * /usr/bin/php /path/to/perfex/index.php cdsm process_reminders

# 5. Add Document (1 min)
# - CDSM → Documents → Add Document
# - Fill form and Save
```

**Done!** Your module is ready to use. ✅

---

## 💼 Usage Guide

### For Administrators

#### Adding a Document

1. **Navigate to Documents**
   - Click: **CDSM → Documents** (left menu)
   - Click: **+ Add Document** (blue button)

2. **Fill Document Form**
   - **Customer**: Select from dropdown
   - **Document Name**: e.g., "Passport Renewal"
   - **Document Type**: Select type (Passport, License, etc.)
   - **Issue Date**: When document was issued
   - **Expiry Date**: When document expires
   - **Notes**: Optional additional information

3. **Save Document**
   - Click: **Save** button
   - Reminders created automatically ✓
   - Redirected to documents list

#### Viewing Documents

**Documents List View:**
- Shows all documents with status
- Color-coded status badges
- Days remaining counter
- Quick actions (View, Edit, Delete)

**Document Details:**
1. Click eye icon in document row
2. View complete information
3. See all associated reminders
4. Edit or delete document

#### Managing Reminders

1. **Go to Reminders Dashboard**
   - Click: **CDSM → Reminders**

2. **View Organized Queue**
   - **30 Days Before Expiry** - First alert
   - **15 Days Before Expiry** - Second alert
   - **7 Days Before Expiry** - Third alert
   - **Expired Documents** - Past expiry

3. **Take Action**
   - Click: **Mark Done** to notify staff it's complete
   - Reminder status changes to "Done"
   - Remove from pending queue

4. **View All Reminders**
   - Table shows all reminders
   - Filter by status
   - Sort by date
   - Export if needed

#### Configuration

1. **Go to Settings**
   - Click: **CDSM → Settings**

2. **Configure Intervals**
   - First reminder: (default 30 days)
   - Second reminder: (default 15 days)
   - Third reminder: (default 7 days)

3. **Email Settings**
   - ✓ Enable Email Reminders checkbox
   - Save settings

4. **Cron Job Info**
   - Copy provided cron command
   - Add to server crontab

### For Customers

#### Viewing Your Documents

1. **Login to Client Portal**
   - Enter: Customer credentials

2. **Navigate to CDSM**
   - Click: **CDSM → My Documents** (if available)

3. **View Document Status**
   - Summary cards at top (Valid, Follow-up, Critical, Expired)
   - Document table with all details
   - Color-coded status indicators
   - Days remaining for each document

4. **Take Action**
   - Contact company if renewal needed
   - Prepare documents for expiry
   - Update information before expiry

---

## ⚙️ Configuration

### Reminder Intervals

Configure when reminders should trigger:

```php
// Default settings (edit in CDSM → Settings)
First Reminder:  30 days before expiry
Second Reminder: 15 days before expiry
Third Reminder:  7 days before expiry
Final Alert:     On expiry date
```

### Email Configuration

#### SMTP Setup
1. **Setup → Email Settings**
2. Select: **Protocol: SMTP**
3. Enter SMTP details:
   - **Host**: smtp.yourmailserver.com
   - **Port**: 587 or 465
   - **Encryption**: TLS or SSL
   - **Username**: your@email.com
   - **Password**: your_password

#### Popular SMTP Providers

**Gmail:**
- Host: smtp.gmail.com
- Port: 587
- Use App Password (not regular password)

**Office 365:**
- Host: smtp.office365.com
- Port: 587
- Encryption: TLS

**SendGrid:**
- Host: smtp.sendgrid.net
- Port: 587
- Username: apikey
- Password: YOUR_SENDGRID_API_KEY

### Document Types

Default types included:
- Passport
- ID Proof
- License
- Insurance
- Contract
- Certificate
- Permit
- Visa
- Other

**Add Custom Type** (Edit `models/Cdsm_model.php`):
```php
public function get_document_types()
{
    return array(
        'passport' => 'Passport',
        'custom_type' => 'Your Type',
        // Add more...
    );
}
```

### Cron Job Configuration

#### Every 2 Hours (Recommended)
```bash
0 */2 * * * /usr/bin/php /path/to/perfex/index.php cdsm process_reminders
```

#### Every Hour (High Volume)
```bash
0 * * * * /usr/bin/php /path/to/perfex/index.php cdsm process_reminders
```

#### Every 4 Hours (Low Volume)
```bash
0 */4 * * * /usr/bin/php /path/to/perfex/index.php cdsm process_reminders
```

#### Testing Cron Job
```bash
# Run manually to test
php /path/to/perfex/index.php cdsm process_reminders

# Expected output:
# Starting reminder processing...
# Found X pending reminders
# Reminder processing completed successfully.
```

---

## 🗄️ Database Schema

### tblcdsm_documents

Stores customer documents.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | INT | NO | Primary Key, Auto-increment |
| customer_id | INT | NO | Foreign Key to tblclients |
| document_name | VARCHAR(255) | NO | Name of document |
| document_type | VARCHAR(100) | YES | Type (Passport, License, etc.) |
| file_path | VARCHAR(500) | YES | Path to uploaded file |
| issue_date | DATE | NO | When document was issued |
| expiry_date | DATE | NO | When document expires |
| notes | TEXT | YES | Additional notes |
| created_by | INT | YES | Staff ID who created |
| created_at | DATETIME | NO | Creation timestamp |
| updated_at | DATETIME | YES | Last update timestamp |

**Indexes:**
```sql
PRIMARY KEY (id)
INDEX (customer_id)
INDEX (expiry_date)
FOREIGN KEY (customer_id) → tblclients(userid)
```

### tblcdsm_reminders

Stores reminder records for documents.

| Column | Type | Nullable | Description |
|--------|------|----------|-------------|
| id | INT | NO | Primary Key, Auto-increment |
| document_id | INT | NO | Foreign Key to tblcdsm_documents |
| reminder_days | INT | NO | Days before expiry (30, 15, 7, 0) |
| reminder_date | DATE | NO | Date reminder should trigger |
| is_notified | TINYINT | NO | 0=Pending, 1=Notified |
| notified_at | DATETIME | YES | When reminder was sent |
| created_at | DATETIME | NO | Creation timestamp |

**Indexes:**
```sql
PRIMARY KEY (id)
INDEX (document_id)
INDEX (reminder_date)
FOREIGN KEY (document_id) → tblcdsm_documents(id)
```

### Query Examples

```sql
-- Get all documents expiring in 30 days
SELECT * FROM tblcdsm_documents 
WHERE expiry_date BETWEEN CURDATE() AND DATE_ADD(CURDATE(), INTERVAL 30 DAY);

-- Get pending reminders
SELECT r.*, d.document_name, c.company 
FROM tblcdsm_reminders r
JOIN tblcdsm_documents d ON r.document_id = d.id
JOIN tblclients c ON d.customer_id = c.userid
WHERE r.is_notified = 0 AND r.reminder_date <= CURDATE();

-- Get expired documents
SELECT * FROM tblcdsm_documents 
WHERE expiry_date < CURDATE();

-- Count reminders by status
SELECT reminder_days, COUNT(*) as count 
FROM tblcdsm_reminders 
WHERE is_notified = 0 
GROUP BY reminder_days;
```

---

## 📡 API Reference

### Controllers

#### Cdsm Controller

**Main controller for document management.**

##### Methods

```php
// List all documents
GET /admin/cdsm

// Add new document
POST /admin/cdsm/add_document
// Parameters: customer_id, document_name, document_type, issue_date, expiry_date, notes

// Edit document
POST /admin/cdsm/edit_document/{id}
// Parameters: Same as add_document

// View document details
GET /admin/cdsm/view_document/{id}

// Delete document
GET /admin/cdsm/delete_document/{id}

// List reminders
GET /admin/cdsm/reminders

// Settings page
GET /admin/cdsm/settings
POST /admin/cdsm/settings
// Parameters: reminder_30_days, reminder_15_days, reminder_7_days, enable_email_reminders

// Mark reminder as notified
POST /admin/cdsm/mark_reminder_notified/{reminder_id}
// Returns: JSON {success: true/false}
```

#### Cron Controller

**Handles automated reminders.**

```php
// Process pending reminders (called by cron)
GET /index.php cdsm process_reminders

// Generate reminders for documents
GET /index.php cdsm generate_reminders

// Cleanup old notifications
GET /index.php cdsm cleanup
```

### Models

#### Cdsm_documents_model

```php
// Get all documents
get_all_documents()

// Get single document
get_document($id)

// Get customer documents
get_customer_documents($customer_id)

// Get document status
get_document_status($document)
// Returns: ['status' => 'valid|follow_up|expiring_soon|nearly_expiring|expired', 
//           'color' => '#hex', 'days' => integer]

// Add document
add_document($data)

// Update document
update_document($id, $data)

// Delete document
delete_document($id)

// Get expiring soon (within 30 days)
get_expiring_soon()

// Get expired documents
get_expired_documents()
```

#### Cdsm_reminders_model

```php
// Get all reminders
get_all_reminders()

// Get reminders by days interval
get_reminders_by_days($days)

// Get expired reminders
get_expired_reminders()

// Get document reminders
get_document_reminders($document_id)

// Mark reminder as notified
mark_as_notified($reminder_id)

// Get pending reminders
get_pending_reminders()
```

### Helper Functions

#### Cdsm_email_helper

```php
// Send reminder email to staff
send_reminder_email($reminder, $days_left)

// Send bulk reminders
send_bulk_reminders($reminders, $interval_days)

// Send customer notification
send_customer_notification($reminder)
```

### Response Examples

#### Add Document
```json
{
  "success": true,
  "message": "Document added successfully",
  "document_id": 123
}
```

#### Get Reminders
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "document_id": 123,
      "document_name": "Passport",
      "customer_name": "John Doe",
      "reminder_days": 30,
      "reminder_date": "2026-04-19",
      "is_notified": 0
    }
  ]
}
```

#### Document Status
```json
{
  "status": "nearly_expiring",
  "color": "#dc3545",
  "days": 5
}
```

---

## 🐛 Troubleshooting

### Module Not Appearing

**Problem:** Module doesn't show in left menu after installation.

**Solutions:**
1. Clear cache: **Setup → Utilities → Clear Cache**
2. Verify folder name is exactly `cdsm` (case-sensitive)
3. Check all required files exist in module root
4. Set file permissions: `chmod 644 cdsm/*.php`
5. Logout and login again to refresh

### Database Tables Not Created

**Problem:** Getting "table doesn't exist" errors.

**Solutions:**
1. Check tables exist in MySQL:
   ```sql
   SHOW TABLES LIKE 'tblcdsm%';
   ```
2. If missing, reinstall: **Setup → Modules → CDSM → Install**
3. Or manually create with SQL in INSTALLATION.md
4. Check database user has CREATE TABLE permission

### Emails Not Sending

**Problem:** No email notifications received.

**Solutions:**
1. Test SMTP: **Setup → Email → Send Test Email**
2. Check Email Queue: **Setup → Utilities → Email Queue**
3. Verify SMTP credentials are correct
4. Check server firewall allows outbound email
5. Review error logs for SMTP errors

### Cron Job Not Running

**Problem:** Reminders not being processed automatically.

**Solutions:**
1. Verify cron job exists: `crontab -l`
2. Test manually: 
   ```bash
   php /path/to/perfex/index.php cdsm process_reminders
   ```
3. Check PHP path is correct
4. Verify permissions on module folder
5. Review server cron logs: `grep CRON /var/log/syslog`

### Permission Denied

**Problem:** "Access Denied" when accessing CDSM.

**Solutions:**
1. Go: **Setup → Staff**
2. Select your staff member
3. Click: **Permissions**
4. Find: **CDSM** section
5. Grant: View, Create, Edit, Delete ✓
6. Click: **Save**

### Duplicate Reminders

**Problem:** Multiple reminders created for same document.

**Solutions:**
1. Check document wasn't saved multiple times
2. Verify no duplicate records in database:
   ```sql
   SELECT document_id, reminder_days, COUNT(*) 
   FROM tblcdsm_reminders 
   GROUP BY document_id, reminder_days 
   HAVING COUNT(*) > 1;
   ```
3. Delete duplicates manually if needed

### SQL Errors

**Problem:** Error messages about missing columns or tables.

**Solutions:**
1. Reinstall module (should recreate tables)
2. Check database compatibility (MySQL 5.6+)
3. Review error log for specific SQL error
4. Check database user has proper permissions

---

## 📋 Permissions

CDSM uses PerfexCRM's standard permission system.

### Permission Levels

| Permission | Description |
|-----------|-------------|
| view | Can view documents and reminders |
| create | Can add new documents |
| edit | Can edit existing documents |
| delete | Can delete documents |

### Setting Permissions

1. **Setup → Staff**
2. Select staff member
3. **Permissions** tab
4. Find **CDSM** section
5. Check desired permissions
6. **Save**

### Permission Checks in Code

```php
// Check if user can view module
if (!has_permission('cdsm', '', 'view')) {
    access_denied('cdsm');
}

// Check if user can edit
if (!has_permission('cdsm', '', 'edit')) {
    access_denied('cdsm');
}
```

---

## 🔄 Cron Job Management

### Understanding Cron Jobs

A cron job is a scheduled task that runs automatically. For CDSM, it processes pending reminders and sends email notifications.

### Setup Cron on Different Platforms

#### Linux/cPanel
```bash
crontab -e
0 */2 * * * /usr/bin/php /home/user/public_html/perfex/index.php cdsm process_reminders
```

#### Plesk
1. Scheduled Tasks
2. Add New Task
3. Command: `/usr/bin/php /path/to/perfex/index.php cdsm process_reminders`
4. Frequency: Every 2 hours

#### Windows Server
1. Task Scheduler
2. Create Task
3. Program: `C:\php\php.exe`
4. Arguments: `C:\path\to\perfex\index.php cdsm process_reminders`

#### Direct URL (Webhook)
```
http://your-domain.com/index.php?c=cdsm&m=process_reminders
```
Add to cron monitoring service like EasyCron, WebCron, etc.

### Monitoring Cron Execution

```bash
# Check cron logs (Linux)
grep CRON /var/log/syslog

# Check cron logs (macOS)
log stream --predicate 'process == "cron"'

# Test manually
php /path/to/perfex/index.php cdsm process_reminders

# Watch cron in real-time
watch -n 10 'tail -20 /var/log/syslog | grep CRON'
```

---

## 🤝 Contributing

We welcome contributions! Here's how:

### Report Bugs
1. Go to [GitHub Issues](https://github.com/virenderwwms-coder/perfex-cdsm-module/issues)
2. Click **New Issue**
3. Describe the bug with steps to reproduce
4. Include PerfexCRM version, PHP version, error message

### Suggest Features
1. Go to Issues
2. Click **New Issue**
3. Describe the feature and use case
4. Include mockups or examples if helpful

### Submit Code
1. Fork repository
2. Create branch: `git checkout -b feature/your-feature`
3. Make changes
4. Commit: `git commit -m "Add feature description"`
5. Push: `git push origin feature/your-feature`
6. Create Pull Request

### Code Style
- Follow PerfexCRM/CodeIgniter conventions
- Use 4-space indentation (not tabs)
- Add comments for complex logic
- Write descriptive commit messages

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file

**Summary:**
- ✅ Use commercially
- ✅ Modify the code
- ✅ Distribute copies
- ❌ No warranty/liability

---

## 📞 Support & Contact

### Documentation
- [INSTALLATION.md](INSTALLATION.md) - Detailed setup guide
- [QUICK_START.md](QUICK_START.md) - 5-minute quick start
- [API.md](API.md) - API reference
- [FAQ.md](FAQ.md) - Frequently asked questions

### Getting Help
1. Check README and documentation
2. Search GitHub Issues for similar problems
3. Report new issue on GitHub
4. Contact via email (see GitHub profile)

### Links
- 🌐 [PerfexCRM Official](https://perfexcrm.com)
- 🐙 [GitHub Repository](https://github.com/virenderwwms-coder/perfex-cdsm-module)
- ��� [Issues & Bug Reports](https://github.com/virenderwwms-coder/perfex-cdsm-module/issues)
- ⭐ [Star this Repository](https://github.com/virenderwwms-coder/perfex-cdsm-module)

---

## 🙏 Acknowledgments

- Built for [PerfexCRM](https://perfexcrm.com)
- Uses [CodeIgniter](https://codeigniter.com) framework
- MIT License

---

## ⭐ Show Your Support

If you find this module helpful:
- ⭐ **Star** this repository
- 📢 **Share** with others
- 🐛 **Report** bugs and issues
- 💡 **Suggest** improvements
- ❤️ **Contribute** to the project

---

<div align="center">

**Made with ❤️ by [Virendra](https://github.com/virenderwwms-coder)**

*Last Updated: March 20, 2026 | Version: 1.0.0*

**[⬆ back to top](#-customer-document-management-system-cdsm---perfexcrm-module)**

</div>
