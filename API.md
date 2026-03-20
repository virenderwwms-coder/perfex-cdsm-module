# 📡 API Reference - CDSM Module

Complete API documentation for Customer Document Management System (CDSM).

## Table of Contents

- [Overview](#overview)
- [Authentication](#authentication)
- [Base URL](#base-url)
- [Response Format](#response-format)
- [Controllers](#controllers)
  - [Cdsm Controller](#cdsm-controller)
  - [Cron Controller](#cron-controller)
- [Models](#models)
  - [Cdsm_documents_model](#cdsm_documents_model)
  - [Cdsm_reminders_model](#cdsm_reminders_model)
  - [Cdsm_model](#cdsm_model)
- [Helpers](#helpers)
  - [Cdsm_email_helper](#cdsm_email_helper)
- [Error Handling](#error-handling)
- [Status Codes](#status-codes)
- [Examples](#examples)

---

## 📖 Overview

The CDSM API allows you to:
- Manage customer documents programmatically
- Query reminder status
- Send notifications
- Automate document workflows
- Integrate with external systems

All API calls require authentication and proper permissions.

---

## 🔐 Authentication

CDSM uses PerfexCRM's built-in authentication system. All API calls must be made within PerfexCRM's admin or staff context.

### Authentication Methods

#### Admin Panel
```php
// Automatic - all actions in admin panel are authenticated
$this->load->model('cdsm_documents_model');
$documents = $this->cdsm_documents_model->get_all_documents();
```

#### API Call with Permission Check
```php
// Always check permissions
if (!has_permission('cdsm', '', 'view')) {
    return ['success' => false, 'message' => 'Permission denied'];
}
```

#### Cron Job (CLI)
```bash
# Runs with server permissions
/usr/bin/php /path/to/perfex/index.php cdsm process_reminders
```

---

## 🔗 Base URL

```
https://your-perfex-installation.com/index.php
```

### Examples

```
Admin Controller: https://domain.com/index.php/admin/cdsm
Cron Job: https://domain.com/index.php cdsm process_reminders
```

---

## 📤 Response Format

All responses follow a consistent format:

### Success Response
```json
{
  "success": true,
  "message": "Operation completed successfully",
  "data": {
    // Response data here
  }
}
```

### Error Response
```json
{
  "success": false,
  "message": "Error description",
  "error_code": "ERROR_TYPE"
}
```

### Redirect Response
```php
// On success, usually redirects to:
redirect(admin_url('cdsm'));
```

---

## 🎮 Controllers

### Cdsm Controller

Main controller for document and reminder management.

#### Cdsm::index()

**List all documents**

```php
GET /admin/cdsm
```

**Response:**
```php
// Renders view with:
$data['title'] = 'Documents';
$data['documents'] = [ /* document array */ ];
```

**Permissions Required:** `cdsm.view`

**Example Output:**
```html
<!-- Document management interface -->
```

---

#### Cdsm::add_document()

**Display add document form or process form submission**

```php
GET /admin/cdsm/add_document
POST /admin/cdsm/add_document
```

**POST Parameters:**
```php
[
    'customer_id'   => INT (required) ,
    'document_name' => STRING (required),
    'document_type' => STRING (required),
    'issue_date'    => DATE (required),
    'expiry_date'   => DATE (required),
    'notes'         => TEXT (optional)
]
```

**Permissions Required:** `cdsm.create`

**Response on Success:**
```php
Set alert: "Document added successfully"
Redirect: /admin/cdsm
```

**Response on Failure:**
```php
Set alert: "Document add failed"
Redirect: /admin/cdsm/add_document
```

---

#### Cdsm::edit_document($id)

**Display edit form or process form submission**

```php
GET /admin/cdsm/edit_document/{id}
POST /admin/cdsm/edit_document/{id}
```

**URL Parameters:**
```php
{id} = INT (Document ID)
```

**POST Parameters:**
Same as `add_document()`

**Permissions Required:** `cdsm.edit`

**Response on Success:**
```php
Set alert: "Document updated successfully"
Redirect: /admin/cdsm
```

---

#### Cdsm::view_document($id)

**Display document details**

```php
GET /admin/cdsm/view_document/{id}
```

**URL Parameters:**
```php
{id} = INT (Document ID)
```

**Permissions Required:** `cdsm.view`

**Response:**
```php
$data['title'] = 'View Document';
$data['document'] = document_object;
$data['reminders'] = reminder_array;
```

---

#### Cdsm::delete_document($id)

**Delete a document**

```php
GET /admin/cdsm/delete_document/{id}
```

**URL Parameters:**
```php
{id} = INT (Document ID)
```

**Permissions Required:** `cdsm.delete`

**Response on Success:**
```php
Set alert: "Document deleted successfully"
Redirect: /admin/cdsm
```

---

#### Cdsm::reminders()

**Display reminders dashboard**

```php
GET /admin/cdsm/reminders
```

**Response:**
```php
$data['title'] = 'Reminders';
$data['reminders_30_days'] = array;
$data['reminders_15_days'] = array;
$data['reminders_7_days'] = array;
$data['expired_reminders'] = array;
$data['all_reminders'] = array;
```

**Permissions Required:** `cdsm.view`

---

#### Cdsm::settings()

**Display/save settings**

```php
GET /admin/cdsm/settings
POST /admin/cdsm/settings
```

**POST Parameters:**
```php
[
    'reminder_30_days' => INT,
    'reminder_15_days' => INT,
    'reminder_7_days'  => INT,
    'enable_email_reminders' => BOOLEAN
]
```

**Permissions Required:** `cdsm.edit`

**Response on Success:**
```php
Set alert: "Settings updated successfully"
Redirect: /admin/cdsm/settings
```

---

#### Cdsm::mark_reminder_notified($reminder_id)

**Mark reminder as notified**

```php
POST /admin/cdsm/mark_reminder_notified/{reminder_id}
```

**URL Parameters:**
```php
{reminder_id} = INT
```

**Response (JSON):**
```json
{
  "success": true,
  "message": "Reminder marked as notified"
}
```

**Response on Failure:**
```json
{
  "success": false,
  "message": "Failed to mark reminder"
}
```

---

### Cron Controller

Handles automated tasks via cron jobs.

#### Cron::process_reminders()

**Process pending reminders and send emails**

```bash
php /path/to/perfex/index.php cdsm process_reminders
```

**What It Does:**
1. Finds all pending reminders (not yet notified)
2. Sends email notifications to staff
3. Sends customer notifications if critical
4. Marks reminders as notified
5. Logs activity

**Output:**
```
Starting reminder processing...
Found X pending reminders
Processing reminder for: [Document Name] - Days: Y
  ✓ Reminder processed and marked as notified
Reminder processing completed successfully.
```

**Returns:**
```php
// Logs to: application/logs/log-*.php
log_message('info', 'CDSM Cron: Successfully processed X reminders');
```

---

#### Cron::generate_reminders()

**Generate reminders for documents (can be used for bulk operations)**

```bash
php /path/to/perfex/index.php cdsm generate_reminders
```

**What It Does:**
- Generates reminders for all documents
- Creates reminder records at 30, 15, 7, 0 days intervals
- Useful for bulk operations

---

#### Cron::cleanup()

**Clean up old notifications**

```bash
php /path/to/perfex/index.php cdsm cleanup
```

**What It Does:**
- Deletes notified reminders older than 6 months
- Keeps database clean
- Archives old data

---

## 🗂️ Models

### Cdsm_documents_model

Manages document database operations.

#### get_all_documents()

**Get all documents**

```php
$documents = $this->cdsm_documents_model->get_all_documents();
```

**Returns:**
```php
// Array of document objects
[
    {
        'id' => 1,
        'customer_id' => 123,
        'document_name' => 'Passport',
        'document_type' => 'passport',
        'issue_date' => '2024-01-01',
        'expiry_date' => '2026-01-01',
        'customer_name' => 'John Doe',
        // ...
    },
    // more documents...
]
```

---

#### get_document($id)

**Get single document**

```php
$document = $this->cdsm_documents_model->get_document(123);
```

**Parameters:**
- `$id` (INT) - Document ID

**Returns:**
```php
// Single document object or null
{
    'id' => 123,
    'customer_id' => 456,
    'document_name' => 'Passport',
    // ...
}
```

---

#### get_customer_documents($customer_id)

**Get all documents for a customer**

```php
$docs = $this->cdsm_documents_model->get_customer_documents(456);
```

**Parameters:**
- `$customer_id` (INT) - Customer ID

**Returns:**
```php
// Array of customer's documents
[
    { document 1 },
    { document 2 },
    // ...
]
```

---

#### get_document_status($document)

**Get document status with color coding**

```php
$status = $this->cdsm_documents_model->get_document_status($document);
```

**Parameters:**
- `$document` (OBJECT) - Document object

**Returns:**
```php
[
    'status' => 'valid|follow_up|expiring_soon|nearly_expiring|expired',
    'color'  => '#hex_color',
    'days'   => integer  // days remaining
]
```

**Example:**
```php
[
    'status' => 'nearly_expiring',
    'color'  => '#dc3545',
    'days'   => 5
]
```

---

#### add_document($data)

**Add new document**

```php
$id = $this->cdsm_documents_model->add_document([
    'customer_id'   => 456,
    'document_name' => 'Passport',
    'document_type' => 'passport',
    'issue_date'    => '2024-01-01',
    'expiry_date'   => '2026-01-01',
    'notes'         => 'Renewal needed',
    'created_by'    => 1
]);
```

**Parameters:**
```php
[
    'customer_id'   => INT,
    'document_name' => STRING,
    'document_type' => STRING,
    'issue_date'    => DATE,
    'expiry_date'   => DATE,
    'notes'         => STRING (optional),
    'created_by'    => INT
]
```

**Returns:**
```php
// Document ID if successful
123

// Automatically creates reminders at 30, 15, 7, 0 days before expiry
```

---

#### update_document($id, $data)

**Update existing document**

```php
$this->cdsm_documents_model->update_document(123, [
    'document_name' => 'Passport - Renewed',
    'expiry_date'   => '2028-01-01'
]);
```

**Parameters:**
- `$id` (INT) - Document ID
- `$data` (ARRAY) - Fields to update (same as add_document)

**Returns:**
```php
true // on success
```

---

#### delete_document($id)

**Delete document and associated reminders**

```php
$this->cdsm_documents_model->delete_document(123);
```

**Parameters:**
- `$id` (INT) - Document ID

**Returns:**
```php
true // Always returns true (deletes document and reminders)
```

---

#### get_expiring_soon()

**Get documents expiring within 30 days**

```php
$expiring = $this->cdsm_documents_model->get_expiring_soon();
```

**Returns:**
```php
// Array of documents expiring within 30 days
[
    { document 1 },
    { document 2 },
    // ...
]
```

---

#### get_expired_documents()

**Get all expired documents**

```php
$expired = $this->cdsm_documents_model->get_expired_documents();
```

**Returns:**
```php
// Array of expired documents
[]
```

---

### Cdsm_reminders_model

Manages reminder database operations.

#### get_all_reminders()

**Get all reminders**

```php
$reminders = $this->cdsm_reminders_model->get_all_reminders();
```

**Returns:**
```php
// Array of all reminders with document/customer info
[
    {
        'id' => 1,
        'document_id' => 123,
        'document_name' => 'Passport',
        'customer_name' => 'John Doe',
        'reminder_days' => 30,
        'reminder_date' => '2026-12-20',
        'is_notified' => 0
    },
    // ...
]
```

---

#### get_reminders_by_days($days)

**Get reminders for specific interval**

```php
$pending_30 = $this->cdsm_reminders_model->get_reminders_by_days(30);
$pending_15 = $this->cdsm_reminders_model->get_reminders_by_days(15);
$pending_7 = $this->cdsm_reminders_model->get_reminders_by_days(7);
$expired = $this->cdsm_reminders_model->get_reminders_by_days(0);
```

**Parameters:**
- `$days` (INT) - 30, 15, 7, or 0 (for expired)

**Returns:**
```php
// Array of reminders for specified days
[]
```

---

#### get_expired_reminders()

**Get all expired document reminders**

```php
$expired = $this->cdsm_reminders_model->get_expired_reminders();
```

**Returns:**
```php
// Array of reminders for expired documents
[]
```

---

#### get_document_reminders($document_id)

**Get all reminders for a document**

```php
$reminders = $this->cdsm_reminders_model->get_document_reminders(123);
```

**Parameters:**
- `$document_id` (INT) - Document ID

**Returns:**
```php
// Array of 4 reminders (30, 15, 7, 0 days)
[
    { reminder_30_days },
    { reminder_15_days },
    { reminder_7_days },
    { reminder_0_days }
]
```

---

#### mark_as_notified($reminder_id)

**Mark reminder as notified/processed**

```php
$this->cdsm_reminders_model->mark_as_notified(456);
```

**Parameters:**
- `$reminder_id` (INT) - Reminder ID

**Returns:**
```php
true // Always returns true
```

**Effect:**
- Sets `is_notified` = 1
- Sets `notified_at` = current timestamp

---

#### get_pending_reminders()

**Get all pending (not yet notified) reminders**

```php
$pending = $this->cdsm_reminders_model->get_pending_reminders();
```

**Returns:**
```php
// Only reminders where is_notified = 0 and reminder_date <= today
[]
```

---

### Cdsm_model

General model for common operations.

#### get_all_customers()

**Get list of all customers**

```php
$customers = $this->cdsm_model->get_all_customers();
```

**Returns:**
```php
// Array of customer objects
[
    { customer 1 },
    { customer 2 },
    // ...
]
```

---

#### get_document_types()

**Get available document types**

```php
$types = $this->cdsm_model->get_document_types();
```

**Returns:**
```php
[
    'passport'   => 'Passport',
    'id_proof'   => 'ID Proof',
    'license'    => 'License',
    'insurance'  => 'Insurance',
    'contract'   => 'Contract',
    'certificate' => 'Certificate',
    'permit'     => 'Permit',
    'visa'       => 'Visa',
    'other'      => 'Other'
]
```

---

## 📧 Helpers

### Cdsm_email_helper

Email notification helper class.

#### send_reminder_email($reminder, $days_left)

**Send reminder email to staff**

```php
$this->load->helper('cdsm_email_helper');
$helper = new Cdsm_email_helper();
$helper->send_reminder_email($reminder, 30);
```

**Parameters:**
- `$reminder` (OBJECT) - Reminder object with document/customer info
- `$days_left` (INT) - Days until expiry

**Returns:**
```php
true   // Email sent successfully
false  // Email sending failed
```

---

#### send_bulk_reminders($reminders, $interval_days)

**Send emails for multiple reminders**

```php
$reminders = $this->cdsm_reminders_model->get_reminders_by_days(30);
$helper->send_bulk_reminders($reminders, 30);
```

**Parameters:**
- `$reminders` (ARRAY) - Array of reminder objects
- `$interval_days` (INT) - Days interval

**Returns:**
```php
// Loops through all reminders and sends emails
```

---

#### send_customer_notification($reminder)

**Send notification to customer**

```php
$helper->send_customer_notification($reminder);
```

**Parameters:**
- `$reminder` (OBJECT) - Reminder object with customer email

**Returns:**
```php
true   // Email sent
false  // Failed
```

**When to Use:**
- Sent when document is within 7 days of expiry
- On expiry date
- Customer gets document renewal alert

---

## ⚠️ Error Handling

### Permission Errors

```php
if (!has_permission('cdsm', '', 'view')) {
    access_denied('cdsm');  // Displays error page
}
```

### Database Errors

```php
if (!$this->db->affected_rows()) {
    set_alert('danger', 'Database operation failed');
    redirect(admin_url('cdsm'));
}
```

### Email Errors

```php
if (!$this->email->send()) {
    log_message('error', 'Email failed: ' . $this->email->print_debugger());
}
```

### Validation Errors

```php
// Form validation in controller
$this->form_validation->set_rules('customer_id', 'Customer', 'required');
$this->form_validation->set_rules('expiry_date', 'Expiry Date', 'required|callback_date_check');

if ($this->form_validation->run() == FALSE) {
    // Validation failed, show form with errors
}
```

---

## 📊 Status Codes

### HTTP Status Codes

| Code | Meaning | Usage |
|------|---------|-------|
| 200 | OK | Successful operation |
| 302 | Redirect | Redirect after operation |
| 404 | Not Found | Document/resource not found |
| 403 | Forbidden | Permission denied |
| 500 | Server Error | Database or system error |

### Alert Types

| Type | Color | Usage |
|------|-------|-------|
| success | Green | Operation completed |
| danger | Red | Error occurred |
| warning | Yellow | Warning message |
| info | Blue | Information |

---

## 💡 Examples

### Example 1: Add Document Programmatically

```php
$this->load->model('cdsm_documents_model');

$data = [
    'customer_id'   => 456,
    'document_name' => 'Passport - John Doe',
    'document_type' => 'passport',
    'issue_date'    => date('Y-m-d', strtotime('-5 years')),
    'expiry_date'   => date('Y-m-d', strtotime('+1 year')),
    'notes'         => 'Valid for travel',
    'created_by'    => get_staff_user_id()
];

$document_id = $this->cdsm_documents_model->add_document($data);

if ($document_id) {
    echo "Document created: #" . $document_id;
    // Reminders automatically created
} else {
    echo "Failed to create document";
}
```

### Example 2: Get Status of All Documents

```php
$this->load->model('cdsm_documents_model');

$documents = $this->cdsm_documents_model->get_all_documents();

foreach ($documents as $doc) {
    $status = $this->cdsm_documents_model->get_document_status($doc);
    
    echo "Document: " . $doc->document_name . "\n";
    echo "Status: " . $status['status'] . " (" . $status['days'] . " days)\n";
    echo "Color: " . $status['color'] . "\n";
    echo "---\n";
}
```

### Example 3: Send Bulk Reminders

```php
$this->load->model('cdsm_reminders_model');
$this->load->helper('cdsm_email_helper');

$reminders = $this->cdsm_reminders_model->get_pending_reminders();

$helper = new Cdsm_email_helper();
$helper->send_bulk_reminders($reminders, 30);

echo "Sent " . count($reminders) . " reminders";
```

### Example 4: Get Customer's Expired Documents

```php
$this->load->model('cdsm_documents_model');

$customer_id = 456;
$docs = $this->cdsm_documents_model->get_customer_documents($customer_id);

$expired = [];

foreach ($docs as $doc) {
    if (strtotime($doc->expiry_date) < time()) {
        $expired[] = $doc;
    }
}

echo "Customer has " . count($expired) . " expired documents";
```

### Example 5: Check Module Availability

```php
// Check if CDSM module is installed
if (module_exists('cdsm')) {
    $this->load->model('cdsm_documents_model');
    // Use CDSM
}
```

---

## 🔗 Integration Example

### Integrate with Custom Module

```php
// In your custom module controller
public function sync_documents() {
    // Load CDSM models
    $this->load->model('cdsm_documents_model');
    
    // Get all expiring documents
    $expiring = $this->cdsm_documents_model->get_expiring_soon();
    
    // Process them
    foreach ($expiring as $doc) {
        // Your logic here
    }
}
```

---

<div align="center">

**[⬆ back to top](#-api-reference---cdsm-module)**

*Last Updated: March 20, 2026 | API Version: 1.0.0*

</div>
