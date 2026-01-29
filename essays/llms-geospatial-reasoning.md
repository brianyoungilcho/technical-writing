# Why LLMs Fail at Geospatial Reasoning

*And what it means for real-world AI deployment*

---

I work on acoustic intelligence products at Flock Safety. Part of my job involves figuring out where to deploy devices across cities — a problem that sounds simple until you try to solve it with AI.

Here's what I've learned: current LLMs are remarkably bad at geospatial reasoning. And this limitation has significant implications for anyone trying to deploy AI in the physical world.

## The Problem

Consider this prompt:

> "Given crime data hotspots, traffic patterns, and available utility poles, recommend optimal locations for 50 acoustic sensors across a 10-square-mile area."

A human expert can look at a map and reason about:
- Coverage overlap and gaps
- Line-of-sight considerations
- Infrastructure constraints (which poles can support devices)
- Response time optimization (how quickly can police reach each area)
- Adjacent jurisdiction boundaries

An LLM will give you a confident-sounding answer that falls apart on inspection.

## Why LLMs Struggle

### 1. Spatial Relationships Don't Tokenize Well

LLMs process text sequentially. Spatial relationships are inherently parallel — "north of X and east of Y" requires holding multiple dimensions simultaneously.

When you describe a city layout in text, the model loses the actual *geometry*. It might understand that location A is "near" location B, but it can't reason about whether a sensor at A would provide redundant coverage with one at B.

### 2. No Persistent World Model

Humans build and maintain a mental model of space. When we say "put a sensor at 5th and Main," we update our internal map and all subsequent reasoning accounts for it.

LLMs don't have this. Each token prediction is based on the prompt context, but there's no persistent spatial representation being updated. Ask about the 50th sensor placement, and the model has to re-derive the implications of the previous 49 from text — a task it can't reliably do.

### 3. Scale Distortion

LLMs trained on text have seen phrases like "a few blocks away" and "across the city." But the actual distance those phrases represent varies wildly by context. In Manhattan, "a few blocks" might be 500 feet. In a suburban area, it might be a mile.

The model has no grounding in actual distances, so its spatial recommendations often violate basic geometric constraints.

## What This Means for AI Deployment

### Build vs. Buy Implications

If you're building products that require spatial reasoning, you probably can't rely on off-the-shelf LLMs. This affects:

- **Logistics and routing** (warehouse layout, delivery optimization)
- **Urban planning tools** (zoning recommendations, infrastructure planning)
- **Real estate applications** (location analysis, comparable identification)
- **IoT deployment** (sensor placement, coverage optimization)

For these applications, you need specialized models or hybrid approaches.

### Hybrid Approaches That Work

What we've found effective:

**1. LLMs for synthesis, GIS for spatial logic**

Use LLMs to understand natural language inputs ("focus on high-crime areas but avoid residential zones") and translate them into structured queries. Then use actual GIS tools (PostGIS, Turf.js, etc.) for spatial computation.

**2. Embedding spaces with geographic priors**

Instead of asking the LLM to reason about locations directly, embed locations with geographic features (lat/long, census data, crime stats) and do similarity matching in that space.

**3. Iterative refinement with human-in-the-loop**

Generate initial recommendations with traditional algorithms, use LLMs to explain the tradeoffs in natural language, and let humans refine. The LLM adds value in communication, not computation.

### What Needs to Change

For LLMs to get better at geospatial reasoning, we probably need:

1. **Multi-modal training** — Models trained on maps, satellite imagery, and text together
2. **Explicit spatial representations** — Architectural changes that maintain geometric relationships
3. **Tool use** — Models that know to call GIS APIs rather than hallucinate spatial reasoning

Some of this is happening (GPT-4V can look at maps), but we're far from models that can reliably reason about physical space.

## The Takeaway

If you're building products that touch the physical world, be skeptical of LLM-generated spatial recommendations. Test rigorously. Build hybrid systems. And don't assume that models will "get better at this" in the next version — spatial reasoning is a fundamental architectural limitation, not just a training data gap.

The good news: this is a genuine competitive moat. If you solve spatial reasoning well for your domain, you're protected from "just use GPT" competitors.

---

*I'm building acoustic intelligence products at Flock Safety and writing about AI limitations I encounter in practice. Connect on [LinkedIn](https://linkedin.com/in/bcho).*
