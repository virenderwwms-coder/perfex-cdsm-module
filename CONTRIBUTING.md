# 🤝 Contributing to CDSM Module

Thank you for considering contributing to Customer Document Management System! 

## Code of Conduct

- Be respectful and inclusive
- No harassment or discrimination
- Welcome all contributions
- Focus on the code, not the person

## Getting Started

### Fork the Repository

1. Go to [GitHub repo](https://github.com/virenderwwms-coder/perfex-cdsm-module)
2. Click: **Fork** button
3. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/perfex-cdsm-module.git
   cd perfex-cdsm-module
   ```

### Create Development Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
# or
git checkout -b docs/your-documentation
```

### Make Changes

- Follow PerfexCRM/CodeIgniter conventions
- Use 4-space indentation (not tabs)
- Write descriptive variable names
- Add comments for complex logic

### Commit Changes

```bash
git add .
git commit -m "Clear description of what you changed"
```

**Commit Message Examples:**
```
Add bulk document import feature
Fix email template rendering bug
Update installation documentation
Improve database query performance
```

### Push and Create Pull Request

```bash
git push origin feature/your-feature-name
```

Then create Pull Request on GitHub:
1. Go to your fork
2. Click: **Pull Request**
3. Fill description
4. Submit

---

## Types of Contributions

### 🐛 Bug Reports

Found a bug? Report it:

1. Go: [GitHub Issues](https://github.com/virenderwwms-coder/perfex-cdsm-module/issues)
2. Click: **New Issue**
3. Select: **Bug Report**
4. Include:
   - PerfexCRM version
   - PHP version
   - Error message
   - Steps to reproduce
   - Expected behavior
   - Actual behavior

### 💡 Feature Requests

Have an idea? Suggest it:

1. Go: [GitHub Issues](https://github.com/virenderwwms-coder/perfex-cdsm-module/issues)
2. Click: **New Issue**
3. Select: **Feature Request**
4. Include:
   - Feature description
   - Use case
   - How it solves a problem
   - Mockups or examples (if helpful)

### 📝 Documentation

Improve our docs:

- README.md improvements
- Better examples
- Clearer explanations
- Fix typos/grammar
- Add translations

### 💻 Code

Contribute code:

- New features
- Bug fixes
- Performance improvements
- Refactoring
- Test cases

---

## Code Style Guide

### PHP Code Standards

**Indentation:**
```php
// Use 4 spaces (not tabs)
public function example() {
    if ($condition) {
        echo "Hello";
    }
}
```

**Naming:**
```php
// Functions: lowercase_with_underscores
public function get_document_status() {}

// Variables: $lowercase_with_underscores
$document_id = 123;

// Classes: PascalCase
class Cdsm_documents_model {}

// Constants: UPPERCASE_WITH_UNDERSCORES
const MAX_DOCUMENTS = 1000;
```

**Comments:**
```php
// Single line comment

/*
 * Multi-line comment
 * explaining complex logic
 */

/**
 * Doc comment for methods
 * @param int $id Document ID
 * @return object Document object
 */
public function get_document($id) {}
```

### SQL Standards

```sql
-- Use UPPERCASE for keywords
SELECT * FROM tblcdsm_documents
WHERE expiry_date > CURDATE()
ORDER BY expiry_date ASC;

-- Use backticks for identifiers
SELECT `id`, `document_name` FROM `tblcdsm_documents`;

-- Use indexes for performance
CREATE INDEX idx_expiry_date ON tblcdsm_documents(expiry_date);
```

### JavaScript Standards

```javascript
// Use camelCase for variables
var documentId = 123;
let reminderDate = "2026-12-20";
const MAX_RETRIES = 3;

// Use clear function names
function processDocumentReminder(documentId) {
    // implementation
}

// Use consistent formatting
if (condition) {
    // do something
} else {
    // do something else
}
```

---

## Testing

### Before Submitting

1. Test your changes thoroughly
2. Check for breaking changes
3. Verify database compatibility
4. Test on multiple PHP versions (5.6, 7.0, 7.4, 8.0)
5. Test with latest PerfexCRM

### Manual Testing

```bash
# Test document creation
# Test document editing
# Test document deletion
# Test reminder generation
# Test email sending
# Test cron job
```

---

## Documentation Updates

When adding features, update docs:

1. **README.md** - If adds major feature
2. **API.md** - If adds new methods/endpoints
3. **FAQ.md** - If answers common question
4. **INSTALLATION.md** - If changes setup process
5. **Code Comments** - Inline documentation

---

## Pull Request Process

### Before Submitting PR

- [ ] Code follows style guide
- [ ] Added/updated documentation
- [ ] Tested thoroughly
- [ ] No breaking changes
- [ ] Commits are clear and atomic

### PR Description Template

```markdown
## Description
Clear description of what this PR does.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
Describe how you tested this change.

## Checklist
- [ ] Code follows style guide
- [ ] Self-reviewed changes
- [ ] Commented complex logic
- [ ] Updated documentation
- [ ] No new warnings generated
```

### Review Process

1. Wait for maintainer review
2. Address feedback/suggestions
3. Make requested changes
4. Re-request review
5. Merge when approved

---

## Development Setup

### Clone and Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/perfex-cdsm-module.git
cd perfex-cdsm-module

# Create development branch
git checkout -b develop
```

### Directory Structure

```
perfex-cdsm-module/
├── controllers/        # Add new controllers here
├── models/            # Add new models here
├── views/             # Add new views here
├── helpers/           # Add helper functions
├── language/          # Language files
├── assets/            # CSS, JS files
├── migrations/        # Database migrations
└── documentation/     # Docs and guides
```

---

## Common Contributions

### Add New Document Type

1. Edit: `models/Cdsm_model.php`
2. Function: `get_document_types()`
3. Add entry:
   ```php
   'license_plate' => 'License Plate',
   ```

### Add New Email Template

1. Edit: `helpers/cdsm_email_helper.php`
2. Add method for template
3. Use in sending function

### Fix a Bug

1. Create issue describing bug
2. Create branch: `git checkout -b fix/bug-name`
3. Fix the bug
4. Add test cases
5. Create PR with "Fixes #123"

### Improve Documentation

1. Fork repository
2. Edit documentation file
3. Submit PR
4. Changes reviewed and merged

---

## Reporting Security Issues

**Do NOT** create public GitHub issues for security vulnerabilities.

Email: [security information pending]

Include:
- Description of vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if available)

---

## Questions?

- Check [FAQ.md](FAQ.md)
- Read [README.md](README.md)
- Review [API.md](API.md)
- Ask on GitHub Discussions (coming soon)

---

## Recognition

Contributors will be:
- Added to CONTRIBUTORS.md
- Mentioned in CHANGELOG.md
- Credited in release notes

---

<div align="center">

**Thank you for contributing! ❤️**

**[⬆ back to top](#-contributing-to-cdsm-module)**

</div>