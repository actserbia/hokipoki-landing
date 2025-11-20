# HokiPoki Landing Page

Marketing website for HokiPoki - The AI model hopping platform.

## Overview

Static HTML landing page featuring:
- Product overview and value propositions
- Interactive "HokiPoki Anthem" (yes, it's a children's song)
- Architecture diagram
- Email waitlist with Cloudflare Turnstile protection

## Configuration

### config.js

Edit `config.js` to configure the landing page for different environments:

```javascript
window.HOKIPOKI_CONFIG = {
    // API Base URL - Backend API endpoint
    // Development: 'http://localhost:3001'
    // Production: 'https://api.hoki-poki.ai'
    API_BASE_URL: 'http://localhost:3001',

    // Cloudflare Turnstile Site Key (public - safe to expose)
    TURNSTILE_SITE_KEY: '0x4AAAAAAB9xKd4hKsWSGfep'
};
```

**Important**:
- The site key is **public** and safe to expose in the browser
- The corresponding **secret key** lives in the backend `.env` file
- Change `API_BASE_URL` when deploying to production

## Local Development

### Running Locally

1. Serve the directory with any static file server:
   ```bash
   # Using Python
   python3 -m http.server 8000

   # Using Node.js
   npx http-server -p 8000

   # Using PHP
   php -S localhost:8000
   ```

2. Open http://localhost:8000

3. Ensure the backend is running at the configured `API_BASE_URL`

### Testing Waitlist Form

1. Backend must be running (`npm run dev` in hokipoki-backend/)
2. Backend must have valid `TURNSTILE_SECRET_KEY` in `.env`
3. Turnstile domain must be authorized in Cloudflare dashboard
4. Fill out email and submit form

## Features

### Interactive Elements

- **Anthem Preview**: Click the song box to make it dance
- **Full Anthem**: Click the lyrics to make them bounce
- **Smooth Scrolling**: Navigation links smoothly scroll to sections

### Waitlist Form

Protected by Cloudflare Turnstile (invisible captcha):
- Validates email format client-side
- Verifies Turnstile token server-side
- Stores emails in PostgreSQL database
- Prevents duplicate signups

### Design

- **Terminal aesthetic**: Pure black background with neon colors
- **JetBrains Mono font**: Monospace throughout
- **Responsive**: Mobile-friendly design
- **Accessible**: Proper semantic HTML and ARIA labels

## Deployment

### Production Checklist

1. **Update config.js**:
   ```javascript
   API_BASE_URL: 'https://api.hoki-poki.ai'
   ```

2. **Configure Cloudflare Turnstile**:
   - Add production domain to authorized domains
   - Update site key if using different Turnstile app

3. **Deploy Files**:
   - Upload all files including `config.js`
   - Ensure proper MIME types (`.js` as `application/javascript`)

4. **Configure CORS**:
   - Backend must allow origin from production domain
   - Update `CORS_ORIGINS` in backend `.env`

5. **Test**:
   - Verify form submission works
   - Check Turnstile validation
   - Confirm emails are saved to database

### Hosting Options

**Static Hosting** (recommended):
- Cloudflare Pages
- Netlify
- Vercel
- AWS S3 + CloudFront
- GitHub Pages

**Why static?**
- No server-side rendering needed
- Fast CDN delivery
- Low cost
- High scalability

## File Structure

```
hokipoki-landing/
├── index.html          # Main landing page
├── config.js           # Configuration (edit this!)
├── .gitignore         # Prevents committing sensitive files
├── README.md          # This file
├── package.json       # npm dependencies (for http-server)
└── package-lock.json
```

## Security

### What's Safe to Expose

✅ **Safe** (public):
- Turnstile site key (in `config.js`)
- API base URL
- All HTML/CSS/JavaScript

❌ **Never expose** (backend only):
- Turnstile secret key (in backend `.env`)
- Database credentials
- JWT secrets

### Content Security Policy

Consider adding CSP headers in production:

```
Content-Security-Policy:
  default-src 'self';
  script-src 'self' https://challenges.cloudflare.com 'unsafe-inline';
  style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
  font-src 'self' https://fonts.gstatic.com;
  connect-src 'self' https://api.hoki-poki.ai;
```

## Troubleshooting

### Form Not Submitting

**Check:**
1. Backend is running and accessible
2. `API_BASE_URL` in `config.js` is correct
3. CORS is configured in backend
4. Turnstile domain is authorized
5. Browser console for errors

### Turnstile Not Loading

**Solutions:**
- Check if Cloudflare Turnstile script is blocked
- Verify site key is correct in `config.js`
- Check domain is authorized in Cloudflare dashboard
- Try in different browser (adblockers may interfere)

### 400 Bad Request

**Causes:**
- Turnstile token validation failed
- Email format invalid
- Domain not authorized in Cloudflare

**Fix:**
- Ensure domain matches Cloudflare configuration
- Check backend logs for detailed error

## Related Services

- **hokipoki-backend** - Backend API that processes waitlist signups
- **hokipoki-dashboard** - Main application dashboard
- **hokipoki-cli** - CLI tool for the platform

## Maintenance

### Updating Content

1. Edit `index.html` directly (it's a single file)
2. Test locally
3. Deploy updated file

### Updating Configuration

1. Edit `config.js`
2. No rebuild needed (static)
3. Deploy updated file

## License

MIT
