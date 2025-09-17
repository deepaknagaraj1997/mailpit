# Mailpit SMTP Server

A development SMTP server using [Mailpit](https://github.com/axllent/mailpit) for capturing and viewing emails during development and testing.

## Overview

Mailpit is a lightweight email testing tool that captures emails sent by your applications and displays them in a web interface. Perfect for development environments where you don't want to send real emails.

## Quick Start

### 1. Start Mailpit

```bash
docker-compose up -d
```

### 2. Access Web Interface

Open your browser and go to: **http://localhost:8025**

### 3. Configure Your Application

Use these SMTP settings in your application:

```env
SMTP_HOST=localhost
SMTP_FROM=noreply@yourapp.com
SMTP_PORT=1025
SMTP_SSL=false
SMTP_USERNAME=any-username
SMTP_PASSWORD=any-password
```

## SMTP Configuration Examples

### For Local Applications

```env
SMTP_HOST=localhost
SMTP_PORT=1025
SMTP_SSL=false
SMTP_TLS=false
SMTP_USERNAME=test
SMTP_PASSWORD=test
```

### For Docker Applications (Same Network)

If your application runs in Docker and uses the same network:

```env
SMTP_HOST=mailpit
SMTP_PORT=1025
SMTP_SSL=false
SMTP_TLS=false
```

### For Docker Applications (Different Network)

```env
SMTP_HOST=host.docker.internal
SMTP_PORT=1025
SMTP_SSL=false
SMTP_TLS=false
```

## Language-Specific Examples

### Node.js (Nodemailer)

```javascript
const nodemailer = require('nodemailer');

const transporter = nodemailer.createTransporter({
  host: 'localhost',
  port: 1025,
  secure: false, // no SSL/TLS
  auth: {
    user: 'any-username',
    pass: 'any-password'
  }
});

// Send email
transporter.sendMail({
  from: 'sender@example.com',
  to: 'recipient@example.com',
  subject: 'Test Email',
  text: 'Hello from Mailpit!'
});
```

### Python (smtplib)

```python
import smtplib
from email.mime.text import MIMEText

# Create message
msg = MIMEText('Hello from Mailpit!')
msg['Subject'] = 'Test Email'
msg['From'] = 'sender@example.com'
msg['To'] = 'recipient@example.com'

# Send email
with smtplib.SMTP('localhost', 1025) as server:
    server.login('any-username', 'any-password')
    server.send_message(msg)
```

### PHP (PHPMailer)

```php
use PHPMailer\PHPMailer\PHPMailer;
use PHPMailer\PHPMailer\SMTP;

$mail = new PHPMailer(true);

$mail->isSMTP();
$mail->Host       = 'localhost';
$mail->SMTPAuth   = true;
$mail->Username   = 'any-username';
$mail->Password   = 'any-password';
$mail->SMTPSecure = false;
$mail->Port       = 1025;

$mail->setFrom('sender@example.com');
$mail->addAddress('recipient@example.com');
$mail->Subject = 'Test Email';
$mail->Body    = 'Hello from Mailpit!';

$mail->send();
```

### Laravel (.env)

```env
MAIL_MAILER=smtp
MAIL_HOST=localhost
MAIL_PORT=1025
MAIL_USERNAME=any-username
MAIL_PASSWORD=any-password
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS=noreply@yourapp.com
MAIL_FROM_NAME="Your App"
```

### Django (settings.py)

```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'localhost'
EMAIL_PORT = 1025
EMAIL_USE_TLS = False
EMAIL_USE_SSL = False
EMAIL_HOST_USER = 'any-username'
EMAIL_HOST_PASSWORD = 'any-password'
DEFAULT_FROM_EMAIL = 'noreply@yourapp.com'
```

## Management Commands

### Start Services
```bash
docker-compose up -d
```

### Stop Services
```bash
docker-compose down
```

### View Logs
```bash
docker-compose logs -f mailpit
```

### Restart Services
```bash
docker-compose restart
```

## Ports

- **1025**: SMTP Server (for sending emails)
- **8025**: Web UI (for viewing emails)

## Features

- ✅ Captures all emails sent to the SMTP server
- ✅ Web-based email viewer
- ✅ No authentication required (accepts any credentials)
- ✅ Supports attachments
- ✅ Search and filter emails
- ✅ Mobile-responsive interface
- ✅ API access for automation
- ✅ Persistent email storage

## Troubleshooting

### Connection Refused
- Ensure Mailpit is running: `docker-compose ps`
- Check if port 1025 is available: `netstat -an | grep 1025`

### Emails Not Appearing
- Check the web interface at http://localhost:8025
- Verify your application is sending to the correct host and port
- Check application logs for SMTP errors

### Docker Network Issues
- If using Docker, ensure containers are on the same network
- Use `host.docker.internal` instead of `localhost` for cross-container communication

## Configuration

The docker-compose.yml includes these environment variables:

- `MP_SMTP_AUTH_ACCEPT_ANY=1`: Accept any SMTP authentication
- `MP_SMTP_AUTH_ALLOW_INSECURE=1`: Allow insecure authentication methods

## Security Note

⚠️ **This is for development only!** Mailpit accepts any authentication and should never be exposed to the internet or used in production environments.

## Links

- [Mailpit GitHub Repository](https://github.com/axllent/mailpit)
- [Mailpit Documentation](https://mailpit.axllent.org/)
- [Docker Hub Image](https://hub.docker.com/r/axllent/mailpit)