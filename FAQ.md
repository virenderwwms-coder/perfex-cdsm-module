# ❓ FAQ - Customer Document Management System (CDSM)

Frequently Asked Questions and Answers

## Table of Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Email & Notifications](#email--notifications)
- [Cron Jobs](#cron-jobs)
- [Database](#database)
- [Troubleshooting](#troubleshooting)
- [Performance](#performance)
- [Security](#security)

---

## Installation

### Q: Do I need to have a special version of PerfexCRM?
**A:** No, CDSM works with any PerfexCRM version 2.0 or higher. Ensure you're running a current version for security patches.

### Q: Can I install CDSM on a shared hosting?
**A:** Yes! CDSM works on shared hosting with standard PHP/MySQL setup. Ensure PHP 5.6+ and MySQL 5.6+ are available.

### Q: Do I need SSH access to install CDSM?
**A:** No, you can upload files via FTP. However, SSH makes it easier. Either way works fine.

### Q: What if I already have documents stored elsewhere?
**A:** You can migrate them manually or write a script to import them. CDSM provides models for easy data insertion.

### Q: Can I uninstall CDSM without losing data?
**A:** When you uninstall, data is preserved. Reinstalling will find existing data. To delete data, manually delete database tables.

---

## Configuration

### Q: Can I change the reminder intervals?
**A:** Yes! Go to **CDSM → Settings** and modify:
- First Reminder (default 30 days)
- Second Reminder (default 15 days)
- Third Reminder (default 7 days)

Changes apply to new documents immediately.

### Q: What if I only want reminders at 30 and 7 days?
**A:** Set middle interval to same as one of the others, or modify the code in `add_document()` method.

### Q: Can I customize document types?
**A:** Yes! Edit `models/Cdsm_model.php`, function `get_document_types()`. Add/remove types as needed.

### Q: How do I customize email templates?
**A:** Edit `helpers/cdsm_email_helper.php` in the `_build_reminder_email()` method. Modify HTML as needed.

### Q: Can I disable email notifications completely?
**A:** Yes, go to **CDSM → Settings** and uncheck "Enable Email Reminders".

---

## Usage

### Q: How do I add a document?
**A:** 
1. Go: **CDSM → Documents**
2. Click: **+ Add Document**
3. Fill form with customer, dates, type
4. Click: **Save**

Reminders are created automatically.

### Q: Can I bulk import documents?
**A:** Currently no built-in bulk import. Options:
1. Import via custom script using the model
2. Add manually through UI
3. Request feature on GitHub

### Q: What happens if expiry date is today?
**A:** A reminder is created for today. If cron runs, the customer gets notified immediately.

### Q: Can I edit a document after creating it?
**A:** Yes! Click the edit (pencil) icon. Reminders are automatically updated.

### Q: What if I delete a document?
**A:** Document and all associated reminders are deleted permanently. Cannot be undone!

### Q: Can customers add their own documents?
**A:** Not in current version. Only staff can add documents. Customers can view in client portal.

### Q: How many documents can the system handle?
**A:** Theoretically unlimited. Performance depends on your server. Tested with 10,000+ documents.

### Q: Can I search for documents?
**A:** Yes, use your browser's find function (Ctrl+F). Full-text search feature planned for v1.1.

---

## Email & Notifications

### Q: Why am I not receiving email reminders?
**A:** Common causes:
1. SMTP not configured: **Setup → Email**
2. Email reminders disabled: **CDSM → Settings**
3. Cron job not running (check execution)
4. Email going to spam (check spam folder)

### Q: Can I send email reminders manually?
**A:** Yes, in reminders dashboard, click "Mark Done" which triggers manual processing.

### Q: Will customers get email notifications?
**A:** Customers get notified only when documents are within 7 days of expiry or expired. Can't disable.

### Q: Can I change the email sender address?
**A:** Change in **Setup → Email Settings**. Set "From Email" to desired address.

### Q: How do I customize email content?
**A:** Edit email templates in `helpers/cdsm_email_helper.php`. Modify `_build_reminder_email()` method.

### Q: Are emails encrypted?
**A:** Connection is encrypted (TLS/SSL). Email content is not encrypted. Use standard email security practices.

### Q: Can I see email history?
**A:** Yes, check **Setup → Utilities → Email Queue** to see sent emails and status.

### Q: What if SMTP server is down?
**A:** Emails are queued. When SMTP is back up, queued emails send automatically.

---

## Cron Jobs

### Q: What does the cron job do?
**A:** Every 2 hours (default), it:
1. Finds pending reminders
2. Sends email notifications
3. Marks reminders as processed
4. Logs activity

### Q: Do I NEED a cron job?
**A:** For full automation, yes. Without it, reminders won't send automatically. You can manually trigger processing.

### Q: How do I know if cron is running?
**A:** 
1. Check logs: `grep CDSM /var/log/syslog`
2. Test manually: `php /path/to/perfex/index.php cdsm process_reminders`
3. Check email queue for recent entries

### Q: Can I run cron more frequently?
**A:** Yes! Use `0 * * * *` for hourly or `*/30 * * * *` for every 30 minutes.

### Q: What's the recommended cron frequency?
**A:** 
- Small installations: Every 4 hours
- Medium: Every 2 hours (default)
- Large: Every hour
- Enterprise: Every 30 minutes

### Q: Will cron use a lot of resources?
**A:** No, very minimal. It's just a database query and email sending.

### Q: Can I schedule cron from outside cPanel?
**A:** Yes! Use external cron monitoring service (EasyCron, WebCron, etc.) to trigger the webhook URL.

---

## Database

### Q: Can I directly query CDSM tables?
**A:** Yes, but be careful! Use:
```sql
SELECT * FROM tblcdsm_documents;
SELECT * FROM tblcdsm_reminders;
```

### Q: How much space do CDSM tables use?
**A:** Very small. Typically < 1MB per 1000 documents. Depends on note length.

### Q: Can I backup just CDSM data?
**A:** Yes:
```sql
mysqldump -u user -p database tblcdsm_documents tblcdsm_reminders > backup.sql
```

### Q: How do I restore CDSM data?
**A:** 
```sql
mysql -u user -p database < backup.sql
```

### Q: Can I migrate CDSM to another PerfexCRM?
**A:** Yes! 
1. Backup CDSM tables
2. Install CDSM on new PerfexCRM
3. Restore tables to new database
4. Update customer IDs if different

### Q: Is there a way to export documents?
**A:** Not built-in. Options:
1. Use MySQL export
2. Write custom export script
3. Use database GUI tool (phpMyAdmin)

### Q: Can I have multiple document types with same expiry?
**A:** Yes! Each document is independent.

---

## Troubleshooting

### Q: Module says "Access Denied"
**A:** Your staff account doesn't have CDSM permissions:
1. Go: **Setup → Staff**
2. Select yourself
3. Click: **Permissions**
4. Check: CDSM section
5. Grant: View, Create, Edit, Delete

### Q: I see "Table doesn't exist" error
**A:** Tables weren't created:
1. Go: **Setup → Modules**
2. Click: **Install** on CDSM
3. Tables should be created
4. Or manually create using SQL

### Q: Documents aren't showing up
**A:** 
1. Check database connection
2. Verify tables exist
3. Check customer_id is valid
4. Ensure permissions are set

### Q: Reminders aren't triggering
**A:** Check:
1. Cron job is running: `crontab -l`
2. Reminder dates are correct (check database)
3. Emails configured: **Setup → Email**
4. Test cron manually

### Q: Edit button doesn't work
**A:** Check permissions. You need `cdsm.edit` permission.

### Q: Delete is showing "Operation failed"
**A:** 
1. Check you have `cdsm.delete` permission
2. Check document exists
3. Check database connectivity
4. Check error logs

### Q: Reminders showing for past dates
**A:** This is normal - indicates they weren't processed when scheduled. Run cron manually:
```bash
php /path/to/perfex/index.php cdsm process_reminders
```

---

## Performance

### Q: Will CDSM slow down my PerfexCRM?
**A:** No, CDSM is optimized:
- Database indexed properly
- Cron jobs run in background
- Minimal admin overhead

### Q: How do I optimize performance?
**A:** 
1. Run cron jobs during off-peak hours
2. Regular database maintenance (OPTIMIZE TABLE)
3. Archive old reminders (CDSM cleanup)

### Q: Can I run CDSM on slow servers?
**A:** Yes, works on slow servers. Just configure longer cron intervals.

### Q: How many documents can one customer have?
**A:** Unlimited. No performance penalty.

---

## Security

### Q: Is CDSM secure?
**A:** Yes!
- SQL injection prevention (parameterized queries)
- XSS prevention (HTML escaping)
- CSRF protection (token validation)
- Permission checking on all operations
- HTTPS/SSL support

### Q: Can I restrict who sees which documents?
**A:** All documents visible to users with CDSM permission. Granular filtering not yet available.

### Q: Is email data encrypted?
**A:** SMTP connection is encrypted (TLS/SSL). Content not encrypted. Standard practice.

### Q: Who can see reminders?
**A:** Any staff member with `cdsm.view` permission.

### Q: Can I audit changes?
**A:** Partially - check email queue for notifications. Full audit log feature planned for v1.2.

### Q: What about data privacy?
**A:** CDSM doesn't collect/share data. It's your data on your server. Comply with applicable privacy laws.

### Q: Can staff delete their own notifications?
**A:** No, only admins can delete. Maintains audit trail.

---

## Advanced Questions

### Q: Can I hook CDSM into my custom module?
**A:** Yes! Use the models:
```php
$this->load->model('cdsm_documents_model');
$docs = $this->cdsm_documents_model->get_all_documents();
```

### Q: Can I create custom views?
**A:** Yes, modify views in `views/` folder or extend controller.

### Q: Can I add custom fields to documents?
**A:** Yes, modify:
1. Database schema (add column)
2. Form in view
3. Model save/update methods

### Q: How do I contribute to CDSM?
**A:** 
1. Fork on GitHub
2. Create feature branch
3. Make changes
4. Submit pull request

### Q: Is source code available?
**A:** Yes! It's open source on GitHub. Read, modify, contribute!

### Q: Can I use CDSM for non-document tracking?
**A:** Yes, you can adapt it for anything with an expiry date (licenses, subscriptions, etc.).

---

## Still Have Questions?

- 📖 Read [README.md](README.md)
- 🔧 Check [INSTALLATION.md](INSTALLATION.md)
- 📡 See [API.md](API.md)
- 🐛 Report on [GitHub Issues](https://github.com/virenderwwms-coder/perfex-cdsm-module/issues)

---

<div align="center">

**Last Updated: March 20, 2026 | Version: 1.0.0**

**[⬆ back to top](#-faq---customer-document-management-system-cdsm)**

</div>