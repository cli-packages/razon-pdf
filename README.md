# Razon - Advanced CLI Email Client

A cutting-edge, enterprise-grade email automation platform developed by the **Razon Team**. Razon delivers unparalleled performance for bulk email campaigns, marketing automation, and professional email management with advanced templating, multi-provider support, and intelligent delivery optimization.

## Features

### 🚀 **Core Email Engine**
- **Multi-Provider SMTP Support**: Seamlessly integrate with unlimited SMTP providers including Gmail, Outlook, SendGrid, Mailgun, Amazon SES, and custom servers
- **Intelligent Failover System**: Automatic provider switching with health monitoring and performance optimization
- **High-Performance Delivery**: Concurrent email processing with configurable parallel connections for maximum throughput
- **Smart Rate Limiting**: Advanced throttling with hourly, daily, and per-minute limits to respect provider constraints

### 📧 **Advanced Email Composition**
- **Dynamic Template Engine**: Powerful HTML templating with 50+ built-in variables and custom tag support
- **Multi-Format Attachments**: Support for PDF, HTML, Images (PNG/JPG), SVG, and EML files with dynamic generation
- **Intelligent Content Processing**: Automatic image embedding, CSS inlining, and mobile-responsive optimization
- **Personalization Engine**: Real-time content customization with fake data generation and company lookup integration

### 🎯 **Campaign Management**
- **Bulk Email Processing**: Efficient batch processing with BCC, CC, and TO distribution modes
- **Test Mode Environment**: Safe testing with isolated recipient lists and comprehensive validation
- **Campaign Analytics**: Real-time delivery tracking, bounce handling, and performance metrics
- **Recipient Management**: CSV/TXT file support with advanced parsing and validation

### 🔒 **Security & Privacy**
- **Proxy Integration**: Full SOCKS5/HTTP proxy support for enhanced privacy and geographic distribution
- **Secure Authentication**: OAuth2, API key, and traditional SMTP authentication methods
- **Encrypted Attachments**: Password-protected PDF generation and secure file handling
- **Audit Logging**: Comprehensive security logs with configurable retention policies

### 🌐 **Enterprise Integration**
- **Outlook Web Access (OWA)**: Native support for Office 365 and Exchange Online accounts
- **Hunter.io Integration**: Automatic company name resolution and email verification
- **API-First Design**: RESTful endpoints for seamless integration with existing systems
- **CLI Interface**: Powerful command-line tools for automation and scripting

### 📊 **Monitoring & Analytics**
- **Real-Time Logging**: Multi-level logging with JSON, text, and structured formats
- **Performance Metrics**: Delivery rates, response times, and provider performance analytics
- **Error Handling**: Intelligent retry mechanisms with exponential backoff and dead letter queues
- **Health Monitoring**: System health checks and automated alerting

## Installation

### Prerequisites
- Modern runtime environment
- Version control system
- Administrative privileges for installation

### Quick Setup

1. **Clone the Razon repository**:
```bash
git clone <repository-url>
cd razon-email-client
```

2. **Install dependencies**:
```bash
# Install required dependencies
npm install
```

3. **Initialize configuration**:
```bash
# Copy example configurations
cp config/core.jsonc.example config/core.jsonc
cp config/smtp.jsonc.example config/smtp.jsonc
cp config/message.jsonc.example config/message.jsonc
```

4. **Build the project**:
```bash
# Build the application
npm run build
```

### Docker Installation (Alternative)

```bash
# Pull the official Razon image
docker pull razon/email-client:latest

# Run with volume mounting for configuration
docker run -v $(pwd)/config:/app/config razon/email-client:latest
```

## Quick Start

### 🎯 **Getting Started in 5 Minutes**

1. **Configure Your Email Provider**: 
   Edit `config/smtp.jsonc` with your SMTP server credentials:
   ```bash
   # Configure your email provider settings
   nano config/smtp.jsonc
   ```

2. **Design Your Email Campaign**: 
   Configure `config/message.jsonc` with your email content and templates:
   ```bash
   # Set up your email template and content
   nano config/message.jsonc
   ```

3. **Prepare Your Recipient List**: 
   Create a recipient file with email addresses:
   ```bash
   # Create recipients file (supports .txt and .csv)
   echo "recipient1@example.com" > recipients.txt
   echo "recipient2@example.com" >> recipients.txt
   ```

