
**4. Create config.php**
```bash
cat > config.php << 'EOF'
<?php
defined('BASEPATH') or exit('No direct script access allowed');

$config['name']              = 'Customer Document Management';
$config['version']           = '1.0.0';
$config['author']            = 'Virendra';
$config['author_url']        = 'https://github.com/virenderwwms-coder';
$config['module_url']        = 'https://github.com/virenderwwms-coder/perfex-cdsm-module';
$config['description']       = 'Manage customer documents with expiry tracking, automated reminders at 30, 15, 7 days before expiry. Color-coded status (Green, Yellow, Orange, Red) and staff notifications.';
$config['module_icon']       = 'fa fa-file-text';
$config['install_demo_data'] = false;
?>
EOF
