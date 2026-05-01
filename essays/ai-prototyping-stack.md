# My AI Prototyping Stack

*Claude, Lovable, Supabase, and how I ship fast*

> **Note (2026):** This essay describes the stack and workflow I used for that season of shipping. Current tooling is summarized in [**AI Prototyping Patterns**](https://github.com/brianyoungilcho/ai-prototyping-patterns) (Claude, Cursor, Codex, Vercel, Supabase).

---

I solo-built [Pupsday.com](https://pupsday.com) — a dog subscription e-commerce platform — in a few weeks. It's now profitable at $1K+ MRR with 30% margins.

I didn't use Shopify. I didn't hire a development team. I used AI tools to prototype faster than traditional approaches would allow.

Here's my stack and workflow.

## The Stack

| Tool | Role | Why |
|------|------|-----|
| **Claude (Opus/Sonnet)** | Architecture, code generation, debugging | Best reasoning for complex decisions |
| **Cursor** | AI-augmented IDE | Claude integration in editor |
| **Lovable** | Rapid UI prototyping | Sketch to code in minutes |
| **Supabase** | Backend, auth, database, edge functions | Postgres + Auth + Realtime in one |
| **Vercel** | Deployment | Zero-config deploys |

## Why Not Shopify?

Pupsday's core value proposition is personalized dog food recommendations based on a quiz about your dog's breed, age, activity level, health conditions, etc.

Shopify's quiz apps couldn't support:
- Complex branching logic (if senior dog + joint issues → different flow)
- Custom scoring algorithms
- Tight integration with subscription management
- The specific UX I wanted

I tried. The Shopify ecosystem is optimized for standard e-commerce. Pupsday isn't standard e-commerce.

Building custom meant I could:
- Control every aspect of the quiz experience
- Implement custom recommendation logic
- Iterate on the conversion funnel without app limitations
- Own the technical architecture completely

## The Workflow

### Phase 1: Design with Lovable

Lovable lets me describe a UI and get working React code.

**Example prompt:**
> Create a multi-step quiz interface for dog owners. Each step should have a progress bar, large touch-friendly buttons for mobile, and smooth transitions between questions. The design should feel premium and trustworthy — think "millennial pet parent" aesthetic.

I get a working prototype in minutes. It's not production-ready, but it's enough to validate the UX concept and iterate on the design.

### Phase 2: Architecture with Claude

Once I have a design direction, I use Claude to plan the technical architecture.

**Example conversation:**
> I'm building a dog food subscription service. Users take a quiz about their dog, get personalized food recommendations, and subscribe to monthly deliveries.
>
> Key requirements:
> - Quiz with branching logic
> - Recommendation engine based on quiz answers
> - Stripe subscription management
> - User accounts with order history
>
> I want to use Supabase for the backend. Help me design the data model and API structure.

Claude gives me:
- Database schema
- API endpoint structure
- Auth flow design
- Edge function architecture

I ask follow-up questions, challenge assumptions, and iterate until I have a solid plan.

### Phase 3: Implementation with Cursor

Cursor is VS Code with Claude built in. I can:
- Highlight code and ask Claude to modify it
- Generate new functions from descriptions
- Debug errors by showing Claude the stack trace
- Refactor across multiple files

**Example workflow:**

1. Claude generates initial implementation of the quiz flow
2. I run it, find issues
3. I paste the error into Cursor, ask "why is this failing?"
4. Claude explains and suggests a fix
5. I apply the fix, continue

This is dramatically faster than Googling every error and reading Stack Overflow.

### Phase 4: Backend with Supabase

Supabase gives me:
- **Postgres database** with a nice GUI for schema management
- **Authentication** with email, social providers, etc.
- **Row-level security** so users can only see their own data
- **Edge functions** for custom logic (recommendation engine, webhook handlers)
- **Realtime subscriptions** (useful for admin dashboard)

**Example: Recommendation Engine**

```typescript
// Supabase Edge Function
import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'

serve(async (req) => {
  const { quizAnswers } = await req.json()
  
  // Score each food option based on quiz answers
  const recommendations = scoreFoods(quizAnswers)
  
  // Return top 3 recommendations
  return new Response(
    JSON.stringify({ recommendations: recommendations.slice(0, 3) }),
    { headers: { 'Content-Type': 'application/json' } }
  )
})
```

The recommendation logic is custom — it accounts for breed size, age, activity level, health conditions, and dietary preferences. This is the "secret sauce" that Shopify apps couldn't provide.

### Phase 5: Deploy with Vercel

Push to GitHub → Vercel deploys automatically.

```bash
git push origin main
# ... deployed in 30 seconds
```

## Key Principles

### 1. Use AI for Speed, Not Perfection

AI-generated code isn't always optimal. But it's *fast*. I can generate a working first version, then optimize the parts that matter.

The quiz conversion funnel? Every millisecond matters. I optimized that carefully.

The admin dashboard? Good enough is good enough.

### 2. Own the Core Logic

I use AI to generate boilerplate, UI components, and standard patterns. But the recommendation engine — the thing that makes Pupsday valuable — I designed carefully and reviewed every line.

Don't let AI generate your competitive advantage without understanding it deeply.

### 3. Iterate in Production

With this stack, deploying is so cheap that I treat production as my testing environment (for non-critical features).

New idea → build it → deploy → watch real user behavior → iterate.

The feedback loop is hours, not weeks.

### 4. Start Smaller Than You Think

Pupsday launched with:
- One quiz flow
- Three food options
- Monthly subscription only
- Stripe checkout (no custom payment flow)

It was enough to validate demand and start generating revenue. Every feature since has been driven by actual user feedback, not speculation.

## What This Stack Can't Do

**Complex integrations:** If you need deep integration with existing enterprise systems, this stack is too lightweight.

**Heavy computation:** Supabase edge functions have limits. ML inference or heavy data processing needs different infrastructure.

**Team collaboration:** This setup is optimized for solo builders. With a team, you'd need more structure (testing, code review, documentation).

**High-stakes applications:** I wouldn't build medical or financial products this way. The speed comes with tradeoffs in rigor.

## The Economics

My monthly costs:
- Supabase: $25 (Pro plan)
- Vercel: $0 (free tier sufficient)
- Stripe: 2.9% + $0.30 per transaction
- Domain: ~$12/year

At $1K MRR with 30% margins, the infrastructure cost is negligible.

Compare to Shopify: $79/month basic + app costs + transaction fees. And I'd still have the quiz limitation problem.

## Getting Started

If you want to try this approach:

1. **Pick a small, contained project.** Something you could launch in a weekend.
2. **Start with Lovable or v0** to get the UI direction.
3. **Use Claude to plan the architecture** before writing code.
4. **Set up Supabase** — follow their quickstart, it's well-documented.
5. **Build in Cursor** with Claude as your pair programmer.
6. **Deploy to Vercel** for instant feedback.

The first project will feel slow as you learn the tools. The second will be dramatically faster.

---

*Building something? I'd love to hear what you're working on — [LinkedIn](https://linkedin.com/in/bcho).*