4. **Launch Your Campaign**:
   ```bash
   # Start Razon CLI
   npm start
   
   # Or run in test mode first
   npm run test-mode
   ```

### 🧪 **Test Mode (Recommended for First Run)**

Before sending to real recipients, always test your configuration:

```bash
# Enable test mode in config/core.jsonc
{
  "enableTestMode": true,
  "testLeadsFile": "test-recipients.txt",
  "testLeadsLimit": 5
}

# Create test recipients
echo "test@yourdomain.com" > test-recipients.txt

# Run test campaign
npm run test
```

### 📊 **Monitor Your Campaign**

```bash
# View real-time logs
tail -f mailer.log

# Check delivery status
npm run status

# View campaign analytics
npm run analytics
```

## Configuration

Razon uses a modular JSONC configuration system that provides maximum flexibility and maintainability. All configuration files are located in the `config/` directory and support comments for better documentation.

### 📁 **Configuration Structure**

```
config/
├── core.jsonc              # Core application settings and global preferences
├── smtp.jsonc              # SMTP server configurations and provider settings
├── message.jsonc           # Email message templates and content settings
├── attachments.jsonc       # Attachment configurations and processing rules
├── addon/
│   └── owa.jsonc          # Outlook Web Access integration settings
└── eml/
    └── attachments.jsonc  # EML-specific attachment configurations
```

### ⚙️ **Core Configuration (`config/core.jsonc`)**

The central configuration file that controls Razon's core functionality and global behavior:

```json
{
    "name": "Razon Advanced Email Client",
    "enableTestMode": false,
    "testLeadsFile": "test-leads.txt",
    "testLeadsLimit": 5,
    "useBulkSending": false,
    "bulkSending": {
        "batchSize": 50,
        "useBccForSending": true,
        "useCcForSending": false,
        "useToForSending": false
    },
    "enableSleepBetweenEmails": false,
    "delayDurationInSeconds": 10,
    "emailsBeforeSleep": 1000,
    "randomizeEmailServers": true,
    "proxy": {
        "useProxy": false,
        "host": "localhost",
        "port": 8080,
        "user": "user",
        "pass": "pass",
        "protocol": "socks5",
        "useAuthentication": false
    },
    "parallel": {
        "parallelConnections": 10,
        "enableParallelSending": true
    },
    "logging": {
        "enabled": true,
        "level": "info",
        "logFile": "mailer.log",
        "format": "text",
        "logFailed": true,
        "logSuccess": true
    },
    "HUNTER_API_KEY": [
        "your-hunter-api-key"
    ]
}
```

### 📧 **SMTP Configuration (`config/smtp.jsonc`)**

Configure multiple email providers with intelligent load balancing and failover capabilities:

```json
{
    "randomizeEmailServers": true,
    "accounts": [
        {
            "useMe": true,
            "host": "smtp.resend.com",
            "port": 587,
            "secure": false,
            "user": "resend",
            "pass": "your-api-key",
            "fromEmail": "campaigns@yourdomain.com",
            "fromName": "Your Campaign Name",
            "useAuth": true,
            "dailyLimit": 10000,
            "retryFailed": false,
            "retryCount": 2,
            "useProxy": false,
            "options": {
                "enableHourlyLimit": false,
                "sendPerHour": 100,
                "enableMinuteLimit": false,
                "sendPer10Minutes": 10
            },
            "advance": {
                "pool": false,
                "maxConnections": 5,
                "maxMessages": 100
            }
        }
    ]
}
```

### 📝 **Message Configuration (`config/message.jsonc`)**

Design sophisticated email campaigns with dynamic content and advanced personalization:

