# Field Type Patterns

Best practices for every common form field type, with implementation guidance and conversion impact data.

---

## Text Input Fields

### Email
**Conversion impact:** Highest-friction single field for lead gen. Every additional field after email reduces conversion 5-15%.

| Pattern | Do | Don't |
|---------|-----|-------|
| Input type | `type="email"` (triggers email keyboard on mobile) | `type="text"` |
| Validation | Inline, on blur (not on keypress) | Validate only on submit |
| Typo detection | Suggest corrections: "Did you mean @gmail.com?" | Silently reject |
| Confirmation | Never require "Confirm email" field | Add a second email field |
| Placeholder | `name@company.com` | `Enter your email address` |
| Autofill | Enable `autocomplete="email"` | Disable autocomplete |

### Phone Number
**Conversion impact:** Adding a required phone field typically reduces form completion by 20-30%.

| Pattern | Do | Don't |
|---------|-----|-------|
| Required? | Make optional whenever possible | Require without explanation |
| Why text | "We'll only call to confirm your appointment" | Leave unexplained |
| Format | Auto-format as they type: (555) 123-4567 | Reject valid formats |
| Input type | `type="tel"` for numeric keyboard | `type="text"` |
| Country code | Auto-detect with flag selector | Require manual entry |
| Autofill | Enable `autocomplete="tel"` | Disable autocomplete |

### Name Fields
**Conversion impact:** "Full name" (1 field) vs "First/Last" (2 fields) — test this. Single field often wins by 5-10%.

| Pattern | Do | Don't |
|---------|-----|-------|
| Field count | Start with single "Name" field | Default to First + Last |
| When to split | Only if you personalize with first name in immediate follow-up | Split "just because" |
| Required? | Only if used in immediate response | Collect for CRM only |
| Autofill | `autocomplete="name"` or `given-name`/`family-name` | Disable autocomplete |

### URL / Website
| Pattern | Do | Don't |
|---------|-----|-------|
| Prefix | Pre-fill "https://" | Leave empty |
| Validation | Accept with or without protocol | Reject "example.com" |
| Required? | Almost never — infer from email domain | Require for lead forms |
| Input type | `type="url"` | `type="text"` |

---

## Selection Fields

### Dropdown / Select
**When to use:** 5+ mutually exclusive options.

| Pattern | Do | Don't |
|---------|-----|-------|
| Default | "Select one..." placeholder (no pre-selection) | Pre-select first option |
| Searchable | If 10+ options, add type-to-filter | Long unsearchable lists |
| "Other" | Include with free-text follow-up | Force users into your categories |
| Order | Alphabetical, or most-common first | Random order |
| Mobile | Use native select (better UX) | Custom dropdown on mobile |

### Radio Buttons
**When to use:** 2-4 mutually exclusive options. Always visible — no hidden options behind a click.

| Pattern | Do | Don't |
|---------|-----|-------|
| Visible | Show all options at once | Hide behind dropdown |
| Default | No pre-selection (forces intentional choice) | Pre-select to bias |
| Layout | Vertical stack for readability | Horizontal if labels are long |
| Labels | Clickable labels (not just the radio dot) | Tiny click targets |

### Checkboxes (Multi-select)
**When to use:** Non-mutually-exclusive options where multiple selections are valid.

| Pattern | Do | Don't |
|---------|-----|-------|
| Instruction | "Select all that apply" | Ambiguous instructions |
| Count | 3-7 options max | 15+ options |
| Required | Require at least 1 if needed | Require all |
| Layout | Vertical, one per line | Cramped horizontal layout |

### Toggle / Switch
**When to use:** Binary on/off choices with immediate effect.

| Pattern | Do | Don't |
|---------|-----|-------|
| Labels | Clear on/off state labels | Ambiguous toggle |
| Default | Sensible default (user-friendly, not business-friendly) | Sneaky opt-ins |
| Feedback | Instant visual feedback on change | No state indication |

---

## Free Text Fields

### Textarea (Message / Comments)
| Pattern | Do | Don't |
|---------|-----|-------|
| Required? | Make optional for most forms | Require "message" on contact forms |
| Size | Start small, expand on focus | Large empty box (intimidating) |
| Guidance | "Tell us briefly what you need help with" | "Enter your message" |
| Character limit | Show remaining characters if limited | Silent truncation |
| Resize | Allow vertical resize | Lock textarea size |

---

## Special Fields

### File Upload
| Pattern | Do | Don't |
|---------|-----|-------|
| Drag & drop | Support drag-and-drop + click to browse | Click-only upload |
| Feedback | Show upload progress + file name | No upload confirmation |
| Limits | State file types and size limits upfront | Error only after upload attempt |
| Multiple | Allow multi-file if needed | Force one-at-a-time |

### Date Picker
| Pattern | Do | Don't |
|---------|-----|-------|
| Input | Allow both typed and calendar picker | Calendar-only (slow on mobile) |
| Format | Show expected format (MM/DD/YYYY) | Ambiguous date format |
| Defaults | Sensible default (today, next business day) | Empty with no guidance |
| Mobile | Use native date picker | Custom calendar on mobile |

### CAPTCHA
**Conversion impact:** Visible CAPTCHAs reduce form completion by 10-20%.

| Pattern | Do | Don't |
|---------|-----|-------|
| Type | Invisible reCAPTCHA or hCaptcha | Image selection puzzles |
| Placement | Only show challenge if score is suspicious | Show to every user |
| Fallback | Honeypot field as first line of defense | CAPTCHA as only defense |
| Mobile | Ensure mobile-friendly challenge | Desktop-only puzzles |

---

## Mobile-Specific Patterns

| Field Type | Mobile Keyboard | Autocomplete Attribute |
|-----------|----------------|----------------------|
| Email | `inputmode="email"` | `autocomplete="email"` |
| Phone | `inputmode="tel"` | `autocomplete="tel"` |
| Number | `inputmode="numeric"` | varies |
| URL | `inputmode="url"` | `autocomplete="url"` |
| ZIP/Postal | `inputmode="numeric"` | `autocomplete="postal-code"` |
| Credit card | `inputmode="numeric"` | `autocomplete="cc-number"` |
| Password | default | `autocomplete="new-password"` or `current-password` |

**Key principle:** Always set `inputmode` and `autocomplete` attributes. The right keyboard reduces typing effort by 30-50% on mobile.
