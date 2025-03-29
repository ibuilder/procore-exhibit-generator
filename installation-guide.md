# Procore Exhibit Generator - Installation Guide

This guide provides detailed instructions for setting up and deploying the Procore Exhibit Generator application.

## Server Requirements

- Web server (Apache, Nginx, IIS, etc.)
- HTTPS support (required for Procore OAuth)
- No server-side scripting required (pure static application)

## Installation Steps

### 1. Download the Application

Download the application files from the repository or extract the provided ZIP file to your web server's document root or a subdirectory.

```bash
# If using Git
git clone https://github.com/yourusername/procore-exhibit-generator.git
cd procore-exhibit-generator

# If deploying to a production environment, you may want to remove Git files
rm -rf .git
```

### 2. Configure Your Web Server

#### Apache Configuration Example

```apache
<VirtualHost *:443>
    ServerName exhibit-generator.yourdomain.com
    DocumentRoot /path/to/procore-exhibit-generator
    
    <Directory /path/to/procore-exhibit-generator>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    # SSL Configuration
    SSLEngine on
    SSLCertificateFile /path/to/certificate.crt
    SSLCertificateKeyFile /path/to/private.key
    SSLCertificateChainFile /path/to/ca-bundle.crt
    
    # Additional security headers
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set X-Content-Type-Options "nosniff"
</VirtualHost>
```

#### Nginx Configuration Example

```nginx
server {
    listen 443 ssl http2;
    server_name exhibit-generator.yourdomain.com;
    root /path/to/procore-exhibit-generator;
    index index.html;
    
    # SSL Configuration
    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;
    ssl_trusted_certificate /path/to/ca-bundle.crt;
    
    # Additional security headers
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
    
    # Cache static assets
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 30d;
        add_header Cache-Control "public, no-transform";
    }
}
```

### 3. Register Your Application with Procore

1. Go to the [Procore Developer Portal](https://developers.procore.com/)
2. Sign in with your Procore account
3. Create a new application
   - Name: "Exhibit Generator" (or any name you prefer)
   - Description: "Generates exhibit documents from Procore data"
   - Company Name: Your company name
4. Configure the OAuth settings:
   - Redirect URI: `https://exhibit-generator.yourdomain.com/callback.html`
   - Scopes: Select the following scopes:
     - `projects.read`
     - `bid_packages.read`
     - `specifications.read`
     - `drawings.read`
     - `rfis.read`
     - `submittals.read`
     - `me.read`
5. Note your Client ID and Client Secret - you'll need these for the application

### 4. Test the Application

1. Open your browser and navigate to `https://exhibit-generator.yourdomain.com`
2. Go to the Settings page
3. Enter your Procore API credentials (Client ID and Client Secret)
4. Click "Authorize with Procore" to complete the OAuth authentication
5. Once authenticated, test the application by generating an exhibit

## Troubleshooting

### OAuth Authentication Issues

- Ensure your redirect URI exactly matches what you configured in the Procore Developer Portal
- Verify that your site uses HTTPS (OAuth requires secure connections)
- Check browser console for any JavaScript errors
- Make sure your Procore account has access to the necessary projects

### CORS Issues

If you encounter Cross-Origin Resource Sharing (CORS) errors:

1. Ensure you're accessing the application via the correct domain
2. Verify that the Procore API is properly configured to allow requests from your domain
3. Check that your web server isn't stripping or modifying Origin headers

### PDF/DOCX Generation Problems

- PDF generation requires the browser to support HTML5 Canvas
- DOCX generation requires modern JavaScript features (ES6+)
- Large documents might cause memory issues in older browsers

## Security Considerations

- Store client secrets securely and avoid committing them to version control
- Implement proper Content Security Policy (CSP) headers
- Consider adding rate limiting if the application will be used by many users
- Regularly update the application's dependencies to patch security vulnerabilities

## Customizing the Application

### Modifying the Look and Feel

The application uses Bootstrap for styling. You can customize the appearance by:

1. Editing the CSS in `styles.css`
2. Modifying the HTML templates in the various `.html` files
3. Adding your company logo by replacing any image files

### Adding Custom Templates

You can add predefined templates by modifying the `database-service.js` file:

1. Locate the `initialize()` method
2. Add your custom templates to the default templates array

```javascript
// Example of adding a custom template
const defaultTemplates = [
  {
    name: "Fire Protection Exhibit",
    division: "Division 21",
    specSections: "019113 – General Commissioning Requirements\n019114 – Functional Testing Requirements\nAll Division 21",
    tradeSpecificItems: "Complete wet and dry sprinkler systems installation\nFire pump and fire pump controller"
  },
  // Add more templates here
];
```

## Updating the Application

To update the application to a newer version:

1. Back up your current configuration and any customizations
2. Download or pull the new version
3. Apply your customizations to the new version
4. Test thoroughly before deploying to production

## Support

For questions or issues regarding this application, please contact your system administrator or the developer who provided the application.