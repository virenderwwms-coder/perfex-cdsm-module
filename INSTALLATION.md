# 🔧 Complete Installation Guide - CDSM Module

Step-by-step guide to install Customer Document Management System on PerfexCRM.

## Table of Contents

- [System Requirements](#system-requirements)
- [Installation Methods](#installation-methods)
- [Post-Installation Setup](#post-installation-setup)
- [Database Configuration](#database-configuration)
- [Email Configuration](#email-configuration)
- [Cron Job Setup](#cron-job-setup)
- [Verification Checklist](#verification-checklist)
- [Troubleshooting](#troubleshooting)
- [Uninstallation](#uninstallation)

---

## ✅ System Requirements

### Server Requirements

| Component | Minimum | Recommended |
|-----------|---------|------------|
| PerfexCRM | 2.0 | Latest |
| PHP | 5.6 | 7.4+ |
| MySQL | 5.6 | 8.0+ |
| Apache/Nginx | Any | Latest |

### File Permissions

```
Module directory:  755 (drwxr-xr-x)
PHP files:        644 (-rw-r--r--)
Writable folders: 775 (drwxrwxr-x)
```

### Required PHP Extensions

- mysqli (database)
- OpenSSL (email/SMTP)
- GD (image processing)
- JSON (data handling)

### Network Requirements

- SMTP access for email
- Outbound port 25/587/465 (email)
- Internet connection for setup
- Cron job capability

---

## 📥 Installation Methods

### Method 1: Git Clone (Recommended)

**Prerequisites:**
- SSH access to server
- Git installed on server

**Steps:**

```bash
# 1. Navigate to modules directory
cd /home/username/public_html/perfex/modules
# or
cd /var/www/html/perfex/modules

# 2. Clone repository
git clone https://github.com/virenderwwms-coder/perfex-cdsm-module.git cdsm

# 3. Set permissions
chmod -R 755 cdsm
chmod 644 cdsm/*.php
find cdsm -name "*.php" -exec chmod 644 {} \;
find cdsm -type d -exec chmod 755 {} \;

# 4. Verify installation
ls -la cdsm/
# Should show: config.php, cdsm.php, controllers/, models/, etc.
```

### Method 2: Manual Download

**Steps:**

1. Download ZIP from GitHub
   - Go: https://github.com/virenderwwms-coder/perfex-cdsm-module
   - Click: Code → Download ZIP

2. Extract ZIP
   - Extract to: `/path/to/perfex/modules/cdsm`

3. Upload via FTP
   - Connect to server via FTP
   - Navigate to: `public_html/perfex/modules/`
   - Upload `cdsm` folder

4. Set Permissions (via FTP or Hosting Control Panel)
   - Folder permissions: 755
   - File permissions: 644

### Method 3: cPanel File Manager

1. Login to cPanel
2. Navigate to: `public_html/perfex/modules/`
3. Upload ZIP file
4. Right-click → Extract
5. Set permissions (File Manager → Change Permissions)
6. Folder: 755, Files: 644

---

## 📦 Installation Structure

After installation, verify folder structure:

```
/modules/cdsm/
├── config.php                    ← Module configuration
├── cdsm.php                      ← Main module class
├── install.php                   ← Installation hooks
│
├── controllers/
│   ├── Cdsm.php                 ← Main controller
│   └── Cron.php                 ← Automation controller
│
├── models/
│   ├── Cdsm_model.php
│   ├── Cdsm_documents_model.php
│   └── Cdsm_reminders_model.php
│
├── views/
│   ├── admin/
│   │   ├── manage_documents.php
│   │   ├── add_edit_document.php
│   │   ├── document_details.php
│   │   ├── reminders_dashboard.php
│   │   └── reminder_settings.php
│   └── client/
│       └── my_documents.php
│
├── helpers/
│   └── cdsm_email_helper.php
│
├── language/
│   └── english/
│       └── cdsm_lang.php
│
├── hooks/
│   └── cdsm_hooks.php
│
├── assets/
│   ├── css/
│   │   └── cdsm.css
│   └── js/
│       └── cdsm.js
│
└── migrations/
    └── 001_create_cdsm_tables.php
```

**Verify all files exist:**
```bash
cd /path/to/perfex/modules/cdsm
ls -la
# Should show all directories above
```

---

## 🚀 Activation in PerfexCRM

### Step 1: Access Admin Panel

1. Open browser
2. Go to: `https://your-perfex-installation.com/admin`
3. Login with administrator credentials

### Step 2: Navigate to Modules

1. Left menu → **Setup**
2. Click: **Modules**
3. Look for section: **Installed Modules** or **Available Modules**

### Step 3: Install CDSM

1. Find: **Customer Document Management (CDSM)**
   - Or search for "CDSM" or "Document"
   
2. Click: **Install** button
   - If already showing "Uninstall", module is already installed

3. Wait for completion
   - Should see success message: ✓ "Module installed successfully"

4. Verify module appears
   - Left menu should now show: **CDSM**
   - With sub-items: Documents, Reminders, Settings

### Step 4: Check Database

Verify tables were created:

**Via phpMyAdmin:**
1. Go to hosting control panel
2. Open phpMyAdmin
3. Select your database
4. Look for tables:
   - `tblcdsm_documents`
   - `tblcdsm_reminders`

**Via SSH:**
```bash
mysql -u username -p database_name
SHOW TABLES LIKE 'tblcdsm%';
```

Expected output:
```
+----------------------------------+
| Tables_in_database (tblcdsm%)    |
+----------------------------------+
| tblcdsm_documents                |
| tblcdsm_reminders                |
+----------------------------------+
```

---

## ⚙️ Post-Installation Setup

### Set Staff Permissions

All staff need permissions to use CDSM:

1. Go: **Setup → Staff**
2. Select staff member
3. Click: **Permissions**
4. Find: **CDSM** section
5. Check permissions:
   - ☑ View (can see documents/reminders)
   - ☑ Create (can add documents)
   - ☑ Edit (can edit documents)
   - ☑ Delete (can delete documents)
6. Click: **Save**

**Repeat for each staff member.**

### Create Test Document

1. Go: **CDSM → Documents** (left menu)
2. Click: **+ Add Document**
3. Fill form:
   - **Customer:** Select any customer
   - **Document Name:** Test Document
   - **Type:** Passport
   - **Issue Date:** Today
   - **Expiry Date:** 30 days from now
   - **Notes:** Test entry
4. Click: **Save**
5. Verify document appears in list
6. Document should show with status and days remaining

---

## 📧 Email Configuration

Email sending is optional but recommended for notifications.

### Step 1: Configure SMTP

1. Go: **Setup → Email Settings**
2. Select **Protocol:** SMTP
3. Fill in SMTP details (see provider list below)
4. Click: **Send Test Email**
5. Check email inbox (or spam folder)

### SMTP Provider Settings

#### Gmail

```
SMTP Host: smtp.gmail.com
SMTP Port: 587
SMTP Encryption: TLS
SMTP Username: your-email@gmail.com
SMTP Password: [App Password - NOT regular password]
From Email: your-email@gmail.com
```

**Get Gmail App Password:**
1. Go: https://myaccount.google.com/apppasswords
2. Select: Mail and Windows Computer
3. Generate password
4. Use this password in CDSM

#### Office 365

```
SMTP Host: smtp.office365.com
SMTP Port: 587
SMTP Encryption: TLS
SMTP Username: your-email@yourdomain.onmicrosoft.com
SMTP Password: Your Office 365 password
From Email: your-email@yourdomain.onmicrosoft.com
```

#### SendGrid

```
SMTP Host: smtp.sendgrid.net
SMTP Port: 587
SMTP Encryption: TLS
SMTP Username: apikey
SMTP Password: SG.your_sendgrid_api_key
From Email: your-email@yourdomain.com
```

#### Custom SMTP

Contact your hosting provider for:
- SMTP Host
- SMTP Port
- Encryption (TLS/SSL)
- Username
- Password

### Step 2: Enable CDSM Email Reminders

1. Go: **CDSM → Settings**
2. Check: ☑ **Enable Email Reminders**
3. Click: **Save Settings**

### Step 3: Test Email Sending

1. Go: **CDSM → Reminders**
2. If any pending reminders exist, cron will send emails
3. Or test manually by marking document to expire in 0 days
4. Check staff email for notification

---

## ⏰ Cron Job Setup

Cron jobs automate reminder processing. **Highly recommended!**

### Method 1: cPanel (Easiest)

1. Login to cPanel
2. Find: **Cron Jobs** (under "Advanced")
3. Click: **Add New Cron Job**

4. Fill form:
   - **Common Settings:** Custom
   - **Minute:** 0
   - **Hour:** */2 (every 2 hours)
   - **Day:** * (every day)
   - **Month:** * (every month)
   - **Weekday:** * (every day)
   - **Command:** 
   ```
   /usr/bin/php /home/username/public_html/perfex/index.php cdsm process_reminders > /dev/null 2>&1
   ```

5. Click: **Add New Cron Job**

### Method 2: SSH/Command Line

```bash
# Edit crontab
crontab -e

# Add line (every 2 hours):
0 */2 * * * /usr/bin/php /path/to/perfex/index.php cdsm process_reminders > /dev/null 2>&1

# Save (Ctrl+X, Y, Enter in nano)

# Verify cron was added
crontab -l
```

### Method 3: Plesk

1. Login to Plesk
2. Go: **Scheduled Tasks**
3. Click: **Add Task**
4. Fill:
   - **Task Name:** CDSM Reminders
   - **Run As:** nobody
   - **Command:** `/usr/bin/php /var/www/vhosts/yourdomain.com/httpdocs/perfex/index.php cdsm process_reminders`
   - **Frequency:** Every 2 hours
5. Click: **OK**

### Method 4: External Cron Service

If server doesn't support cron, use external service:

1. Go: https://easycron.com (or similar)
2. Create account
3. Add cron job:
   - **URL:** `https://your-domain.com/index.php?c=cdsm&m=process_reminders&key=YOUR_SECRET_KEY`
   - **Frequency:** Every 2 hours
4. Save

### Cron Frequency Reference

| Volume | Frequency | Command |
|--------|-----------|---------|
| Low (<100 docs) | Every 4 hours | `0 */4 * * *` |
| Medium (100-500) | Every 2 hours | `0 */2 * * *` |
| High (500-5000) | Every hour | `0 * * * *` |
| Very High (5000+) | Every 30 min | `*/30 * * * *` |

### Testing Cron

```bash
# Manual test
php /path/to/perfex/index.php cdsm process_reminders

# Expected output:
# Starting reminder processing...
# Found X pending reminders
# Processing reminder for: [Document Name]
#   ✓ Reminder processed and marked as notified
# Reminder processing completed successfully.
```

### Monitoring Cron

```bash
# Check if cron ran (Linux)
grep CDSM /var/log/syslog

# Check cron logs (CentOS)
tail -f /var/log/cron

# Real-time monitoring
watch 'tail -20 /var/log/syslog | grep CDSM'
```

---

## ✅ Verification Checklist

After installation, verify everything works:

- [ ] Module folder exists at `/modules/cdsm/`
- [ ] All required files present (use `ls -la`)
- [ ] Module appears in left menu (**CDSM**)
- [ ] Can access **CDSM → Documents**
- [ ] Can access **CDSM → Reminders**
- [ ] Can access **CDSM → Settings**
- [ ] Database tables exist (`tblcdsm_documents`, `tblcdsm_reminders`)
- [ ] Staff permissions set for at least one user
- [ ] Test document added successfully
- [ ] Test document shows in documents list
- [ ] Test document shows status and days remaining
- [ ] SMTP configured in **Setup → Email**
- [ ] Test email sends successfully
- [ ] Email reminders enabled in **CDSM → Settings**
- [ ] Cron job added to server (verify with `crontab -l`)
- [ ] Cron job tested manually

**If all checked:**  ✅ **Installation Complete!**

---

## 🐛 Troubleshooting

### Module Not Appearing in Menu

**Symptom:** After installation, CDSM doesn't show in left menu.

**Solutions:**
1. Clear cache: **Setup → Utilities → Clear Cache**
2. Logout and login again
3. Check folder name: Must be exactly `cdsm` (lowercase)
4. Verify file `config.php` exists
5. Check permissions: `chmod 755 cdsm && chmod 644 cdsm/*.php`
6. Check error logs: Check PHP error_log for messages

### "Table doesn't exist" Error

**Symptom:** Error "Table 'tblcdsm_documents' doesn't exist"

**Solutions:**
1. Verify tables exist:
   ```sql
   SHOW TABLES LIKE 'tblcdsm%';
   ```
2. If missing, reinstall: **Setup → Modules → CDSM → Install**
3. Or manually create:
   ```sql
   CREATE TABLE tblcdsm_documents (
       id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
       customer_id INT UNSIGNED NOT NULL,
       document_name VARCHAR(255),
       document_type VARCHAR(100),
       issue_date DATE,
       expiry_date DATE,
       notes TEXT,
       created_by INT UNSIGNED,
       created_at DATETIME,
       updated_at DATETIME,
       INDEX (customer_id),
       INDEX (expiry_date)
   );
   
   CREATE TABLE tblcdsm_reminders (
       id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
       document_id INT UNSIGNED NOT NULL,
       reminder_days INT,
       reminder_date DATE,
       is_notified TINYINT DEFAULT 0,
       notified_at DATETIME,
       created_at DATETIME,
       INDEX (document_id),
       INDEX (reminder_date)
   );
   ```

### "Access Denied" Error

**Symptom:** "Access Denied" when opening CDSM

**Solutions:**
1. Check staff permissions:
   - **Setup → Staff** → Select staff → **Permissions**
   - Find **CDSM** section
   - Check permissions ✓
2. Ensure user role has CDSM access
3. Try logging out and back in
4. Check database for permissions table

### Emails Not Sending

**Symptom:** No emails received for reminders

**Solutions:**
1. Test SMTP:
   - **Setup → Email** → **Send Test Email**
   - Check inbox (including spam)
2. Check Email Queue:
   - **Setup → Utilities → Email Queue**
   - See if emails are queued
3. Verify SMTP settings:
   - Check host, port, username, password
   - Verify port 25/587/465 not blocked
4. Check logs:
   - Look in `application/logs/` for email errors
5. Verify cron running:
   - Check `crontab -l`
   - Test manually

### Cron Job Not Running

**Symptom:** Reminders not processing automatically

**Solutions:**
1. Verify cron exists:
   - `crontab -l`
2. Test manually:
   ```bash
   php /path/to/perfex/index.php cdsm process_reminders
   ```
3. Check PHP path:
   - Verify `/usr/bin/php` exists
   - Or use `which php` to find correct path
4. Check file permissions:
   - Folder: `chmod 755 cdsm`
   - Files: `chmod 644 cdsm/*.php`
5. Check cron logs:
   - `grep CDSM /var/log/syslog`
6. Run cron more frequently (test with every minute)

### Duplicate Reminders

**Symptom:** Multiple reminders for same document

**Solutions:**
1. Check document wasn't saved twice
2. Find duplicates:
   ```sql
   SELECT document_id, reminder_days, COUNT(*) 
   FROM tblcdsm_reminders 
   GROUP BY document_id, reminder_days 
   HAVING COUNT(*) > 1;
   ```
3. Delete duplicates:
   ```sql
   DELETE FROM tblcdsm_reminders 
   WHERE id IN (
       SELECT id FROM (
           SELECT id, ROW_NUMBER() OVER (PARTITION BY document_id, reminder_days ORDER BY id) as row_num
           FROM tblcdsm_reminders
       ) t WHERE row_num > 1
   );
   ```

### Error in Controller/Model

**Symptom:** PHP errors in controller or model files

**Solutions:**
1. Check syntax (look for typos)
2. Verify all required methods exist
3. Check file encoding (must be UTF-8)
4. Ensure all quotes are matching
5. Review error message for line number
6. Check PHP error logs

---

## 🗑️ Uninstallation

### To Deactivate Module (Preserve Data)

1. Go: **Setup → Modules**
2. Find: **Customer Document Management (CDSM)**
3. Click: **Uninstall**
4. Confirm: Yes
5. Module removed from menu
6. **Database tables remain** (data preserved)

### To Delete Module Completely

1. Deactivate module (above steps)
2. Via FTP/SSH, delete folder:
   ```bash
   rm -rf /path/to/perfex/modules/cdsm
   ```
3. Optional: Delete database tables:
   ```sql
   DROP TABLE tblcdsm_reminders;
   DROP TABLE tblcdsm_documents;
   ```

### To Preserve and Archive Data

Before uninstalling:

```bash
# Backup tables
mysqldump -u user -p database tblcdsm_documents tblcdsm_reminders > cdsm_backup.sql

# Backup files
tar -czf cdsm_backup.tar.gz /path/to/modules/cdsm/
```

---

## 📞 Need Help?

- Check [FAQ.md](FAQ.md) for common questions
- Read [README.md](README.md) for overview
- Check [API.md](API.md) for technical details
- Report issue on [GitHub](https://github.com/virenderwwms-coder/perfex-cdsm-module/issues)

---

<div align="center">

**Last Updated: March 20, 2026 | Version: 1.0.0**

**[⬆ back to top](#-complete-installation-guide---cdsm-module)**

</div>