```json
{
    "subject": "Your Personalized Campaign Subject",
    "fromEmail": "campaigns@yourdomain.com",
    "priority": "normal",
    "fromName": "",
    "yourLinks": [],
    "linksFromFile": "../links.txt",
    "useLinkFromFile": false,
    "subjects": {
        "enabled": false,
        "subjectsPath": "subject.txt",
        "offset": 0,
        "mode": "sequence",
        "countBeforeSubject": 500,
        "startFresh": false
    },
    "fromNames": {
        "enabled": false,
        "namesPath": "from-names.txt",
        "offset": 0,
        "mode": "sequence",
        "countBeforeName": 500,
        "startFresh": false
    },
    "recipient_file": "recipients.txt",
    "body": {
        "useHTML": true,
        "latterPath": "letter.html",
        "image": {
            "embedInLetter": false,
            "imagePath": "image.png",
            "alt": "Campaign Image",
            "strictMode": true,
            "style": {
                "useStyle": false,
                "width": "100%",
                "height": "100%",
                "marginTop": "0px",
                "marginBottom": "0px",
                "marginLeft": "0px",
                "marginRight": "0px",
                "paddingTop": "0px",
                "paddingBottom": "0px",
                "paddingLeft": "0px",
                "paddingRight": "0px",
                "borderRadius": "0px"
            }
        }
    }
}
```

### 📎 **Attachment Configuration (`config/attachments.jsonc`)**

Configure sophisticated file attachments with dynamic generation and multi-format support:

```json
{
    "useAttachment": true,
    "throwErrorOnInvalidAttachment": true,
    "replaceTagsInAttachment": true,
    "attachments": [{
        "useMe": true,
        "format": "PDF",
        "attachmentPath": "letterAttachment.html",
        "attachmentNameWithPlaceholders": "Document_{UNIQUE_ID}.pdf",
        "pdf": {
            "usePassword": false,
            "password": "secret",
            "inlineAttachment": false,
            "landscape": false,
            "printBackground": true,
            "format": "A4",
            "margin": {
                "top": 1,
                "left": 1,
                "bottom": 1,
                "right": 1
            }
        },
        "image": {
            "inlineAttachment": false,
            "type": "jpeg",
            "format": "A4",
            "landscape": true,
            "margin": {
                "top": 0,
                "left": 0,
                "bottom": 0,
                "right": 0
            },
            "printBackground": true,
            "fitToPaperMode": "Default",
            "optimizeForSpeed": true,
            "quality": 80
        },
        "eml": {
            "useMessageConfig": true,
            "fromEmail": "campaigns@yourdomain.com",
            "fromName": "Campaign Team",
            "subject": "Attached Email Message",
            "priority": "normal",
            "attachment": {
                "useAttachment": false,
                "throwErrorOnInvalidAttachment": false
            },
            "headers": {
                "X-Mailer": "Razon Email Client"
            }
        }
    }]
}
```

### 🌐 **OWA Configuration (`config/addon/owa.jsonc`)**

Configure Outlook Web Access integration for seamless Office 365 and Exchange Online email sending:

```json
{
    "randomizeEmailServers": true,
    "accounts": [{
        "useMe": false,
        "senderEmail": "your-email@company.com",
        "webmail_url": "https://outlook.office.com/mail/",
        "cookie_string": "your-session-cookies",
        "deleteOnSent": true,
        "dailyLimit": 10000,
        "retryFailed": false,
        "retryCount": 2,
        "useProxy": false,
        "options": {
            "enableHourlyLimit": false,
            "sendPerHour": 100,
            "enableMinuteLimit": false,
            "sendPer10Minutes": 10
        }
    }]
}
```

#### **OWA Integration Features:**
- **Native Office 365 Support**: Direct integration with Microsoft Outlook Web Access
- **Session Management**: Automatic cookie handling and session persistence
- **Enterprise Security**: Maintains corporate security policies and compliance
- **Bulk Operations**: Efficient handling of large-scale email campaigns
- **Rate Limiting**: Respects Exchange Online throttling policies
- **Auto-cleanup**: Optional sent item deletion for storage management

#### **Setting Up OWA Integration:**
1. **Obtain Session Cookies**: Login to your Outlook Web Access and extract session cookies
2. **Configure Account**: Add your corporate email and webmail URL
3. **Set Limits**: Configure daily and hourly sending limits per your organization's policies
4. **Test Connection**: Verify connectivity before launching campaigns

## Template System

### HTML Templates

