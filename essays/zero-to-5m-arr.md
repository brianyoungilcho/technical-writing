# Zero to $5M ARR: What Actually Worked

*Lessons from building Jobtest.org with a 2-person team*

---

In 2022, I co-founded Jobtest.org — a B2C career marketplace. By 2025, we had reached $5M ARR, 200K MAU, and a 35% session-to-paid conversion rate. We did this with a team of two.

Here's what actually worked.

## The Uncomfortable Truth About Early-Stage Growth

Most startup advice assumes you have resources: a team to A/B test, budget to experiment, time to iterate. When it's just you (or two of you), you don't have the luxury of "move fast and break things." Every decision has compounding consequences.

The frameworks that worked for us:

### 1. Optimize for Learning Velocity, Not Growth Velocity

Early on, we could have chased user acquisition. Instead, we obsessed over understanding *why* people paid. 

Our first 100 paying customers taught us more than our first 10,000 free users. We called them. We asked what they expected vs. what they got. We learned that people didn't want "career advice" — they wanted permission to pursue careers they were already curious about.

That insight shaped everything: our quiz design, our recommendation framing, our pricing.

### 2. The GPT-3 Opportunity (And Its Trap)

When GPT-3 launched, I saw an immediate application: synthesizing user quiz data into career insights. The model was excellent at this — it could weave together personality traits, skills, and preferences into compelling narratives.

But there was a problem: it hallucinated job roles that didn't exist.

Users would get recommendations for "Digital Wellness Coordinator" or "Sustainable Innovation Strategist" — titles that sounded real but had no job postings. This wasn't just a quality issue; it was a trust issue. Users who couldn't find jobs matching their recommendations felt deceived.

**The fix:** We implemented vector-matching algorithms that grounded recommendations in real job data. The LLM could still synthesize and explain, but the actual job recommendations came from embeddings matched against verified job postings.

Results:
- Hallucination rate dropped from ~15% to <1%
- LTV increased by 20% (users trusted recommendations more → completed more purchases)
- Support tickets about "fake jobs" went to zero

**Lesson:** LLMs are powerful for synthesis and explanation, but unreliable for factual claims. Ground their outputs in verified data.

### 3. Conversion Rate > Traffic

We never cracked paid acquisition. Our CAC was always too high for the market. So we focused relentlessly on conversion.

35% session-to-paid sounds impressive, but it came from hundreds of micro-optimizations:
- Quiz length (12 questions was optimal — fewer felt shallow, more caused drop-off)
- Progress indicators (showing "you're 80% done" increased completion by 23%)
- Immediate value (free users got *some* insights, paid users got depth)
- Friction at the right moment (asking for payment after the "aha moment," not before)

### 4. Marketplace Dynamics Changed Everything

Our initial model was simple: users pay for career recommendations. $2M ARR.

The shift to $5M came from recognizing we had something more valuable: intent data. Users who completed our quiz had explicitly told us what they wanted to do next.

We built a marketplace connecting users to relevant services (courses, coaching, certifications). Revenue per user increased 150%. The same traffic, dramatically different economics.

## What I'd Do Differently

**Ship the marketplace earlier.** We waited too long to test this model. The infrastructure wasn't that different — we had the user intent data from day one.

**Hire for customer success earlier.** Our churn rate was higher than it needed to be. Users who got stuck needed a human touch we couldn't provide at scale with two people.

**Build in public sooner.** Our best acquisition channel ended up being word-of-mouth. If we had documented our journey publicly, we might have accelerated this.

## The Takeaway

Building to $5M ARR with two people wasn't about being smarter or working harder. It was about:

1. Choosing a market where small teams could win (B2C, self-serve, high-intent)
2. Obsessing over unit economics from day one
3. Using AI as a lever, not a crutch (ground outputs in reality)
4. Optimizing conversion over acquisition when acquisition is expensive

The unsexy truth is that most of our growth came from incremental improvements to a fundamentally sound model. No viral moments. No hockey stick. Just compounding gains from understanding our users better than anyone else.

---

*Questions? Reach out on [LinkedIn](https://linkedin.com/in/bcho) or [Twitter](https://twitter.com/brianyoungilcho).*
