# Community Platform Comparison

Detailed comparison of community platforms by use case, pricing, and feature depth.

---

## Platform Matrix

| Feature | Discord | Slack | Circle | Reddit | Discourse | Facebook Groups |
|---------|---------|-------|--------|--------|-----------|----------------|
| **Best for** | Dev, gaming, creator | B2B, professional | Course, creator | Public, SEO | Technical, long-form | Consumer, older demo |
| **Real-time chat** | Excellent | Excellent | Good | Poor | Poor | Poor |
| **Async discussion** | Weak | Weak | Strong | Strong | Excellent | Moderate |
| **SEO benefit** | None | None | Optional | Strong | Strong | Weak |
| **Onboarding UX** | Moderate friction | Low friction | Low friction | Low friction | Moderate friction | Low friction |
| **Moderation tools** | Strong | Basic | Strong | Strong | Excellent | Moderate |
| **API/integrations** | Strong | Excellent | Moderate | Moderate | Strong | Weak |
| **Analytics** | Basic (bots) | Basic | Built-in | Subreddit stats | Built-in | Built-in |
| **Content ownership** | You own | You own | You own | Reddit owns | You own | Meta owns |
| **Mobile app** | Yes | Yes | Yes | Yes | Via browser | Yes |
| **Free tier** | Generous | Limited (90-day history) | None | Free | Free (self-host) | Free |
| **Paid plans from** | $2.99/user boost | $7.25/user/mo | $89/mo | N/A | $50/mo (hosted) | N/A |

---

## Decision Framework

### Choose Discord when:
- Your audience is developers, gamers, or creators
- Real-time interaction matters more than searchability
- You want voice/video channels for events
- Community is supplementary (not the core product)
- Budget is limited

**Watch out for:** High noise ratio, hard to search old conversations, onboarding friction for non-tech users, content gets buried in scroll.

### Choose Slack when:
- Your audience is business professionals already using Slack
- Conversations need to feel professional
- Integration with work tools matters (Notion, Jira, Google Workspace)
- Community is <1,000 members (free tier limit matters at scale)

**Watch out for:** Free tier only keeps 90 days of messages, feels like "more work" to members, expensive at scale.

### Choose Circle when:
- You're running courses, memberships, or creator programs
- You want discussion threads + events in one place
- Clean UX matters for your brand
- You need built-in member directory and analytics

**Watch out for:** No free tier, less organic discovery than open platforms, requires you to drive all traffic.

### Choose Reddit when:
- You want SEO benefit from community discussions
- Public visibility matters
- You don't need to own the community data
- Your audience already uses Reddit

**Watch out for:** You don't control the platform, trolling/moderation is hard, algorithm changes can reduce visibility, subreddit can be taken over.

### Choose Discourse when:
- Long-form technical discussions are the core format
- SEO-friendly content matters
- You want full data ownership (self-hosted option)
- Archival quality matters — conversations stay findable

**Watch out for:** Higher barrier to post (not chat-native), slower velocity, requires hosting if self-managed.

---

## Hybrid Approaches

Many successful communities use multiple platforms:

| Primary | Secondary | Use Case |
|---------|-----------|----------|
| Discord | Blog/Discourse | Real-time chat + searchable knowledge base |
| Circle | Slack | Async threads + team collaboration |
| Discourse | Discord | Long-form Q&A + real-time chat |
| Reddit | Email newsletter | Public discovery + owned audience |

**Rule:** Use one platform for real-time engagement, another for persistent/searchable content. Don't split your community across two similar platforms.

---

## Migration Considerations

### Moving communities between platforms:

1. **Announce early** — Give 4-6 weeks notice minimum
2. **Run both in parallel** — 2-4 weeks overlap period
3. **Move power users first** — They set the culture in the new space
4. **Archive old content** — Export and make searchable if possible
5. **Don't force it** — Some members won't migrate. Accept 60-70% migration as success.

### Data portability by platform:
- **Discord**: Full export via bots (DiscordChatExporter)
- **Slack**: Admin export available, loses formatting
- **Circle**: CSV export of members, no full content export
- **Discourse**: Full database export
- **Reddit**: Limited API export, no built-in tool
