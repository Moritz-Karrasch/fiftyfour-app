# Technical Privacy Review – FiftyFour App

**Review Date:** January 2025  
**Reviewer:** Technical Analysis based on Source Code Inspection  
**Purpose:** Establish a technical foundation for the privacy policy based on actual implementation

---

## 1. Executive Summary

FiftyFour is a local-first dice game app with **minimal data processing**. The app operates **without backend services, cloud sync, user accounts, or analytics tracking**. The only external data processing occurs through **Google AdMob** for ad monetization.

**Key Findings:**
- ✅ No proprietary backend or cloud services
- ✅ No user authentication or account system
- ✅ No analytics/telemetry services (Firebase, Sentry, etc.)
- ✅ No crash reporting services
- ✅ No social media integrations
- ✅ All game data stored locally in SQLite
- ⚠️ Google AdMob integration (personalized ads)

---

## 2. Code Analysis – Sources Reviewed

### 2.1 Project Files
- **FiftyFour.MobileApp.csproj** (lines 1–150)
  - Target frameworks: iOS, Android, Windows, macOS
  - Application ID: `de.ml.fiftyfour`
  - Display name: "54"
  - NuGet packages reviewed

- **MauiProgram.cs** (lines 1–200)
  - Dependency injection setup
  - Service registrations
  - Ad service configuration

- **AndroidManifest.xml**
  - Permissions: `INTERNET`, `ACCESS_NETWORK_STATE` (only for ads)
  - AdMob App ID: `ca-app-pub-7925087649227451~8369541814`

- **Info.plist** (iOS)
  - AdMob App ID: `ca-app-pub-7925087649227451~4548014047`
  - No tracking permissions requested
  - Encryption compliance: No custom encryption

### 2.2 NuGet Packages Analyzed
From **FiftyFour.MobileApp.csproj**:
- `CommunityToolkit.Maui` – UI components only
- `CommunityToolkit.Mvvm` – MVVM framework (local)
- `SkiaSharp.Views.Maui.Controls` – Rendering library (local)
- `Plugin.AdMob` (v3.0.2) – **Google AdMob integration** ⚠️
- `Syncfusion.Maui.Toolkit` – UI toolkit (local)
- `sqlite-net-pcl` – Local SQLite database
- `SQLitePCLRaw.bundle_green` – SQLite runtime

**No analytics, crash reporting, or cloud sync packages found.**

### 2.3 Backend/Infrastructure Projects
- **FiftyFour.Core.csproj**
  - Pure .NET 10 library
  - No external dependencies except `CommunityToolkit.Mvvm`

- **FiftyFour.Infrastructure.csproj**
  - Only `sqlite-net-pcl` and `SQLitePCLRaw.bundle_green`
  - No cloud SDKs (no Supabase, Firebase, Azure, AWS)

- **AppDatabase.cs**
  - Tables: `PlayerEntity`, `SettingsEntity`, `PlayerStatisticsEntity`, `DiceColorUnlockEntity`
  - Database path: `FileSystem.AppDataDirectory/FiftyFour.db3` (local only)

---

## 3. Data Processing Analysis

### 3.1 Local Data Storage (No Privacy Concerns)

**Location:** SQLite database stored in app-specific directory  
**Tables:**
1. **PlayerEntity**
   - Player names (user-chosen, not personally identifying)
   - Dice colors (cosmetic preference)
   - **No real names, emails, or personal data required**

2. **SettingsEntity**
   - Target score, falling dice setting, theme, language
   - **Purely functional app settings**

3. **PlayerStatisticsEntity**
   - Games played, won, lost, sixes rolled, points
   - **Anonymous gameplay statistics**

4. **DiceColorUnlockEntity**
   - Unlock timestamps for premium dice colors
   - **No personal identifiers, only unlock states**

**Technical Assessment:**
- ✅ All data stored locally via `FileSystem.AppDataDirectory`
- ✅ No network requests to own servers
- ✅ No cloud sync services (Supabase, Firebase, etc.)
- ✅ Data deleted on app uninstall

---

### 3.2 Google AdMob Integration (Main Privacy Relevance)

**Source Files:**
- `Services/Ads/AdConfig.cs` (lines 1–100)
- `Services/Ads/BannerAdService.cs`
- `Services/Ads/InterstitialAdService.cs` (lines 1–100+)
- `Services/Ads/RewardedAdService.cs`

**Ad Unit IDs (Production):**
- **Android Banner Primary:** `ca-app-pub-7925087649227451/5974656687`
- **Android Banner Fallback:** `ca-app-pub-7925087649227451/8433073725`
- **Android Interstitial:** `ca-app-pub-7925087649227451/6928420360`
- **iOS Banner Primary:** `ca-app-pub-7925087649227451/9809959912`
- **iOS Banner Fallback:** `ca-app-pub-7925087649227451/3234932372`

**Ad Formats:**
1. **Banner Ads**
   - Refresh interval: 45 seconds
   - Locations: Home, Setup, Settings, Statistics, Game, DetermineStart, Tiebreaker
   - Fallback waterfall: Primary (with eCPM floor) → Fallback (no floor)

