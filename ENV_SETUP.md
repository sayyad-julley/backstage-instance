# Environment Variables Setup

This directory contains environment variable configuration for Backstage.

## Files

- `.env.example` - Template file with all required environment variables (committed to git)
- `.env` - Your actual environment variables (gitignored, not committed)

## Setup

1. **Copy the example file:**
   ```bash
   cp .env.example .env
   ```

2. **Edit `.env` and fill in your values:**
   ```bash
   nano .env
   # or
   vi .env
   ```

## Required Variables

### GitHub OAuth (Required for Authentication)
- `GITHUB_CLIENT_ID` - From GitHub OAuth App
- `GITHUB_CLIENT_SECRET` - From GitHub OAuth App

### PostgreSQL (For Local Development)
- `POSTGRES_HOST` - Database host (default: localhost)
- `POSTGRES_PORT` - Database port (default: 5432)
- `POSTGRES_USER` - Database user (default: backstage)
- `POSTGRES_PASSWORD` - Database password
- `POSTGRES_DB` - Database name (default: backstage_plugin_catalog)

### Backstage URLs (For Local Development)
- `APP_BASE_URL` - Frontend URL (default: http://localhost:3000)
- `BACKEND_BASE_URL` - Backend URL (default: http://localhost:7007)
- `CORS_ORIGIN` - CORS origin (default: http://localhost:3000)

## Usage

### Local Development
The `.env` file in this directory is automatically used when running Backstage locally.

### Docker Deployment
When using docker-compose, the `.env` file is automatically loaded:
1. First checks for `.env` in the docker-compose directory
2. Falls back to `.env` in backstage-instance directory
3. Uses environment variables as final fallback

### EC2 Deployment
On EC2, create a `.env` file in `/home/ec2-user/apps/` with production values:
```bash
# On EC2
cd /home/ec2-user/apps
nano .env
```

## Security

- ⚠️ **Never commit `.env` to git** - it contains sensitive credentials
- ✅ `.env` is already in `.gitignore`
- ✅ `.env.example` is safe to commit (contains no secrets)

## Getting GitHub OAuth Credentials

1. Go to: https://github.com/settings/developers
2. Click "New OAuth App"
3. Fill in:
   - Application name: Backstage
   - Homepage URL: Your Backstage URL
   - Authorization callback URL: `https://your-domain/backstage/api/auth/github/handler/frame`
4. Copy Client ID and Client Secret
5. Add them to your `.env` file
