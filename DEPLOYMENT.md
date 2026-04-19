# Deployment Guide - Weather Runner 🚀

Это руководство поможет вам развернуть Weather Runner в production.

## 📋 Table of Contents

- [Quick Deploy](#quick-deploy)
- [GitHub Pages](#github-pages)
- [Netlify](#netlify)
- [Vercel](#vercel)
- [Custom Server](#custom-server)
- [Performance Optimization](#performance-optimization)
- [Monitoring](#monitoring)

---

## Quick Deploy

### Option 1: Direct File Hosting

Самый простой способ - загрузить `index.html` на любой статический хостинг:

1. Загрузите `index.html` на хостинг
2. Откройте URL в браузере
3. Готово! ✅

**Recommended hosts:**
- GitHub Pages (free)
- Netlify (free)
- Vercel (free)
- Firebase Hosting (free tier)

---

## GitHub Pages

### Step-by-Step

1. **Create repository**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/username/weather-runner.git
   git push -u origin main
   ```

2. **Enable GitHub Pages**
   - Go to repository Settings
   - Scroll to "Pages" section
   - Select source: "Deploy from a branch"
   - Branch: main / root
   - Click Save

3. **Access your game**
   ```
   https://username.github.io/weather-runner/
   ```

### Custom Domain (Optional)

1. Add CNAME file with your domain
2. Configure DNS records with your registrar
3. Wait for propagation (up to 48 hours)

---

## Netlify

### Method 1: Drag & Drop

1. Go to [netlify.com](https://netlify.com)
2. Sign up/login
3. Drag your project folder to the deploy area
4. Get instant URL! ✅

### Method 2: Git Integration

1. Push code to GitHub
2. Connect repository in Netlify
3. Build settings:
   - Build command: (leave empty)
   - Publish directory: `/`
4. Deploy!

### Environment Variables

If using API key, set it in Netlify dashboard:
```
OPENWEATHER_API_KEY=your_actual_api_key
```

Then update code to read from environment:
```javascript
this.API_KEY = process.env.OPENWEATHER_API_KEY || 'YOUR_API_KEY';
```

---

## Vercel

### Quick Deploy

1. Install Vercel CLI:
   ```bash
   npm i -g vercel
   ```

2. Deploy:
   ```bash
   cd weather-runner
   vercel
   ```

3. Follow prompts
4. Get production URL! ✅

### GitHub Integration

1. Import GitHub repository in Vercel dashboard
2. Configure project:
   - Framework Preset: Other
   - Build Command: (leave empty)
   - Output Directory: /
3. Deploy automatically on push

---

## Custom Server

### Nginx Configuration

```nginx
server {
    listen 80;
    server_name weather-runner.example.com;
    root /var/www/weather-runner;
    index index.html;

    # Enable gzip compression
    gzip on;
    gzip_types text/html text/css application/javascript;

    # Cache static assets
    location ~* \.(html)$ {
        expires -1;
        add_header Cache-Control "no-cache, no-store, must-revalidate";
    }

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
    add_header X-XSS-Protection "1; mode=block";

    # HTTPS redirect (if using SSL)
    # return 301 https://$server_name$request_uri;
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName weather-runner.example.com
    DocumentRoot /var/www/weather-runner
    
    <Directory /var/www/weather-runner>
        Options -Indexes +FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    # Enable compression
    <IfModule mod_deflate.c>
        AddOutputFilterByType DEFLATE text/html text/css application/javascript
    </IfModule>

    # Cache control
    <IfModule mod_expires.c>
        ExpiresActive On
        ExpiresByType text/html "access plus 0 seconds"
    </IfModule>
</VirtualHost>
```

### Upload Files

```bash
# Using SCP
scp -r * user@server:/var/www/weather-runner/

# Using FTP
ftp user@server
cd /var/www/weather-runner
mput *
bye

# Using rsync
rsync -avz --progress ./ user@server:/var/www/weather-runner/
```

---

## Performance Optimization

### Before Deployment Checklist

- [ ] Minify HTML/CSS/JS (optional for single file)
- [ ] Compress images (if any added)
- [ ] Enable gzip compression on server
- [ ] Set proper cache headers
- [ ] Use CDN for faster global access
- [ ] Enable HTTPS (SSL certificate)
- [ ] Test in multiple browsers
- [ ] Test on mobile devices
- [ ] Check page load speed

### Minification (Optional)

For better performance, you can minify the HTML:

```bash
# Install html-minifier
npm install -g html-minifier

# Minify
html-minifier --collapse-whitespace --remove-comments --minify-js --minify-css index.html -o index.min.html
```

**Note**: For this project, minification is optional as the file is already small (~32 KB).

### CDN Setup

Use Cloudflare for free CDN:

1. Sign up at [cloudflare.com](https://cloudflare.com)
2. Add your domain
3. Update nameservers with your registrar
4. Enable automatic optimizations:
   - Auto Minify
   - Brotli compression
   - HTTP/2
   - HTTP/3

---

## Monitoring

### Analytics

Add Google Analytics to track usage:

```html
<!-- Add before </head> tag -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### Error Tracking

Add Sentry for error monitoring:

```html
<script
  src="https://browser.sentry-cdn.com/7.0.0/bundle.min.js"
  integrity="sha384-..."
  crossorigin="anonymous"
></script>
<script>
  Sentry.init({ dsn: "YOUR_DSN_HERE" });
</script>
```

### Performance Monitoring

Use Web Vitals:

```javascript
// Add to your JavaScript
import {getLCP, getFID, getCLS} from 'web-vitals';

getLCP(console.log);
getFID(console.log);
getCLS(console.log);
```

---

## Security Best Practices

### HTTPS

Always use HTTPS in production:

```bash
# Let's Encrypt (free SSL)
sudo certbot --nginx -d weather-runner.example.com
```

### Content Security Policy

Add CSP header to prevent XSS:

```nginx
add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' https://api.openweathermap.org;";
```

### API Key Protection

**Never expose API keys in client-side code!**

Option 1: Use environment variables (server-side proxy)
Option 2: Use backend service to proxy API calls
Option 3: Use restricted API keys (IP/domain whitelist)

Example backend proxy (Node.js/Express):

```javascript
const express = require('express');
const axios = require('axios');
const app = express();

app.get('/api/weather', async (req, res) => {
  const { lat, lon } = req.query;
  const API_KEY = process.env.OPENWEATHER_API_KEY;
  
  try {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`
    );
    res.json(response.data);
  } catch (error) {
    res.status(500).json({ error: 'Weather API error' });
  }
});

app.listen(3000);
```

Update frontend to use proxy:

```javascript
const response = await fetch(`/api/weather?lat=${lat}&lon=${lon}`);
```

---

## Testing Before Deployment

### Automated Testing

```bash
# Lighthouse CI
npm install -g @lhci/cli
lhci autorun

# Results will show:
# - Performance score
# - Accessibility score
# - Best practices
# - SEO score
```

### Manual Testing Checklist

- [ ] Game loads in < 2 seconds
- [ ] All weather types work
- [ ] Geolocation prompt appears
- [ ] Fallback works when denied
- [ ] Controls responsive (keyboard + touch)
- [ ] Score saves correctly
- [ ] No console errors
- [ ] Mobile responsive
- [ ] Works in Chrome, Firefox, Safari
- [ ] HTTPS enabled

---

## Post-Deployment

### Monitor Metrics

Track these KPIs:
- Page load time
- Bounce rate
- Session duration
- Error rate
- Daily active users

### Gather Feedback

- Add feedback form
- Monitor social media
- Read user reviews
- Track feature requests

### Regular Updates

- Check for browser compatibility issues
- Update dependencies (if any)
- Fix reported bugs
- Add new features based on feedback

---

## Troubleshooting

### Issue: Game not loading

**Solution:**
- Check browser console for errors
- Verify file uploaded correctly
- Check server logs
- Ensure correct MIME types

### Issue: Weather API not working

**Solution:**
- Verify API key is valid
- Check API quota not exceeded
- Ensure HTTPS used (some APIs require it)
- Check CORS settings

### Issue: Poor performance

**Solution:**
- Enable gzip compression
- Use CDN
- Optimize images
- Minify code
- Enable browser caching

---

## Cost Estimation

### Free Tier Options

| Service | Free Tier | Limitations |
|---------|-----------|-------------|
| GitHub Pages | Unlimited | 1 GB storage, 100 GB/month bandwidth |
| Netlify | 100 GB bandwidth | 300 build minutes/month |
| Vercel | 100 GB bandwidth | Unlimited personal projects |
| Cloudflare | Unlimited | None for basic features |
| Firebase | 1 GB storage, 10 GB/month | Generous free tier |

### Paid Options (if needed)

| Service | Starting Price | Features |
|---------|---------------|----------|
| AWS S3 + CloudFront | ~$1/month | Scalable, reliable |
| DigitalOcean Apps | $5/month | Simple deployment |
| Heroku | $7/month | Easy scaling |

---

## Continuous Deployment

### GitHub Actions Example

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./
```

---

## Support

Need help with deployment?

1. Check this guide thoroughly
2. Search existing issues on GitHub
3. Open a new issue with details
4. Contact maintainer

---

**Happy deploying! 🚀**

Remember: Test thoroughly before going live!