2. **Interstitial Ads**
   - Cooldown: 120 seconds between displays
   - Session limit: 4 interstitials max
   - Minimum session age: 60 seconds
   - Grace period: 1 day after install

3. **Rewarded Video Ads**
   - Optional (user-initiated)
   - Reward: 7-day dice color unlock
   - No forced viewing

**Data Processed by AdMob:**
- Device type, OS version, screen resolution
- Advertising ID (IDFA/AAID)
- IP address (for geolocation)
- Ad interaction data (impressions, clicks)
- App ID: `de.ml.fiftyfour`

**Technical Notes:**
- AdMob consent handling: Assumed via `Plugin.AdMob` SDK defaults
- No explicit consent management code found (TODO: verify consent dialog implementation)
- AdMob privacy settings: `DELAY_APP_MEASUREMENT_INIT`, `OPTIMIZE_INITIALIZATION`, `OPTIMIZE_AD_LOADING`

---

### 3.3 Permissions Analysis

**Android (AndroidManifest.xml):**
- `INTERNET` – Required for ad loading
- `ACCESS_NETWORK_STATE` – Check network availability for ads

**iOS (Info.plist):**
- No explicit permissions requested
- `ITSAppUsesNonExemptEncryption: false` – No custom encryption

**Assessment:**
- ✅ Minimal permissions (only for ads)
- ✅ No location, camera, contacts, microphone, etc.
- ✅ No background location tracking

---

### 3.4 Analytics & Tracking

**Absence of Common SDKs:**
- ❌ No Firebase Analytics
- ❌ No Google Analytics
- ❌ No Sentry (crash reporting)
- ❌ No AppCenter/HockeyApp
- ❌ No Mixpanel, Amplitude, Segment
- ❌ No Facebook SDK
- ❌ No custom analytics endpoints

**MauiProgram.cs Analysis:**
- Logging: `Microsoft.Extensions.Logging.Debug` (DEBUG builds only)
- No telemetry services registered
- No HTTP clients for external APIs

**Conclusion:** App does not perform any proprietary analytics or tracking.

---

### 3.5 Authentication & Cloud Sync

**No Account System:**
- No login/registration screens
- No authentication services (Firebase Auth, Supabase Auth, OAuth)
- No user profile management

**No Cloud Sync:**
- No backend API endpoints
- No `HttpClient` registrations for sync
- No cloud database (Supabase, Firebase, Azure, AWS)

**Conclusion:** App is fully offline/local.

---

## 4. Privacy Policy Statements – Technical Justification

| Privacy Policy Statement | Technical Evidence | Confidence |
|--------------------------|-------------------|------------|
| "No cloud sync or backend" | No HTTP services, no cloud SDKs | ✅ High |
| "Local SQLite storage" | `AppDatabase.cs`, local `FileSystem.AppDataDirectory` | ✅ High |
| "No user accounts" | No auth services, no login screens | ✅ High |
| "No analytics/tracking (except AdMob)" | No analytics packages, no telemetry | ✅ High |
| "Google AdMob integration" | `Plugin.AdMob`, ad service classes, manifest metadata | ✅ High |
| "Permissions: INTERNET, ACCESS_NETWORK_STATE" | AndroidManifest.xml | ✅ High |
| "AdMob data: Device info, Advertising ID, IP" | Standard AdMob SDK behavior (not overridden) | ⚠️ Medium (based on AdMob docs) |
| "No crash reporting" | No Sentry, AppCenter, Firebase Crashlytics | ✅ High |

---

## 5. Open Questions & Manual Review Needed

### 5.1 AdMob Consent Management
**Status:** ⚠️ **Requires Verification**

**Question:** Does the app implement an explicit consent dialog for personalized ads (GDPR/ePrivacy compliance)?

**Code Review:**
- `Plugin.AdMob` v3.0.2 used
- No explicit consent dialog code found in services
- AdMob manifest flags: `DELAY_APP_MEASUREMENT_INIT` (suggests consent waiting)

**Recommendation:**
- ✅ **Verify:** Check if `Plugin.AdMob` v3.0.2 automatically shows Google's consent dialog (UMP SDK)
- ✅ **Test:** Launch app on fresh install and confirm consent prompt appears
- ✅ **Legal Review:** Ensure consent wording complies with GDPR Art. 6(1)(a) and ePrivacy Directive

---

### 5.2 Contact Information & Legal Entity
**Status:** ⚠️ **Needs Clarification**

**Current Privacy Policy:**
- Developer: "FiftyFour App"
- Email: `support@fiftyfour-app.de`

**Missing Information:**
- Legal entity name (individual developer or company?)
- Full address (required for German Impressumspflicht if commercial)
- Data Protection Officer (if applicable)

**Recommendation:**
- ✅ **Provide:** Full legal name and address of developer/company
- ✅ **Clarify:** Is this a commercial app or hobby project?
- ✅ **Add:** Impressum/Imprint page if required by German law

---

### 5.3 Third-Party Data Processing Agreements
**Status:** ⚠️ **Legal Compliance Check Needed**

