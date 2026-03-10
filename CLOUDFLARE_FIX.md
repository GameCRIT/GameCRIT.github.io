# Cloudflare Configuration Fix for School Network Access Issues

## Problem
The Unity WebGL application is not accessible from school networks, mobile internet, and some home networks. This is typically caused by Cloudflare's security features being too aggressive.

## Required Cloudflare Dashboard Settings

### 1. Security Settings (CRITICAL)

**Navigate to:** Security → Settings

- **Security Level:** Set to **"Medium"** or **"Low"** (NOT High or I'm Under Attack!)
  - High security blocks legitimate users from school networks
  - I'm Under Attack mode blocks almost everyone

- **Challenge Passage:** Set to **30 minutes** or higher
  - This reduces repeated challenges for the same users

### 2. Bot Fight Mode (CRITICAL - This is likely the main issue!)

**Navigate to:** Security → Bots

- **Bot Fight Mode:** **DISABLE** or set to **"Off"**
  - This is the #1 cause of school network blocking
  - School networks often share IPs and trigger false positives
  - Bot Fight Mode blocks many legitimate users

- **Super Bot Fight Mode:** **DISABLE**
  - Even more aggressive than Bot Fight Mode

### 3. Firewall Rules

**Navigate to:** Security → WAF → Firewall rules

Create a rule to **ALLOW** traffic from known good IP ranges:
- If possible, whitelist school network IP ranges
- Create a rule: "Allow requests from trusted IPs"
- Action: Allow

Or create a rule to **BYPASS CHALLENGES** for:
- User-Agent contains "Unity" or "WebGL"
- Referrer matches your domain
- Action: Skip (Bypass)

### 4. Rate Limiting

**Navigate to:** Security → WAF → Rate limiting rules

- **Disable** any aggressive rate limiting rules
- Or create exceptions for:
  - Unity WebGL files (`/Build/*`)
  - Streaming assets (`/StreamingAssets/*`)
  - Your domain

### 5. Page Rules (Alternative Solution)

**Navigate to:** Rules → Page Rules

Create rules to bypass challenges for Unity assets:
- URL Pattern: `*gamecrit.github.io/Build/*`
  - Settings: Security Level: Off, Disable Apps, Cache Level: Cache Everything

- URL Pattern: `*gamecrit.github.io/StreamingAssets/*`
  - Settings: Security Level: Off, Cache Level: Cache Everything

### 6. Always Use HTTPS

**Navigate to:** SSL/TLS → Overview

- **SSL/TLS encryption mode:** Set to **"Full"** or **"Full (strict)"**
- This ensures proper HTTPS connection

### 7. Caching Settings

**Navigate to:** Caching → Configuration

- Ensure **Caching Level: Standard**
- **Browser Cache TTL:** Respect Existing Headers (our `_headers` file sets this)

## Quick Fix Priority

1. **IMMEDIATE:** Disable Bot Fight Mode (Security → Bots)
2. **IMMEDIATE:** Lower Security Level to Medium (Security → Settings)
3. **IMPORTANT:** Create Page Rules for `/Build/*` and `/StreamingAssets/*` to bypass challenges
4. **RECOMMENDED:** Adjust Rate Limiting rules

## Testing After Changes

1. Wait 5-10 minutes for Cloudflare changes to propagate
2. Clear browser cache
3. Test from a school network
4. Check browser console for any CORS or network errors
5. Test Firebase API connectivity from Unity WebGL

## Additional Notes

- The `_headers` file has been updated with proper CORS headers for Firebase REST API
- All Unity WebGL assets now have proper `Cross-Origin-Resource-Policy: cross-origin` headers
- CORS is enabled for all files (`Access-Control-Allow-Origin: *`)
- This allows Firebase REST API calls to work from the Unity WebGL build

## If Issues Persist

Check Cloudflare Analytics:
- **Navigate to:** Analytics → Web Traffic
- Look for blocked requests or high challenge rates
- Check if specific countries/regions are being blocked

Check Firewall Events:
- **Navigate to:** Security → Events
- Review blocked requests and reasons
- Adjust rules based on legitimate traffic being blocked






