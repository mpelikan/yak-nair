# CLAUDE.md - Project Guide for Claude Code

## Project Overview

Yak Nair is a parody product landing page combining "yak shaving" (programming term for endless prerequisite tasks) with Nair hair removal products. The tagline is "Stop Shaving. Start Shipping."

- **Live site**: https://yaknair.com
- **GitHub**: https://github.com/mpelikan/yak-nair

## Tech Stack

- Plain HTML/CSS (no build tools, no frameworks)
- Single `index.html` file with embedded CSS
- Hosted on AWS S3 + CloudFront

## Deployment

### AWS Configuration
- **AWS Profile**: `jaby` (uses SSO - see login command below)
- **S3 Bucket**: `yaknair.com`
- **CloudFront Distribution (yaknair.com)**: `E1HJMFXZVS8RHH`
- **CloudFront Distribution (yak-nair.com)**: Redirects to yaknair.com

### SSO Login
Before deploying, ensure you're logged in:
```bash
aws sso login --profile jaby
```

### Deploy Commands
```bash
# Sync to S3 (exclude non-web files)
AWS_PROFILE=jaby aws s3 sync . s3://yaknair.com --exclude ".git/*" --exclude ".DS_Store" --exclude ".gitignore" --exclude ".claude/*" --exclude "favicon_io/*" --exclude "README.md" --exclude "CLAUDE.md" --exclude "bottle-original.png"

# Invalidate CloudFront cache
AWS_PROFILE=jaby aws cloudfront create-invalidation --distribution-id E1HJMFXZVS8RHH --paths "/*"
```

### Deployment Preferences
- **Wait for approval** before deploying - do not auto-deploy
- Always set `Cache-Control: public, max-age=31536000` on static assets (images, fonts, etc.)
- Set `content-type: text/html` when uploading index.html

## Code Style Preferences

- Use **tabs** for indentation
- Use **HTML entities** (`&trade;`, `&mdash;`, `&copy;`, etc.) instead of raw characters
- Keep CSS embedded in `index.html` (no separate stylesheet)
- Prefer semantic HTML elements

## Design Preferences

- No JavaScript that affects users with sensory issues (animations, flashing, etc.)
- Console easter egg is OK (doesn't affect visual experience)
- Accessibility matters - aim for WCAG compliance
- Test on Safari (some CSS features like `text-decoration-style: wavy` don't work)

## Color Scheme

| Purpose | Color | Hex |
|---------|-------|-----|
| Primary (dark blue) | Headers, icons | `#1a237e` |
| Accent (pink) | CTAs, highlights, strikethrough | `#e91e63` |
| Green | Badges, checkmarks, terminal tagline | `#4caf50` |
| Warning orange | Safety section | `#e65100` |
| Warning background | Safety section | `#fff3e0` |

## Key Files

- `index.html` - Main page with all HTML and embedded CSS
- `yaknair.png` - Logo (823 x 288)
- `bottle.png` - Product image, optimized (600 x 900)
- `bottle-original.png` - Original product image (excluded from S3 deploy)
- `social-preview.png` - Open Graph image

## Notes

- CloudFront is on the Free pricing plan (no custom response headers policies)
- The site passed PageSpeed Insights with 100 score
- W3C Validator clean
- Dedicated to the memory of Steven Pelikan
