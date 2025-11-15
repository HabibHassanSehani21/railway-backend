# PDF Tools API - Backend

FastAPI backend for PDF processing tools (image conversion, merging, compression).

## Features

- **Image to PDF**: Convert JPG, PNG, GIF, BMP images to PDF
- **Merge PDF**: Combine multiple PDF files into one
- **Compress PDF**: Reduce PDF file size with customizable options
- **Automatic cleanup**: Files are automatically deleted after 1 hour
- **CORS enabled**: Ready for cross-origin requests

## Tech Stack

- Python 3.11
- FastAPI
- Uvicorn (ASGI server)
- Pillow (image processing)
- PyPDF2 (PDF manipulation)

## Local Development

### Prerequisites

- Python 3.11 or higher
- pip

### Setup

1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Run the server:
```bash
python main.py
```

The API will be available at `http://localhost:8000`

### API Documentation

Once running, visit:
- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

## Deploy to Railway.app

### Quick Deploy

1. **Create Railway Account**
   - Sign up at https://railway.app

2. **Deploy from GitHub**
   - Push this code to a GitHub repository
   - In Railway dashboard, click "New Project"
   - Select "Deploy from GitHub repo"
   - Choose your repository
   - Railway will auto-detect Python and deploy

3. **Generate Public URL**
   - Click on your deployed service
   - Go to Settings → Networking
   - Click "Generate Domain"
   - Copy your URL (e.g., `https://your-api-production.up.railway.app`)

### Configuration Files

This repository includes Railway configuration:

- **`.python-version`**: Specifies Python 3.11
- **`railway.json`**: Deployment configuration
- **`requirements.txt`**: Python dependencies

### Environment Variables

No environment variables required for basic deployment. Add these in Railway dashboard if needed:

- `PORT`: Automatically provided by Railway
- Custom variables: Add in Railway dashboard → Variables tab

### Update CORS (Important!)

After deploying, update the `allow_origins` list in `main.py` to include your frontend domain:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:5000",           # Local development
        "http://localhost:3000",           # Next.js dev
        "https://your-app.vercel.app",     # Your frontend domain
        "https://*.vercel.app",            # Vercel preview deployments
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

Push the changes and Railway will automatically redeploy.

## API Endpoints

### Health Check
```
GET /health
```
Returns server health status.

### Image to PDF
```
POST /api/image-to-pdf
```
Convert multiple images to a single PDF file.

**Request**: multipart/form-data with image files  
**Supported formats**: JPG, PNG, GIF, BMP  
**Response**: PDF file download

### Merge PDFs
```
POST /api/merge-pdf
```
Merge multiple PDF files into one.

**Request**: multipart/form-data with PDF files (minimum 2)  
**Response**: Merged PDF file download

### Compress PDF
```
POST /api/compress-pdf
```
Compress a PDF file with customizable options.

**Request**: multipart/form-data with PDF file  
**Query Parameters**:
- `dpi` (optional): Image DPI (default: 144)
- `image_quality` (optional): JPEG quality 1-100 (default: 75)
- `color_mode` (optional): "no-change", "grayscale", or "bw" (default: "no-change")

**Response**: Compressed PDF file download

## File Storage

- **Uploads**: Temporary storage in `uploads/` directory
- **Output**: Generated files in `temp/` directory
- **Cleanup**: Both directories are cleaned every 5 minutes (files older than 1 hour are deleted)

## Alternative Deployment Options

### Render.com

1. Create account at https://render.com
2. Create new Web Service
3. Connect GitHub repository
4. Configure:
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Deploy

### Fly.io

```bash
# Install Fly CLI
curl -L https://fly.io/install.sh | sh

# Deploy
fly launch
fly deploy
```

### Google Cloud Run

1. Create `Dockerfile`:
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install --no-cache-dir -r requirements.txt
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "$PORT"]
```

2. Deploy via Google Cloud Console or CLI

## Monitoring

### Railway Logs

```bash
# Install Railway CLI
npm i -g @railway/cli

# Login
railway login

# View logs
railway logs
```

### Health Monitoring

Set up health check monitoring using the `/health` endpoint.

## Security Notes

- CORS should be restricted to your frontend domain in production
- Files are automatically cleaned up after 1 hour
- No permanent file storage
- Input validation on all endpoints
- File type verification before processing

## License

This is an open-source project for PDF processing tools.

## Support

For issues or questions about Railway deployment:
- Railway Docs: https://docs.railway.app
- Railway Discord: https://discord.gg/railway

For issues with the API itself, please create an issue in the repository.
