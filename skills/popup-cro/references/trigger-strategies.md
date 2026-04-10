# Popup Trigger Strategies

Detailed trigger configurations, timing data, and platform-specific implementation.

---

## Trigger Type Comparison

| Trigger | Avg Conversion | Annoyance Risk | Best For | Mobile Support |
|---------|---------------|----------------|----------|---------------|
| Exit intent | 3-10% | Medium | Last-chance offers | Limited (no cursor tracking) |
| Scroll depth (50%) | 2-5% | Low | Content engagement | Full |
| Time delay (30-60s) | 2-4% | Medium-High | General visitors | Full |
| Click-triggered | 10-20% | Zero | Lead magnets, demos | Full |
| Page count (3+) | 3-6% | Low | Research/comparison behavior | Full |
| Cart abandonment | 5-15% | Low | E-commerce | Full |
| Inactivity (30s idle) | 2-4% | Medium | Re-engagement | Limited |

---

## Exit Intent Configuration

### Desktop Implementation
Detects cursor movement toward browser chrome (close button, address bar, tabs).

**Recommended settings:**
- Sensitivity: 20-50px from top edge
- Delay after page load: Minimum 10 seconds (don't trigger on accidental moves)
- Frequency: Once per session
- Cool-down after dismiss: 7-30 days

### Mobile Alternatives
Mobile has no cursor, so exit intent doesn't work. Use these substitutes:

| Alternative | How It Works | Reliability |
|-------------|-------------|-------------|
| **Back button press** | Intercept browser back navigation | High but intrusive |
| **Scroll-up** | Fast upward scroll suggests leaving | Medium (false positives) |
| **Tab switch** | Visibility API detects tab change | High |
| **Inactivity** | No interaction for 30-60 seconds | Medium |
| **Bottom-of-page** | Reached end of content | High for content pages |

**Recommended mobile approach:** Combine scroll-up + inactivity as dual trigger. Show bottom slide-in (not full-screen modal).

---

## Scroll-Based Triggers

### Optimal Depth by Page Type

| Page Type | Recommended Trigger Depth | Rationale |
|-----------|--------------------------|-----------|
| Blog post (long) | 40-60% | Reader is engaged but hasn't finished |
| Landing page | 60-80% | Has seen the pitch, needs a nudge |
| Product page | 50-70% | Has reviewed features/specs |
| Pricing page | 70-90% | Comparing options, offer help |
| Homepage | 30-50% | Exploring, capture early |

### Progressive Scroll Triggers
Instead of a single trigger, use progressive engagement:

```
25% scroll → Show subtle sticky bar ("Get the guide")
50% scroll → Expand sticky bar with more detail
75% scroll → Show slide-in with full offer
Exit intent → Show modal with best offer
```

**Each subsequent trigger only fires if the previous was not engaged with.**

---

## Time-Based Triggers

### Timing Benchmarks

| Delay | Use Case | Risk |
|-------|----------|------|
| 0-5 seconds | Never recommended (feels aggressive) | Very high annoyance |
| 5-15 seconds | Returning visitors who know you | Medium annoyance |
| 15-30 seconds | Warm traffic (from ads, referrals) | Low-medium annoyance |
| 30-60 seconds | Cold traffic, first-time visitors | Low annoyance |
| 60+ seconds | Long-form content readers | Very low annoyance |

**Best practice:** Start at 30 seconds and test shorter. Never go below 10 seconds for first-time visitors.

### Smart Timing
Combine time with engagement signals:

```
IF time_on_page > 30s AND scroll_depth > 25%
  → Show popup (user is engaged, not bouncing)

IF time_on_page > 60s AND scroll_depth < 10%
  → Don't show (user is idle/distracted, not reading)
```

---

## Click-Triggered Patterns

Zero-annoyance popups that convert at 10-20% because the user self-selected.

### Implementation Patterns

**Inline content upgrade:**
```
[Article about email marketing]

📥 Get the complete email template pack (12 templates)
[Download Free →]  ← Click opens popup form
```

**Navigation CTA:**
```
[Header navigation]
[Product] [Pricing] [Blog] [Get Demo ←] ← Click opens popup
```

**Content gate (partial reveal):**
```
[Visible content: first 3 items of a 10-item list]

🔒 See all 10 items — enter your email
[Unlock Full List →]
```

**Exit-area CTA (footer/sidebar):**
```
[End of article]
Enjoyed this? Get weekly tips like this.
[Subscribe →]
```

---

## Behavior-Based Triggers

### High-Intent Signals

| Behavior | What It Signals | Recommended Popup |
|----------|----------------|-------------------|
| Visited pricing page | Considering purchase | "Questions about pricing? Chat with us" |
| Viewed 3+ product pages | Researching/comparing | "Want a personalized demo?" |
| Returned within 7 days | Still interested | "Welcome back — ready to start?" |
| Added to cart, didn't buy | Purchase hesitation | "Complete your order — 10% off" |
| Viewed competitor comparison | Active evaluation | "See how we compare — get a free trial" |
| Spent 2+ min on feature page | Deep interest | "Want to see [feature] in action?" |

### Low-Intent Signals (Don't Trigger)

| Behavior | What It Signals | Action |
|----------|----------------|--------|
| Bounced in <10 seconds | Wrong page / not interested | Don't show popup |
| Only visited 1 page | Casual browsing | Wait for more engagement |
| Already converted | Existing customer | Exclude from lead gen popups |
| Dismissed popup today | Not interested right now | Respect the dismissal |

---

## Conflict Resolution Rules

When running multiple popups, prevent them from colliding.

### Priority Hierarchy
```
1. Cart abandonment (highest priority — active revenue)
2. Exit intent offer
3. Behavior-triggered (pricing page, repeat visit)
4. Scroll-based content upgrade
5. Time-delayed general offer (lowest priority)
```

### Rules Engine
```
- Maximum 1 popup per page view
- Maximum 1 popup per session (unless click-triggered)
- If popup A was dismissed, don't show popup B for 24 hours
- Click-triggered popups always allowed (user-initiated)
- Never show popups on checkout, login, or settings pages
- Returning visitors who dismissed: wait 7-30 days
```

---

## Measurement by Trigger Type

Track these metrics for each trigger independently:

| Metric | What It Tells You |
|--------|-------------------|
| Impression rate | % of visitors who see the popup |
| Instant close rate | % who close within 2 seconds (trigger is too aggressive) |
| Engagement rate | % who interact before closing (read, hover, scroll popup) |
| Conversion rate | % who submit/click CTA |
| Time to close | Average seconds before dismiss (longer = more interest) |
| Post-popup bounce rate | Do dismissed users leave the site? |

**Red flags:**
- Instant close rate > 70%: Trigger is too aggressive or irrelevant
- Post-popup bounce rate increases: Popup is hurting UX
- Conversion rate < 1%: Offer or copy needs improvement
