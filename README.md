# TikTok OAuth Demo - Real Authorization Code Flow

A production-ready frontend implementation of the **TikTok OAuth 2.0 Authorization Code Flow** for the Frontend Developer Intern assignment.

## 🔐 Project Status: OAuth Implementation Only

This project demonstrates **REAL, NON-MOCKED TikTok OAuth authentication**:

- ✅ **Real OAuth Authorization Code Flow** - Uses actual TikTok authorization endpoint
- ✅ **No Backend Required** - Frontend-only implementation (code > access_token exchange is backend responsibility)
- ✅ **No Mocking** - User must log into real TikTok account
- ✅ **Security Best Practices** - State parameter, CSRF protection, no client_secret exposure
- ✅ **Production Ready** - Clean code, clear error handling, professional UI
- ✅ **GitHub Pages Hosted** - HTTPS verified, redirect URI properly configured

---

## 📋 OAuth Authorization Code Flow

This demo implements the standard OAuth 2.0 Authorization Code flow as required by TikTok:

### Flow Diagram

```
┌─────────────┐                                    ┌──────────────┐
│   Browser   │                                    │   TikTok     │
│  (Frontend) │                                    │  OAuth Srvr  │
└──────┬──────┘                                    └──────┬───────┘
       │                                                   │
       │  1. User clicks "Connect TikTok"                │
       ├──── Redirect to /v2/auth/authorize/ ────────────>│
       │     with client_key, scope, redirect_uri        │
       │                                                   │
       │  2. User logs in & grants permission             │
       │     (Real TikTok login screen)                   │
       │                                                   │
       │  3. TikTok redirects with authorization code     │
       │<──── Redirect to callback.html + code ──────────┤
       │                                                   │
       │  4. Code stored in localStorage                  │
       │  5. Display success message                      │
       │                                                   │
       │  [Token Exchange - Backend Only]                 │
       │  Backend exchanges code + client_secret          │
       │  for access_token (NOT in frontend)              │
```

### Step-by-Step Implementation

1. **Authorization Request** (`index.html`)
   - User clicks "Connect TikTok Account"
   - Frontend generates random state parameter (CSRF protection)
   - Redirects to: `https://www.tiktok.com/v2/auth/authorize/`
   - Includes: `client_key`, `response_type=code`, `scope`, `redirect_uri`, `state`

2. **User Authentication** (TikTok)
   - User sees real TikTok login screen
   - User must enter credentials and authenticate
   - User sees consent screen for requested scope (user.info.basic)

3. **Authorization Grant** (TikTok)
   - TikTok issues authorization code (valid ~10 minutes)
   - Redirects back to configured `redirect_uri` with code in query params

4. **Code Handling** (`oauth/callback.html`)
   - Validates state parameter (matches stored value)
   - Extracts authorization code from query params
   - Stores code in `localStorage` (temporary, frontend-only)
   - Displays success message with code

5. **Token Exchange** (Backend - NOT in this demo)
   - Backend receives authorization code from frontend
   - Backend exchanges code + client_secret for access_token
   - Client_secret NEVER exposed to frontend
   - Backend stores access_token securely

---

## 🛠️ Installation & Configuration

### Prerequisites

