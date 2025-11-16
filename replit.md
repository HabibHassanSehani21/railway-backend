# PDF Tools API Backend

## Overview
FastAPI backend service for PDF processing tools. Provides REST API endpoints for image-to-PDF conversion, PDF merging, and PDF compression operations.

**Current Status**: Backend API running in Replit development environment. Production deployment on Azure.

## Project Structure
```
.
├── main.py                    # FastAPI application entry point
├── tools/                     # PDF processing modules
│   ├── __init__.py
│   ├── image_to_pdf.py       # Image to PDF conversion
│   ├── merge_pdf.py          # PDF merging functionality
│   └── compress_pdf.py       # PDF compression
├── requirements.txt          # Python dependencies
├── uploads/                  # Temporary upload storage (auto-created)
├── temp/                     # Temporary output files (auto-created)
├── DEPLOYMENT_HETZNER.md    # Hetzner deployment guide
└── README.md                 # Project documentation
```

## Technology Stack
- **Framework**: FastAPI 0.109.0
- **Server**: Uvicorn 0.27.0 (ASGI)
- **Image Processing**: Pillow 10.3.0
- **PDF Manipulation**: PyPDF2 3.0.1
- **Python**: 3.11

## API Endpoints

### Health Check
- `GET /` - API info
- `GET /health` - Health status

### PDF Tools
- `POST /api/image-to-pdf` - Convert images to PDF
- `POST /api/merge-pdf` - Merge multiple PDFs
- `POST /api/compress-pdf` - Compress PDF with options

### Interactive Documentation
- `/docs` - Swagger UI
- `/redoc` - ReDoc documentation

## Features
- ✅ Image to PDF conversion (JPG, PNG, GIF, BMP)
- ✅ PDF merging (multiple files)
- ✅ PDF compression with customizable options (DPI, quality, color mode)
- ✅ Automatic file cleanup (1-hour retention)
- ✅ CORS enabled for cross-origin requests
- ✅ File type validation
- ✅ Error handling and logging

## Development Environment

### Running the Backend
The backend runs on port 8000 via the `backend` workflow:
```bash
python main.py
```

Server binds to `0.0.0.0:8000` (configurable via PORT environment variable).

### File Storage
- **Uploads**: Temporary storage for uploaded files
- **Temp**: Generated output files
- **Cleanup**: Automatic deletion after 1 hour (background task)

## Configuration

### CORS Settings
Currently allows all origins (`allow_origins=["*"]`). 

**For production**, update `main.py` to restrict origins:
```python
allow_origins=[
    "https://yourdomain.com",
    "https://your-frontend.vercel.app"
]
```

### Environment Variables
- `PORT` - Server port (default: 8000)

## Deployment

### Azure (Current Production)
This backend is currently deployed on Azure.

### Hetzner Cloud (Alternative)
Complete Hetzner deployment guide available in `DEPLOYMENT_HETZNER.md`, including:
- Server setup in Germany
- NGINX reverse proxy configuration
- SSL/TLS with Let's Encrypt
- Systemd service management
- Redis & Celery setup
- Security hardening
- Monitoring and maintenance

## Recent Changes
- 2024-11-16: Initial setup in Replit environment
- Added Hetzner deployment documentation
- Configured Python 3.11 environment
- Installed all required dependencies

## Notes
- Uses FastAPI's deprecated `@app.on_event("startup")` (warning in logs) - consider migrating to lifespan events
- File retention is 1 hour by default
- No authentication/authorization implemented (add if needed for production)
- GDPR-compliant automatic file deletion

## User Preferences
- Production deployment: Azure
- Deployment documentation: Hetzner Cloud guide created