**Question:** Are Data Processing Agreements (DPAs) required with Google for AdMob?

**Technical Finding:**
- Google AdMob is a third-party processor
- AdMob processes personal data (Advertising ID, IP address)

**Recommendation:**
- ✅ **Legal Review:** Confirm Google AdMob's GDPR compliance mechanisms (Standard Contractual Clauses, Data Privacy Framework)
- ✅ **Document:** Reference AdMob's Terms of Service and Data Processing Terms in privacy policy
- ✅ **Update:** Privacy policy already references AdMob partner list and Google's privacy policy (✅ done)

---

### 5.4 Data Deletion Mechanism
**Status:** ✅ **Technically Implemented**

**Current Implementation:**
- Local data: Deleted on app uninstall (automatic via OS)
- Individual player deletion: Via in-app player management

**AdMob Data:**
- User can reset Advertising ID in device settings
- User can opt-out of personalized ads in device settings

**Privacy Policy Status:** ✅ Correctly documented

---

### 5.5 Children's Privacy (COPPA/GDPR Age Gate)
**Status:** ⚠️ **Needs Legal Assessment**

**Technical Finding:**
- App is a casual dice game (all-ages content)
- No explicit age gate or parental consent mechanism
- AdMob ads shown to all users

**Questions:**
- Does the app target children under 13 (COPPA) or under 16 (GDPR)?
- Should AdMob be configured for "Child-Directed Treatment"?

**Recommendation:**
- ✅ **Legal Review:** Determine target audience and age rating
- ✅ **AdMob Config:** If targeting children, enable AdMob's child-directed settings
- ✅ **Privacy Policy:** Add explicit children's privacy section if applicable

---

## 6. Recommended Next Steps

### 6.1 Immediate (Pre-Launch)
1. ✅ **Verify AdMob Consent Dialog:** Test app on fresh install, confirm UMP consent prompt
2. ⚠️ **Add Legal Entity Info:** Replace "FiftyFour App" with full developer name and address
3. ⚠️ **Create Impressum Page:** If legally required (German commercial app)
4. ✅ **Test Advertising ID Opt-Out:** Verify ads respect user's personalized ad settings

### 6.2 Legal Review (Recommended)
1. Have a qualified attorney review the privacy policy for GDPR/ePrivacy compliance
2. Clarify COPPA/GDPR-K applicability based on target audience
3. Confirm Google AdMob's Standard Contractual Clauses are sufficient
4. Review Apple's App Store privacy "nutrition labels" for accuracy

### 6.3 Technical Enhancements (Optional)
1. Add in-app link to privacy policy (currently only on website)
2. Implement explicit "Delete All Data" button in settings
3. Add privacy policy version tracking and update notifications

---

## 7. Confidence Assessment

| Category | Confidence Level | Reason |
|----------|-----------------|---------|
| Local-Only Storage | ✅ **High** | SQLite code reviewed, no cloud SDKs |
| No Proprietary Analytics | ✅ **High** | No analytics packages or HTTP clients |
| AdMob Integration | ✅ **High** | Code and manifest metadata verified |
| AdMob Data Processing Details | ⚠️ **Medium** | Based on AdMob SDK docs (standard behavior) |
| Consent Management | ⚠️ **Medium** | Plugin.AdMob assumed to handle UMP |
| Legal Completeness | ⚠️ **Low** | Missing legal entity details |

---

## 8. Final Recommendations

**For Developer:**
1. **Update Contact Info:** Add full legal name, address, and (if required) VAT ID
2. **Test Consent:** Launch app fresh and document consent dialog flow
3. **Add Impressum:** If commercial/German jurisdiction requires it

**For Legal Review:**
1. Validate GDPR Art. 6(1)(a) compliance for AdMob consent
2. Confirm ePrivacy Directive cookie/tracking consent handling
3. Assess COPPA applicability (if targeting US audience)
4. Review Apple/Google store compliance (privacy labels, age ratings)

**For App Store Submission:**
1. ✅ Privacy policy URL: `https://fiftyfour-app.de/privacy.html` (or GitHub Pages URL)
2. ✅ Support URL: `https://fiftyfour-app.de/` or `mailto:support@fiftyfour-app.de`
3. ✅ Privacy "nutrition labels": Data linked to user = Advertising data only

---

## 9. Conclusion

The FiftyFour app demonstrates **good privacy-by-design practices** with local-first architecture and minimal external dependencies. The privacy policy accurately reflects the technical implementation. The primary data processing concern is **Google AdMob**, which is industry-standard and well-documented.

**Action Items:**
- ⚠️ **High Priority:** Add full legal entity information
- ⚠️ **High Priority:** Verify AdMob consent dialog implementation
- ✅ **Medium Priority:** Legal review for GDPR compliance
- ✅ **Low Priority:** Optional technical enhancements (in-app privacy settings)

**Overall Privacy Rating:** ⭐⭐⭐⭐☆ (4/5)  
*Deduction for missing legal entity details and consent verification pending.*

---

**Document Version:** 1.0  
**Last Updated:** January 2025  
**Next Review:** Before App Store submission or upon significant code changes
