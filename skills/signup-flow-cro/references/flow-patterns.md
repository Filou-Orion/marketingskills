# Signup Flow Patterns

Proven signup flow architectures by product type, with wireframe descriptions and conversion benchmarks.

---

## Flow Architecture Comparison

| Pattern | Fields | Steps | Best For | Typical Completion Rate |
|---------|--------|-------|----------|------------------------|
| Single-step minimal | 2-3 | 1 | B2C, simple products | 60-80% |
| Single-step with social auth | 1-3 + SSO | 1 | B2C, developer tools | 65-85% |
| Multi-step progressive | 4-8 | 2-4 | B2B SaaS, complex products | 50-70% |
| Invite-only / waitlist | 1-2 | 1 | Pre-launch, exclusive products | 70-90% |
| Product-first (reverse) | 0 initially | 1 (deferred) | PLG, freemium | 30-50% (but higher activation) |
| Guided onboarding signup | 4-6 | 3-5 | Products needing context | 40-60% |

---

## Pattern 1: Single-Step Minimal

**Architecture:**
```
┌─────────────────────────────┐
│  [Logo]                     │
│                             │
│  Create your account        │
│                             │
│  Email     [____________]   │
│  Password  [____________]   │
│                             │
│  [Create Account]           │
│                             │
│  Already have an account?   │
│  Log in                     │
│                             │
│  "No credit card required"  │
└─────────────────────────────┘
```

**When to use:**
- Consumer products with simple onboarding
- High-intent traffic (from paid ads, referrals)
- Products that can personalize after signup

**Optimization checklist:**
- [ ] Social auth options above email form
- [ ] Show/hide password toggle
- [ ] Inline email validation
- [ ] "No credit card required" if applicable
- [ ] Testimonial or social proof nearby
- [ ] Clear value proposition above form

---

## Pattern 2: Social Auth Primary

**Architecture:**
```
┌─────────────────────────────┐
│  Get started with [Product] │
│                             │
│  [🔵 Continue with Google]  │
│  [⚫ Continue with GitHub]  │
│  [🔵 Continue with Microsoft│
│                             │
│  ─── or ───                 │
│                             │
│  Email     [____________]   │
│  Password  [____________]   │
│                             │
│  [Create Account]           │
└─────────────────────────────┘
```

**When to use:**
- Developer tools (GitHub auth)
- B2B products (Google/Microsoft auth)
- Consumer apps (Google/Apple auth)
- When you want to minimize typing

**Key decisions:**
- Which SSO providers? Match your audience:
  - B2B SaaS: Google, Microsoft, SSO/SAML
  - Developer tools: GitHub, Google, GitLab
  - Consumer: Google, Apple, Facebook
- Order: Most popular option first
- Limit to 2-3 options (paradox of choice)

**Conversion lift:** Social auth typically increases signup completion by 20-40% vs. email-only.

---

## Pattern 3: Multi-Step Progressive

**Architecture:**
```
Step 1 of 3 [●○○]          Step 2 of 3 [●●○]          Step 3 of 3 [●●●]
┌──────────────────┐       ┌──────────────────┐       ┌──────────────────┐
│ What's your      │       │ Tell us about    │       │ Almost there     │
│ email?           │       │ your company     │       │                  │
│                  │       │                  │       │ Password         │
│ [_____________]  │       │ Company          │       │ [_____________]  │
│                  │       │ [_____________]  │       │                  │
│ [Continue →]     │       │ Team size        │       │ Role             │
│                  │       │ [▼ Select___]    │       │ [▼ Select___]    │
│                  │       │                  │       │                  │
│                  │       │ [← Back] [Next →]│       │ [← Back] [Start]│
└──────────────────┘       └──────────────────┘       └──────────────────┘
```

**When to use:**
- 4+ fields required before product access
- B2B products needing segmentation data
- Complex products requiring workspace setup

**Critical rules:**
- Step 1: Email only (lowest possible barrier)
- Easy fields first, sensitive fields last
- Back navigation always available
- Progress saved on each step (no data loss on refresh)
- Progress indicator visible
- Each step completable in <15 seconds

**Conversion tip:** Collect email on step 1 so you can follow up with abandoners who don't complete step 2-3.

