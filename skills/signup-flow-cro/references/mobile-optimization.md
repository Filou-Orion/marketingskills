# Mobile Signup Optimization

Mobile-specific patterns, constraints, and optimization strategies for signup flows.

---

## Mobile vs Desktop: Key Differences

| Factor | Desktop | Mobile |
|--------|---------|--------|
| Typing speed | ~40-60 WPM | ~20-30 WPM |
| Screen width | 1200px+ | 320-428px |
| Autofill reliability | Good | Excellent (saved credentials) |
| Tap target minimum | 24px | 44px (Apple), 48px (Google) |
| Keyboard switches | N/A | Costly (email → text → number) |
| Social auth | Redirect flow | Native app deep-link (faster) |
| Attention span | Moderate | Short (more interruptions) |

**Key insight:** Mobile users have 30-50% less patience for form-filling. Every unnecessary field or keyboard switch costs more on mobile than desktop.

---

## Mobile Form Layout Rules

### Sizing
```
┌─────────────────────┐
│                     │  ← 16px padding on sides
│  ┌───────────────┐  │
│  │ Email         │  │  ← Full-width fields
│  │ [___________] │  │  ← 48px minimum height
│  │               │  │  ← 12-16px between fields
│  │ Password      │  │
│  │ [___________] │  │
│  │               │  │
│  │ [Create Acct] │  │  ← 48px minimum height, full-width
│  └───────────────┘  │
│                     │
└─────────────────────┘
```

### Rules
- **Always single column** — never side-by-side fields on mobile
- **Full-width inputs** — edge-to-edge minus padding
- **48px minimum touch targets** — for inputs and buttons
- **16px minimum font size** — prevents iOS auto-zoom
- **12-16px spacing** between fields
- **Label above field** — never beside it
- **Sticky CTA button** — if form scrolls below fold

---

## Input Types and Keyboards

Setting the correct `inputmode` and `type` attributes reduces typing effort by 30-50%.

### Keyboard Map

| Field | HTML `type` | `inputmode` | Keyboard Shown |
|-------|-----------|-------------|----------------|
| Email | `email` | `email` | @ symbol visible, no spaces |
| Password | `password` | — | Default keyboard |
| Phone | `tel` | `tel` | Number pad with +, -, () |
| Name | `text` | — | Default with auto-capitalize |
| URL | `url` | `url` | .com, /, : visible |
| Verification code | `text` | `numeric` | Number pad |
| ZIP code | `text` | `numeric` | Number pad |
| Credit card | `text` | `numeric` | Number pad |

### Autocomplete Attributes
```html
<!-- Signup form -->
<input type="email" autocomplete="email" />
<input type="password" autocomplete="new-password" />
<input type="text" autocomplete="name" />
<input type="tel" autocomplete="tel" />
<input type="text" autocomplete="organization" />

<!-- Login form -->
<input type="email" autocomplete="username" />
<input type="password" autocomplete="current-password" />
```

**Why this matters:** Proper autocomplete enables one-tap autofill. Users complete forms in seconds instead of minutes.

---

## Social Auth on Mobile

Social auth is even more valuable on mobile because it eliminates typing entirely.

### Platform-Specific Behavior

| Auth Provider | Mobile UX | Notes |
|--------------|-----------|-------|
| **Google (One Tap)** | Shows account picker overlay, no redirect | Best mobile experience, implement first |
| **Apple Sign In** | Face ID/Touch ID, instant | Required for iOS apps with social auth |
| **Microsoft** | Redirect to Authenticator app or browser | Good for B2B, slower flow |
| **GitHub** | Redirect to browser/app | Dev tools, moderate friction |
| **Facebook** | In-app browser or app switch | Declining trust, consider removing |

### Implementation Tips
- **Google One Tap** should be the default mobile auth — it's the least friction
- Place social auth buttons **above** email form (more prominent on smaller screens)
- Minimum button height: **48px**
- Show the user's Google account name/avatar when possible (reduces uncertainty)

---

## Mobile-Specific Optimizations

### Reduce Keyboard Switches
Every keyboard switch (text → number → email) adds friction on mobile. Minimize by:
- Grouping fields that use the same keyboard type
- Avoiding unnecessary fields that require different keyboards
- Using dropdowns/selects instead of free text where possible

### Smart Defaults
- Auto-detect country from locale for phone number formatting
- Pre-select country code based on device locale
- Auto-capitalize first letter for name fields
- Suggest email domains after @ (gmail.com, outlook.com, etc.)

### Handle Interruptions
Mobile users get interrupted (notifications, calls, app switches). Protect against:
- **Save state on every field blur** (not just on submit)
- **Restore state on return** — don't lose filled data
- **Session persistence** — if user closes and reopens, resume where they left off
- **Deep link back** — if redirected to auth provider, handle return gracefully

---

## Testing on Mobile

### Devices to Test
Minimum device matrix:

| Category | Devices |
|----------|---------|
| **iOS (recent)** | iPhone 15/16, current Safari |
| **iOS (older)** | iPhone SE (small screen test) |
| **Android (flagship)** | Samsung Galaxy S24, Pixel 8 |
| **Android (budget)** | Samsung Galaxy A-series (slower processor) |

### What to Check
- [ ] All fields show correct keyboard type
- [ ] Autofill works for email, password, name
- [ ] Social auth completes without broken redirects
- [ ] Form doesn't shift/jump when keyboard appears
- [ ] CTA button visible when keyboard is open (or accessible by scrolling)
- [ ] Error messages visible when keyboard is open
- [ ] Touch targets are at least 44px
- [ ] Text is at least 16px (no auto-zoom on iOS)
- [ ] Form state persists across app switches
- [ ] Orientation change doesn't lose data
