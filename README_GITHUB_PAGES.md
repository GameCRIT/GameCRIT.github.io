# GameCRIT - GitHub Pages Deployment Guide

## 🎮 Current Status

**Hosting:** GitHub Pages (`*.github.io`)  
**Issue:** Cannot access from school networks

---

## ⚠️ IMPORTANT: About the `_headers` File

The `_headers` file in this repo **DOES NOT WORK** on GitHub Pages.

- GitHub Pages doesn't support custom header configuration
- The `_headers` file is for Netlify/Cloudflare Pages only
- **Good news:** GitHub Pages already sets CORS headers automatically
- Firebase REST API calls should work fine

**You can safely ignore or delete `_headers` file** - it has no effect on GitHub Pages.

---

## 🚨 Why Schools Can't Access

### Real Issue (Not Cloudflare):

Since you're on GitHub Pages (not Cloudflare), you **DON'T have** access to:
- ❌ Cloudflare dashboard
- ❌ Bot Fight Mode settings
- ❌ Security level settings
- ❌ Firewall rules

**The real problem is likely:**

**School firewalls block `*.github.io` entirely** because:
- Students host proxies/VPNs on GitHub Pages
- IT departments blanket-ban the whole domain
- Your legitimate site gets caught in the ban

---

## ✅ Solutions That Actually Work

### Solution 1: Custom Domain (BEST - $10-15/year)

**This solves 90% of school blocking issues.**

#### Why it works:
- Schools don't block random domains
- Only blocks known problematic domains
- Your custom domain is unknown = not blocked

#### Setup:

1. **Buy a domain** ($10-15/year):
   - Namecheap.com
   - Google Domains  
   - Cloudflare Registrar (cheapest)
   - Porkbun.com

2. **Add CNAME file to your repo:**
   ```bash
   # Create file named CNAME with your domain
   echo "gamecrit.com" > CNAME
   git add CNAME
   git commit -m "Add custom domain"
   git push
   ```

3. **Configure DNS** (in your domain registrar):
   ```
   Type: CNAME
   Name: www (or @)
   Value: yourusername.github.io
   ```

4. **Enable HTTPS** (in GitHub repo settings):
   - Settings → Pages → Enforce HTTPS

**After this, your site is at `gamecrit.com` instead of `*.github.io`**

**Bonus:** Once you have a custom domain, you can optionally route it through Cloudflare CDN for additional security controls.

---

### Solution 2: Request School Whitelist (FREE)

Contact school IT department:

**Email template:**
```
Subject: Whitelist Request for Educational Research Application

Hi [IT Department],

I'm conducting educational research and need access to:
https://[your-username].github.io

This is a Unity WebGL game for [educational purpose/research].
It's hosted on GitHub Pages and is completely safe.

Could you whitelist this specific subdomain in your firewall?

Attached: Screenshots/demo of the application

Thank you!
```

**Success rate:** 50/50 - depends on IT policy

---

### Solution 3: Portable Browsers for Old PCs (FREE)

**For students with old browsers** (no WebAssembly support):

Direct them to: **`browser-check.html`**

This page recommends:
- **Chrome Portable** - https://portapps.io/app/chrome-portable/
- **Firefox Portable** - https://portableapps.com/apps/internet/firefox_portable

**Benefits:**
- Runs from USB drive or Documents folder
- No installation/admin rights needed
- Has WebAssembly support
- Perfect for locked-down school PCs

---

### Solution 4: Alternative Hosting (If GitHub Pages Blocked)

If GitHub Pages is completely blocked, migrate to:

#### Option A: Netlify (Recommended)
- **Free** tier is generous
- Supports `_headers` file
- Custom domain on free tier
- Better control than GitHub Pages
- Sign up: https://netlify.com

