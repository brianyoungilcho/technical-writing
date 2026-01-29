# Grounding LLM Outputs with Vector Matching

*How we reduced hallucinations and increased LTV by 20%*

---

At Jobtest.org, we used LLMs to generate personalized career recommendations. The problem: GPT-3 would confidently recommend jobs that didn't exist.

This is a common failure mode when using LLMs for recommendations. Here's how we fixed it — and the broader pattern for grounding AI outputs in reality.

## The Hallucination Problem

Our career quiz collected data about personality traits, skills, interests, and values. We fed this to GPT-3 and asked for career recommendations.

The model was *excellent* at synthesis. It could weave together seemingly unrelated traits into coherent career narratives:

> "Your combination of analytical thinking and desire for social impact, combined with your interest in emerging technology, makes you well-suited for roles in Climate Tech Product Management or Sustainable Finance Analytics."

The problem? "Climate Tech Product Management" wasn't a job title that appeared in any job posting database. Users who searched for it found nothing. They felt misled.

Worse, the model would sometimes invent entire career paths — "Digital Wellness Coordinator" or "AI Ethics Consultant" — that sounded plausible but had minimal real-world demand.

**The result:** ~15% of our recommendations pointed to jobs users couldn't find. Trust eroded. Refund requests increased. LTV suffered.

## The Solution: Vector Matching

The insight was simple: separate **synthesis** (what LLMs do well) from **factual claims** (what they do poorly).

### Architecture

```
User Quiz Data
      ↓
┌─────────────────────────────┐
│   Embedding Generation      │
│   (quiz answers → vector)   │
└─────────────────────────────┘
      ↓
┌─────────────────────────────┐
│   Vector Similarity Search  │
│   (match against job DB)    │
└─────────────────────────────┘
      ↓
   Top N Real Jobs
      ↓
┌─────────────────────────────┐
│   LLM Synthesis             │
│   (explain why these jobs   │
│    match this person)       │
└─────────────────────────────┘
      ↓
   Final Recommendation
```

### Step 1: Build a Job Embedding Database

We embedded ~50,000 real job postings using their titles, descriptions, and requirements. Each job became a vector in a high-dimensional space where similar jobs clustered together.

Key decisions:
- Used OpenAI's `text-embedding-ada-002` for embeddings
- Stored in Pinecone for fast similarity search
- Updated weekly from job board APIs to stay current

### Step 2: Embed User Profiles

We converted quiz responses into a text representation:

```
"Analytical thinker who values work-life balance. 
Strong interest in technology and environmental impact. 
Prefers collaborative environments over solo work.
5 years experience in data analysis."
```

Then embedded this profile into the same vector space as the jobs.

### Step 3: Retrieve Real Jobs

Vector similarity search returned the top 20 jobs whose embeddings were closest to the user profile. These were *real* jobs with *real* postings that users could actually apply to.

### Step 4: LLM Synthesizes (Doesn't Invent)

Now we gave the LLM a constrained task:

```
Given this user profile: [profile]
And these matching jobs: [list of 20 real jobs]

Explain why the top 5 jobs are good fits for this person.
Do NOT recommend any jobs not in this list.
```

The model could still do what it's good at — creating compelling narratives — but it couldn't hallucinate job titles. Every recommendation was grounded in reality.

## Results

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Hallucinated jobs | ~15% | <1% | -93% |
| User trust score | 3.2/5 | 4.1/5 | +28% |
| LTV | $47 | $56 | +20% |
| Support tickets (job not found) | ~50/week | ~2/week | -96% |

## The General Pattern

This approach generalizes beyond career recommendations:

### E-commerce Product Recommendations

**Don't:** "Based on your preferences, you might like the Sony WH-2000XM5 headphones"  
(Model might hallucinate product names or specs)

**Do:** Embed user preferences → match against product catalog → have LLM explain why matched products fit

### Medical Information

**Don't:** "Your symptoms suggest you might have [condition]"  
(Dangerous hallucination territory)

**Do:** Match symptoms to verified medical database → have LLM explain the matches in accessible language

### Legal Research

**Don't:** "The relevant case law is Smith v. Jones (2019)"  
(Model will confidently cite cases that don't exist)

**Do:** Embed legal question → retrieve from verified case database → have LLM synthesize relevance

## Implementation Notes

### Embedding Quality Matters

We experimented with different embedding approaches:
- Pure job title embeddings (too narrow — missed conceptual matches)
- Full job description embeddings (too noisy — picked up irrelevant details)
- Structured embeddings (title + requirements + key responsibilities) worked best

### Retrieval Count Affects Quality

Too few retrieved jobs (5) meant sometimes the best matches weren't in the set. Too many (100) gave the LLM too much to work with and quality dropped.

We settled on retrieving 20, having the LLM rank and explain the top 5.

### Update Frequency

Stale job data was almost as bad as hallucinations. Users would find their recommended job had been filled. Weekly updates were the minimum viable frequency.

## The Takeaway

LLMs are synthesizers, not databases. When your application requires factual accuracy:

1. **Separate retrieval from generation** — use vector search (or any grounded retrieval) for facts
2. **Constrain the LLM's output space** — give it a list of valid options, not free reign
3. **Let the LLM do what it's good at** — explaining, synthesizing, personalizing

The result is outputs that are both compelling *and* reliable. Users get the benefits of AI personalization without the trust-destroying hallucinations.

---

*Building something that needs grounded AI outputs? Happy to chat — [LinkedIn](https://linkedin.com/in/bcho).*