Create rich HTML email templates with dynamic content:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>{SUBJECT}</title>
    <style>
        body { font-family: Arial, sans-serif; }
        .header { background-color: #f0f0f0; padding: 20px; }
        .content { padding: 20px; }
        .footer { background-color: #333; color: white; padding: 10px; }
    </style>
</head>
<body>
    <div class="header">
        <h1>Welcome {FAKE_FIRST_NAME}!</h1>
    </div>
    
    <div class="content">
        <p>Dear {FAKE_FIRST_NAME} {FAKE_LAST_NAME},</p>
        
        <p>Thank you for your interest in our services. This email was sent on {DATE} at {TIME}.</p>
        
        <p>Your unique reference ID is: <strong>{UNIQUE_ID}</strong></p>
        
        <p>Company: {FAKE_COMPANY_NAME}</p>
        <p>Phone: {FAKE_PHONE_NUMBER}</p>
        <p>Address: {FAKE_ADDRESS}</p>
        
        <a href="{PAGE_LINK}" style="background-color: #007cba; color: white; padding: 10px 20px; text-decoration: none; border-radius: 5px;">
            Visit Our Website
        </a>
    </div>
    
    <div class="footer">
        <p>© 2024 Your Company. All rights reserved.</p>
        <p>Random Number: {RANDOM_NUMBER_5}</p>
    </div>
</body>
</html>
```

## Tag System

The application includes a powerful tag replacement system with 50+ built-in tags for dynamic content generation.

### Basic Tags

| Tag | Description | Example Output |
|-----|-------------|----------------|
| `{EMAIL_ADDRESS}` | Recipient's email address | `john.doe@example.com` |
| `{UNIQUE_ID}` | Unique identifier | `abc123def456` |
| `{DATE}` | Current date | `2024-01-15` |
| `{TIME}` | Current time | `14:30:25` |
| `{TOMORROW_DATE}` | Tomorrow's date | `2024-01-16` |
| `{YESTERDAY_DATE}` | Yesterday's date | `2024-01-14` |

### Fake Data Tags

| Tag | Description | Example Output |
|-----|-------------|----------------|
| `{FAKE_FIRST_NAME}` | Random first name | `John` |
| `{FAKE_LAST_NAME}` | Random last name | `Smith` |
| `{FAKE_FULL_NAME}` | Random full name | `John Smith` |
| `{FAKE_COMPANY_NAME}` | Random company name | `Tech Solutions Inc` |
| `{FAKE_EMAIL}` | Random email address | `john.smith@example.com` |
| `{FAKE_PHONE_NUMBER}` | Random phone number | `+1-555-123-4567` |
| `{FAKE_ADDRESS}` | Random street address | `123 Main Street` |
| `{FAKE_CITY}` | Random city name | `New York` |
| `{FAKE_STATE}` | Random state name | `California` |
| `{FAKE_ZIP_CODE}` | Random ZIP code | `90210` |
| `{FAKE_COUNTRY}` | Random country name | `United States` |

### Random Number Tags

| Tag | Description | Example Output |
|-----|-------------|----------------|
| `{RANDOM_NUMBER_3}` | 3-digit random number | `847` |
| `{RANDOM_NUMBER_5}` | 5-digit random number | `92847` |
| `{RANDOM_NUMBER_8}` | 8-digit random number | `38472951` |

### Special Tags

| Tag | Description | Example Output |
|-----|-------------|----------------|
| `{PAGE_LINK}` | Dynamic page link | `https://example.com` |
| `{REAL_COMPANY_NAME}` | Real company from domain | `Google Inc` |

## Best Practices

### 1. SMTP Security
- Use environment variables for sensitive credentials
- Enable two-factor authentication where possible
- Regularly rotate passwords and API keys

### 2. Rate Limiting
- Monitor your SMTP provider's rate limits
- Configure appropriate delays between emails
- Use multiple SMTP accounts for load balancing

### 3. Content Management
- Test email templates in multiple clients
- Use inline CSS for better compatibility
- Optimize images to keep email size manageable

### 4. Attachment Management
- Limit attachment size and number
- Use cloud storage links for large files
- Encrypt sensitive attachments when necessary

### 5. Logging and Monitoring
- Enable comprehensive logging for troubleshooting
- Monitor logs for delivery failures
- Implement log rotation to manage disk usage

## License

MIT License - Feel free to use this project in your own applications, both personal and commercial.

---

**Made with ❤️ by Razon Team**
