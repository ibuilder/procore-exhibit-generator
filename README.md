# Procore Exhibit Generator

A web application that connects to Procore via the API to generate standardized exhibit documents for construction contracts. The application allows users to create, preview, and export exhibits to PDF and DOCX formats.

## Features

- **Procore API Integration**: Pull project data directly from your Procore account
- **Customizable Templates**: Create and save exhibit templates for reuse
- **Export Options**: Export generated exhibits as PDF or DOCX files
- **Responsive Design**: Works on desktop and mobile devices
- **Local Storage**: Save your settings and recent exhibits

## Installation

### Prerequisites

- Web server (Apache, Nginx, etc.) or static hosting service
- Procore API credentials (Client ID, Client Secret)
- Modern web browser

### Setup Instructions

1. Clone or download this repository to your web server directory
2. Register your application in the [Procore Developer Portal](https://developers.procore.com/)
3. Set up the OAuth credentials and redirect URI in your Procore Developer Portal
4. Launch the application and go to the Settings page to enter your API credentials

```bash
# Example deployment using any static file server
cd /path/to/webserver/directory
git clone https://github.com/yourusername/procore-exhibit-generator.git
cd procore-exhibit-generator
# If using npm static server
npx serve
```

## Directory Structure

```
procore-exhibit-generator/
├── index.html              # Landing page
├── generator.html          # Main exhibit generator page
├── settings.html           # API configuration page
├── preview.html            # Exhibit preview page
├── callback.html           # OAuth callback handler
├── js/
│   ├── procore-api.js      # Procore API service
│   ├── exhibit-generator.js # Main application logic
│   ├── database-service.js # Local storage service
│   └── export-utils.js     # PDF/DOCX export utilities
├── css/
│   └── styles.css          # Custom styles
└── README.md               # This file
```

## Procore API Integration

This application uses the Procore REST API to access project data. The key endpoints used include:

- `/rest/v1.0/projects` - Get all accessible projects
- `/rest/v1.0/projects/{project_id}/bid_packages` - Get bid packages for a project
- `/rest/v1.0/projects/{project_id}/specifications` - Get specifications for a project

Authentication is handled via OAuth 2.0. Users need to authorize the application to access their Procore data.

## Using the Application

1. **Configure API Settings**
   - Go to the Settings page
   - Enter your Procore API credentials
   - Click "Authorize with Procore" to complete OAuth authentication

2. **Generate an Exhibit**
   - Go to the Generator page
   - Select a project and bid package from your Procore account
   - Customize the exhibit content
   - Click "Preview Exhibit" to see how it will look

3. **Export the Exhibit**
   - Preview the generated exhibit
   - Choose to export as PDF or DOCX
   - Save the file to your computer

## Customization

### Adding New Templates

You can create and save custom templates by modifying the structure in the `exhibit-generator.js` file. Templates are stored in the browser's local storage.

### Modifying the Exhibit Structure

The structure of the exhibit document is defined in the `generateExhibitHtml` function in `exhibit-generator.js`. You can modify this function to change the layout, styling, and content of the generated exhibits.

## Browser Compatibility

This application is compatible with modern browsers that support ES6, including:

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- [Bootstrap](https://getbootstrap.com/) - UI framework
- [docx.js](https://docx.js.org/) - DOCX generation
- [jsPDF](https://github.com/parallax/jsPDF) - PDF generation
- [FileSaver.js](https://github.com/eligrey/FileSaver.js/) - File download functionality
- [Procore API](https://developers.procore.com/) - Construction project management API