**Deploy to Netlify:**
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod --dir=.
```

#### Option B: Vercel
- **Free** tier
- Excellent performance
- Custom domain included
- Sign up: https://vercel.com

#### Option C: Cloudflare Pages
- **Free** tier
- Full Cloudflare security control
- Then `CLOUDFLARE_FIX.md` applies!
- Sign up: https://pages.cloudflare.com

---

## 🔍 Diagnostic Steps

### Test 1: Is GitHub Pages Blocked?

Ask someone at school to visit:
```
https://[your-username].github.io
```

**Results:**

| What happens | Diagnosis |
|--------------|-----------|
| Page doesn't load at all | GitHub Pages is blocked |
| Page loads but shows error | Browser compatibility issue |
| Stuck loading forever | Firewall blocking Unity files |
| Loads slowly but works | Normal (large files) |

### Test 2: Is It the Browser?

Have them visit:
```
https://[your-username].github.io/browser-check.html
```

- Shows "WASM not supported" → Old browser (use portable solution)
- Shows "All checks passed" → Not a browser issue

### Test 3: Is It the Network?

Have them try with:
- School WiFi → Doesn't work
- Mobile hotspot → Works

**If works on mobile but not school WiFi:**
→ Definitely school firewall blocking GitHub Pages

---

## 📊 What Files Actually Work

| File | Works on GitHub Pages? | Purpose |
|------|----------------------|---------|
| `index.html` | ✅ YES | Main game page with enhanced error handling |
| `browser-check.html` | ✅ YES | Browser compatibility checker |
| `_headers` | ❌ NO | GitHub Pages doesn't support this |
| `CLOUDFLARE_FIX.md` | ❌ N/A | Only applies if using Cloudflare CDN |

---

## 🎯 Recommended Action Plan

### Step 1: Diagnose (Today)
- [ ] Share `browser-check.html` with someone at school
- [ ] Get screenshot of results
- [ ] Test from school network vs mobile hotspot
- [ ] Identify if it's firewall or browser issue

### Step 2: Quick Fix (Today)
- [ ] If old browser → Share portable browser solution
- [ ] If blocked → Request IT whitelist

### Step 3: Permanent Fix (This Week)
- [ ] Buy custom domain ($10-15)
- [ ] Configure DNS
- [ ] Add CNAME file to repo
- [ ] Test from school network

### Step 4: If Still Issues (Next Week)
- [ ] Consider migrating to Netlify/Vercel
- [ ] Or set up Cloudflare CDN with custom domain
- [ ] Then `CLOUDFLARE_FIX.md` applies

---

## 💡 Quick Wins Already Implemented

### ✅ What's Already Fixed:

1. **Browser detection** - `index.html` now detects:
   - Old browsers (IE, old Chrome/Firefox/Safari)
   - Missing WebAssembly support
   - Missing WebGL support
   - Shows clear error messages

2. **Compatibility checker** - `browser-check.html`:
   - Tests all requirements
   - Shows what's missing
   - Recommends portable browsers
   - Provides download links

3. **Better error messages:**
   - Explains WHY game won't work
   - Provides specific solutions
   - No confusing technical jargon
   - Link to browser checker

### ❌ What Doesn't Apply:

1. **`_headers` file** - Doesn't work on GitHub Pages
2. **Cloudflare settings** - You don't have access (yet)
3. **Server-side fixes** - GitHub Pages is fully managed

---

## 🚀 Next Steps (Prioritized)

### Priority 1: DIAGNOSE (FREE - Do First)
Test from school to confirm the actual issue:
1. Share `browser-check.html` link
2. Get results/screenshots
3. Try mobile hotspot vs school WiFi
4. Determine if firewall or browser issue

### Priority 2: CUSTOM DOMAIN ($10-15 - Best Solution)
Solves 90% of firewall issues:
1. Buy domain
2. Add CNAME file
3. Configure DNS
4. Test from school

### Priority 3: PORTABLE BROWSERS (FREE - For Old PCs)
Already documented:
1. Share browser-check.html
2. Users download Chrome/Firefox Portable
3. Run from USB/Documents folder
4. Works even on locked-down PCs

### Priority 4: ALTERNATIVE HOSTING (FREE - If Needed)
If GitHub Pages blocked and custom domain doesn't help:
1. Migrate to Netlify
2. Or use Cloudflare Pages
3. Better control, same free tier

---

## 📞 Support Checklist

When someone reports "it doesn't work":

1. **Get specific symptoms:**
   - [ ] Page doesn't load at all?
   - [ ] Page loads but shows error?
   - [ ] Stuck loading forever?
   - [ ] Works on some networks but not others?

2. **Check browser:**
   - [ ] What browser and version?
   - [ ] Visit browser-check.html
   - [ ] Screenshot the results

3. **Check network:**
   - [ ] Try mobile hotspot
   - [ ] Works on mobile but not school?
   - [ ] Then it's firewall issue

4. **Apply correct fix:**
   - Firewall → Custom domain or whitelist request
   - Browser → Portable browser
   - Slow → Educate about loading time

---

## 📝 Summary

### Your Situation:
- ✅ Hosted on GitHub Pages (`.github.io`)
- ❌ No Cloudflare dashboard (can't configure security)
- ❌ `_headers` file doesn't work
- ⚠️ School firewalls likely blocking `*.github.io`

### Best Solution:
**Get a custom domain** - $10-15/year, solves 90% of issues

### What Already Works:
- ✅ Browser compatibility detection
- ✅ Error handling and messaging
- ✅ Portable browser recommendations

### What Doesn't Apply:
- ❌ `CLOUDFLARE_FIX.md` (unless you add Cloudflare CDN)
- ❌ `_headers` file (GitHub Pages doesn't support it)

### Next Action:
1. Test from school to confirm issue
2. Get custom domain (recommended)
3. Or request IT whitelist
4. Share portable browser solution

---

**Questions? Need help with custom domain setup? Let me know!**






