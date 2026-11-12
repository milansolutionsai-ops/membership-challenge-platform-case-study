# Membership Challenge Platform

**A 90-day members-only challenge platform for a Spanish-language creator membership with a six-figure contact list. I was the only technical person on it.**

Case study. The source is private (it's a client product), so this documents the scope, the numbers and the decisions.

---

## The numbers

| | |
|---|---|
| Participants | **870** |
| Complete profiles | **840, or 96%** |
| Submissions in the first three weeks | **3,665** |
| Active-participant retention, week 1 to week 3 | **~81%** |

The two I care about are the 96% and the 81%. Gated products normally bleed people at the profile step, and challenge formats normally collapse after week one. Neither happened.

---

## Scope

One person: product design, frontend, backend, database, the CRM layer underneath, the email journey, and deliverability.

- **Public bilingual landing page** (Spanish and English), built around real community video rather than stock
- **Membership-gated signup** validating against live CRM membership state, with an access model covering paying members, staff, influencers, professors and ambassadors
- **Member portal**: weekly challenges, private evidence uploads, a live leaderboard, profiles
- **Admin console**: participation stats, conversion tracking, leaderboard management, winner publication, team invitations, media management
- **Nine email templates**, seven in the CRM plus two branded transactional ones
- **Full deliverability audit** of the sending infrastructure

**Stack:** Next.js · React · TypeScript · Tailwind · Supabase · GoHighLevel · Vercel

---

## Three decisions worth naming

**The access gate fails closed.** If the CRM is unreachable, access is denied rather than granted. A membership gate that fails open during an outage is not a membership gate.

**No dead ends in the gate.** A non-member who hits it is routed to the purchase funnel. A mis-tagged member gets an actionable message and a support path rather than a generic rejection. An auth check is a conversion surface; treating it as a wall wastes the most motivated traffic you will ever get.

**Conversions are counted as a de-duplicated union of two independent signals**, funnel purchase and lead-turned-active, with the source label kept attached per contact and carried into the CSV export. When a platform won't tell you cleanly where a conversion came from, build the measurement yourself and keep the provenance rather than collapsing it into a number nobody can audit.

---

## A bug worth telling you about

Internal staff filling in forms on a shared browser were **silently overwriting the wrong contact**.

GoHighLevel's sticky-contact feature is cookie-based rather than domain-based, so the browser kept handing back a cached contact regardless of what was typed. The part that made it hard: **the form log displayed the correct typed email while the workflow ran against the cached one.** The primary evidence pointed the wrong way. And clearing the cache doesn't fix it, because it's cookies, so the obvious first attempt looks like it disproves your own theory.

Fix: sticky contact off, plus a fresh incognito window per submission, or provision directly in the contact record.

---

More of my work: [milan-solutions.vercel.app](https://milan-solutions.vercel.app) · [LinkedIn](https://www.linkedin.com/in/milan-veljkovikj)
