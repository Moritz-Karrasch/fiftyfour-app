# FiftyFour – Website & Privacy Policy

This directory contains the static website for **FiftyFour** (the dice game app), designed for **GitHub Pages** and **Apple App Store** compliance.

## 📁 Contents

- **index.html** – Main landing page with app information, features, and download links
- **privacy.html** – Privacy policy (GDPR-compliant, based on actual app implementation)
- **styles.css** – Unified stylesheet matching app branding (purple/dark theme)
- **TECHNICAL_PRIVACY_REVIEW.md** – Technical analysis documenting privacy policy decisions

## 🚀 GitHub Pages Setup

### Option 1: Deploy from `/docs` folder (Recommended)
1. Go to **Repository Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: **main** → Folder: **/docs**
4. Save

Your site will be live at: `https://<username>.github.io/fiftyfour/`

### Option 2: Custom Domain
If you own `fiftyfour-app.de`:
1. Add a `CNAME` file to `/docs` with content: `fiftyfour-app.de`
2. Configure DNS:
   - Type: `CNAME`
   - Name: `@` or `www`
   - Value: `<username>.github.io`
3. Enable "Enforce HTTPS" in GitHub Pages settings

## 🍎 Apple App Store URLs

When submitting to the App Store, use these URLs:

- **Website URL:** `https://<your-domain>/`
- **Privacy Policy URL:** `https://<your-domain>/privacy.html`
- **Support URL:** `https://<your-domain>/#support` or `mailto:support@fiftyfour-app.de`

## 📱 App Information

- **App Name:** FiftyFour (Display: "54")
- **Bundle ID:** `de.ml.fiftyfour`
- **Platforms:** iOS, Android, Windows, macOS
- **Category:** Games / Dice Game

## 🎨 Design

The website uses the app's branding:
- **Primary Color:** #6C63FF (purple)
- **Secondary Color:** #00D9FF (cyan)
- **Background:** Dark gradient (#0A0E1A → #0F1629)
- **Typography:** System fonts (San Francisco on iOS, Roboto on Android)

## 🔒 Privacy Policy Notes

The privacy policy is **technically accurate** and based on:
- Code review of the entire solution
- Analysis of NuGet packages
- Review of Android/iOS manifests
- Examination of data storage (SQLite local-only)
- Verification of ad integration (Google AdMob)

**Key Privacy Features:**
- ✅ No user accounts or authentication
- ✅ No cloud sync or backend services
- ✅ Local-only SQLite storage
- ✅ No analytics or crash reporting (except AdMob ads)
- ✅ Minimal permissions (INTERNET for ads only)

**Third-Party Services:**
- **Google AdMob** (ads) – Banner, Interstitial, Rewarded Video

See `TECHNICAL_PRIVACY_REVIEW.md` for full technical analysis.

## ⚠️ Before Launch – Action Items

### Required Updates
1. **Contact Information:** Replace `support@fiftyfour-app.de` with real contact email
2. **Legal Entity:** Add full developer name and address in privacy policy (Section 1)
3. **Impressum (Germany):** If commercial, create `imprint.html` with legal details

### Verification Needed
1. **AdMob Consent:** Test that consent dialog appears on first launch
2. **Store Links:** Update download buttons in `index.html` with actual App Store/Play Store URLs
3. **Domain:** Update all URLs if using custom domain instead of GitHub Pages

### Optional Enhancements
1. Add app screenshots to landing page
2. Create `favicon.ico` with dice icon
3. Add OpenGraph meta tags for social sharing
4. Implement cookie consent banner (if using analytics later)

## 🛠️ Maintenance

### Updating Privacy Policy
If you add new features or SDKs:
1. Update `privacy.html` with new data processing details
2. Document changes in `TECHNICAL_PRIVACY_REVIEW.md`
3. Update "Stand: [Date]" timestamp in privacy policy
4. Increment version in technical review

### Updating Website
- All changes should be made in the `/docs` folder
- GitHub Pages auto-deploys on push to main branch
- Test locally: `python -m http.server 8000` in `/docs` folder

## 📄 License

This website is part of the FiftyFour app project.  
© 2025 FiftyFour. All rights reserved.

---

**Need Help?**  
Contact: [support@fiftyfour-app.de](mailto:support@fiftyfour-app.de)