---

## Pattern 4: Waitlist / Early Access

**Architecture:**
```
┌──────────────────────────────┐
│  [Product] is launching soon │
│                              │
│  Be the first to know.      │
│                              │
│  Email [__________] [Join →] │
│                              │
│  🎉 2,847 people waiting    │
└──────────────────────────────┘
```

**When to use:**
- Pre-launch products
- Exclusive/invite-only positioning
- Building an audience before launch

**Enhancements:**
- Counter showing waitlist size (social proof)
- "Invite friends to move up" referral mechanic
- Optional "What would you use [product] for?" field
- Post-join: shareable referral link

---

## Pattern 5: Product-First (Reverse Trial)

**Architecture:**
```
Phase 1: Use product without account
┌──────────────────────────────┐
│  [Product interface]         │
│                              │
│  User creates, explores,     │
│  experiences value            │
│                              │
│  ┌──────────────────┐        │
│  │ Save your work?  │        │
│  │ Create a free     │        │
│  │ account to keep   │        │
│  │ your [items].     │        │
│  │                   │        │
│  │ [Create Account]  │        │
│  │ [Continue without]│        │
│  └──────────────────┘        │
└──────────────────────────────┘
```

**When to use:**
- Products where value is immediately demonstrable
- Tools, editors, builders, design apps
- Product-led growth strategy
- When traditional signup gates hurt activation

**Trigger signup when:**
- User tries to save/export
- User hits a usage limit
- User tries a premium feature
- Session length exceeds threshold
- User returns for a second session

**Trade-off:** Lower signup rate (30-50%) but significantly higher activation and retention, because users who sign up have already experienced value.

---

## Pattern 6: Guided Onboarding Signup

**Architecture:**
```
Step 1: Context       Step 2: Account      Step 3: Setup
┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
│ What brings you │   │ Create your     │   │ Let's set up    │
│ here?           │   │ account         │   │ your workspace  │
│                 │   │                 │   │                 │
│ ○ Build website │   │ [Google auth]   │   │ Team name       │
│ ○ Write content │   │ [Email/pass]    │   │ [____________]  │
│ ○ Manage team   │   │                 │   │                 │
│ ○ Something else│   │                 │   │ Invite team     │
│                 │   │                 │   │ [____________]  │
│ [Continue →]    │   │ [Continue →]    │   │ [Skip] [Done →] │
└─────────────────┘   └─────────────────┘   └─────────────────┘
```

**When to use:**
- Products that need context to personalize the experience
- Products with multiple use cases or templates
- When first-run experience depends on user intent

**Key insight:** The "what brings you here" step before account creation increases completion by 10-20% because it creates psychological commitment.

---

## Benchmarks by Industry

| Industry | Avg Signup Completion | Top Quartile | Typical Flow |
|----------|----------------------|-------------|--------------|
| B2C SaaS | 60-70% | 80%+ | Single-step + social auth |
| B2B SaaS | 40-55% | 65%+ | Multi-step progressive |
| E-commerce | 70-80% | 85%+ | Guest checkout default |
| Developer tools | 65-75% | 85%+ | GitHub/Google SSO primary |
| Fintech | 30-45% | 55%+ | Multi-step with verification |
| Healthcare | 25-40% | 50%+ | Multi-step with compliance |

**Note:** These are landing-page-to-account-created rates. Activation rates (account created → meaningful action) are typically 30-60% lower.

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It Hurts | Fix |
|-------------|-------------|-----|
| Requiring email confirmation before product access | Adds a step, loses momentum | Delay verification until needed |
| CAPTCHA on every signup | 10-20% drop in completion | Use invisible CAPTCHA or honeypot |
| Requiring phone number without explanation | 20-30% drop | Make optional or explain why |
| Disabling paste in password field | Frustrates password manager users | Always allow paste |
| "Confirm password" field | Adds friction, rarely prevents errors | Use show/hide toggle instead |
| Redirecting to login page after signup | Confusing, breaks flow | Auto-login after account creation |
| Terms checkbox before signup | Adds friction | "By signing up you agree to..." text |
| Asking for credit card on free trial | 50-70% reduction in signups | Ask only when trial expires |