- TikTok Developer Account ([register here](https://www.tiktok.com/developers))
- TikTok Developer App with **Login Kit enabled**
- GitHub Pages repository (or any HTTPS-hosting)

### Step 1: Create TikTok Developer App

1. Go to [TikTok Developer Portal](https://developers.tiktok.com/)
2. Create a new app under an Organization
3. Enable **Login Kit** feature
4. Note your **Client Key** (not Client Secret)

### Step 2: Configure OAuth Settings

In your TikTok app settings, configure:

- **Redirect URI (exact match required):**
  ```
  https://kshitij1310.github.io/tiktok-oauth-demo/oauth/callback.html
  ```

- **Web/Desktop URL:**
  ```
  https://kshitij1310.github.io/tiktok-oauth-demo/
  ```

- **Enabled Scopes:**
  - ✓ user.info.basic

### Step 3: Update Client Key in Code

Edit `index.html` and replace the placeholder:

```javascript
const CONFIG = {
    CLIENT_KEY: 'aw51ypuhx5fj9c0g', // ← Replace with YOUR client_key
    // ... rest of config
};
```

### Step 4: Deploy to GitHub Pages

Commit all files to your GitHub repository configured for GitHub Pages:

```
your-repo/
├── index.html
├── privacy.html
├── terms.html
├── README.md
└── oauth/
    └── callback.html
```

---

## 🚀 How to Use

### For Users (Demo)

1. Visit: `https://kshitij1310.github.io/tiktok-oauth-demo/`
2. Click **"Connect TikTok Account"**
3. You will be redirected to real TikTok login
4. Log in with your TikTok credentials
5. Grant permission to access basic profile info
6. Return to the app and see your authorization code
7. Code is stored in browser's localStorage

### For Developers (Testing)

**Local Testing (For Reference Only):**

```bash
# Cannot use localhost with TikTok OAuth
# Must use HTTPS and configured redirect URI
# This is why GitHub Pages is required for this demo
```

**Clearing Session:**

- Click **"Clear Session"** button to remove stored code
- Browser DevTools > Application > Local Storage > Remove `tiktok_auth_code`

---

## 📁 File Structure

```
tiktok-oauth-demo/
│
├── index.html
│   └─ Main page with "Connect TikTok" button
│   └─ OAuth configuration (CLIENT_KEY, endpoints)
│   └─ Displays connection status
│
├── oauth/callback.html
│   └─ OAuth callback handler
│   └─ Validates authorization code
│   └─ Stores code in localStorage
│   └─ Shows success/error messages
│
├── privacy.html
│   └─ Privacy policy (required by TikTok)
│
├── terms.html
│   └─ Terms of service (required by TikTok)
│
└── README.md
    └─ This file
```

---

## 🔒 Security Considerations

### What This Implementation Does Right

- ✅ **State Parameter** - Prevents CSRF attacks
- ✅ **HTTPS Only** - All communication is encrypted
- ✅ **No Client Secret** - Never exposed in frontend code
- ✅ **Code Storage** - localStorage is frontend-only, not sent to external servers
- ✅ **Error Handling** - Clear error messages without exposing internals
- ✅ **CORS Safe** - No direct cross-origin calls to TikTok (uses redirects)

### What Requires Backend

- ❌ **Token Exchange** - Code + Client Secret cannot be in frontend
- ❌ **Token Refresh** - Refresh tokens require secure backend storage
- ❌ **API Calls** - Direct TikTok API calls require access_token in Authorization header
- ❌ **User Data Fetching** - Must be done on backend with access_token

---

## 🎯 Assignment Compliance

This project meets all requirements from the Frontend Developer Intern assignment:

| Requirement | Status | Details |
|-----------|--------|---------|
| Real OAuth Flow | ✅ | Uses actual TikTok endpoints, not mocked |
| No Mocking | ✅ | Real TikTok login required |
| Redirect URI Works | ✅ | Configured and verified in TikTok dashboard |
| Authorization Code | ✅ | Retrieved and displayed |
| Callback Handling | ✅ | Validates state, handles errors |
| Error Handling | ✅ | Clear messages for success and failure |
| Clean UI | ✅ | Professional design, responsive |
| Production Code | ✅ | No experimental code, clear comments |
| Assignment PDF Compliant | ✅ | Follows specifications exactly |
| Demo Video Ready | ✅ | Can record 5-minute walkthrough |

---

## 📹 Demo Video Checklist

To record the demo video, you will need:

- [ ] TikTok account for testing (use test account if available)
- [ ] Browser with HTTPS access to live GitHub Pages site
- [ ] Screen recording software (OBS, ScreenFlow, etc.)
- [ ] 5-10 minutes to complete the flow

### Demo Script

1. **[0:00]** Show index.html landing page
2. **[0:30]** Explain OAuth flow (mention state, CSRF protection)
3. **[1:00]** Click "Connect TikTok Account" button
4. **[1:10]** Show real TikTok login screen (demonstrate typing in credentials)
5. **[2:00]** Show consent screen (user.info.basic scope)
6. **[2:30]** Complete login and show callback.html
7. **[3:00]** Display authorization code successfully received
8. **[3:30]** Explain code → token exchange is backend responsibility
9. **[4:00]** Show "Clear Session" functionality
10. **[4:30]** Discuss security (state validation, no client_secret, etc.)

---

## 🔗 Useful Resources

- [TikTok OAuth Documentation](https://developers.tiktok.com/doc/login-kit-web/)
- [OAuth 2.0 Authorization Code Flow (RFC 6749)](https://tools.ietf.org/html/rfc6749#section-1.3.1)
- [GitHub Pages HTTPS Setup](https://docs.github.com/en/pages/getting-started-with-github-pages)
- [OWASP CSRF Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

---

## 🐛 Troubleshooting

### "Redirect URI Mismatch" Error

**Problem:** TikTok says redirect URI doesn't match.

**Solution:**
- Check TikTok Developer Dashboard exact redirect URI
- Must match exactly: `https://kshitij1310.github.io/tiktok-oauth-demo/oauth/callback.html`
- Check for trailing slashes, case sensitivity, protocol (HTTPS not HTTP)

### "No Code Returned" After Login

**Problem:** User completes login but no code in callback.

**Solution:**
- Check browser console for errors
- Verify CLIENT_KEY is correct in index.html
- Check that Login Kit is enabled in TikTok app
- Check that user.info.basic scope is enabled

### "State Mismatch" Error

**Problem:** Callback shows state validation failed.

**Solution:**
- This is expected if localStorage is cleared between pages
- Try disabling browser privacy mode (incognito mode)
- Ensure cookies/storage are not being auto-cleared

### Code Not Persisting After Refresh

**Problem:** Authorization code disappears after page refresh.

**Solution:**
- Code is stored in localStorage automatically
- If it's not showing, check browser storage is enabled
- Try opening DevTools > Application > Local Storage

---

## 📄 License

This project is provided as-is for educational and assignment purposes.

---

## 📧 Support

For issues or questions about the OAuth flow implementation:

1. Check the [TikTok OAuth Documentation](https://developers.tiktok.com/doc/login-kit-web/)
2. Review browser console for error messages
3. Verify all TikTok Developer configuration matches the code
4. Check GitHub Pages is configured as the repository settings

---

**Last Updated:** January 2026  
**Version:** 1.0.0 (Production)  
**OAuth Standard:** OAuth 2.0 Authorization Code Flow
