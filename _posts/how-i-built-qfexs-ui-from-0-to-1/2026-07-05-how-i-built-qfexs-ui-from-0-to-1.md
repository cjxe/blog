---
title: "How I built the QFEX trading UI from 0 → 1"
date: 2026-07-05 18:00:00 +01:00
tags: [technology, finance]
description: The story of joining QFEX as the first hire and building a trading exchange UI from scratch — from recruiter DMs to MVP launch.
toc: true
---

It all started with:

<figure>
	<img src="/how-i-built-qfexs-ui-from-0-to-1/recruiter-linkedin-message.png" alt="LinkedIn message from recruiter Luke">
</figure>

Was it an offer to become a YC co-founder? That's too good to be true, and I was already in the loop with other companies. So, I kept focusing on those.

*Fast forward 1 day*

Luke followed up, and one detail in his message caught my attention:

<figure>
	<img src="/how-i-built-qfexs-ui-from-0-to-1/recruiter-follow-up.png" alt="Follow-up LinkedIn message mentioning YC-backed exchange startup">
</figure>

I hopped on the 2-minute call and started taking notes:

- YC backed
- Cambridge Mathematics graduates
- Ex-Citadel Software Engineer Co-Founder
- Ex-Tower Research Quant Co-Founder
- Building the next biggest exchange
- The first LinkedIn post was 3 days ago

Perfect! So I asked Luke to put me forward for the role.

# Interviewing to get the job

Most tech interviews follow a standard process, and it was similar at QFEX:

1. **Take-home test**: This step usually consists of a LeetCode-style challenge or a product problem. I was asked to implement a sign-up flow using mock responses. They also mentioned I could use AI, so I bought Cursor's monthly plan, and managed multiple agents for implementation detail tasks to finish the project in time. It was the highest ROI purchase of my life.
2. **Live pair programming**: I was given a simple but broken React app and asked to fix it. Thankfully, my LeetCode skills were already sharp since I was interviewing at other companies.
3. **Behavioural interview**: This step focused on my motivation and validated my past experience by going into the details. I answered the "Big 3" questions (tell me about yourself, favourite project, and conflict resolution) and answered the rest of the questions using the STAR method.

After each round, Luke called to tell me I'd made it through. I was in shock. I couldn't believe what was happening. 

It was time for the final call and negotiations...

<figure>
	<img src="/how-i-built-qfexs-ui-from-0-to-1/negotiation-bingo.png" alt="Negotiation call notes with BINGO highlighted">
</figure>

BINGO!

# Leaving my first job

After I signed my QFEX employment contract, I called my manager and handed in my resignation. She was a great manager, and the team was amazing. I had grown a lot and genuinely enjoyed working with them, so it wasn't easy to leave. Even though my manager was sad to see me go, she was understanding. She even arranged a lovely leaving drinks to give me a proper goodbye. 

<figure>
	<img src="/how-i-built-qfexs-ui-from-0-to-1/leaving-drinks.png" alt="Leaving drinks with former colleagues">
</figure>

# Day 1

I woke up, took a different route to my new workplace, collected my new badge, and walked into the new office.

Now what?

# Understanding the problem

It's me and the two co-founders, both worth millions of dollars, in a tiny room. They'll handle the backend services, and I'll build the exchange UI. My boss walks me through the current prototype, but there are a few concerns with it. So, I start asking questions to understand the underlying problems.

Going back to first principles:

- **Problem**: Traditional stock, ETF, and FX exchanges don't allow 24/7 trading or leveraged exposure in a seamless way.
- **Solution**: Build a new exchange that enables both.

As someone who had built a [full-stack CLOB exchange](https://github.com/cjxe/on-chain-dex) before, I was familiar with many of the user flows and requirements, but I wasn't completely sure yet. So, I started looking for a designer with industry experience to help guide us.

# Interviewing the designers

To be honest, I barely knew what I was doing in my first few interviews. I'd never interviewed anyone before, let alone a designer. It's not like I could ask a programming question selected from a question bank, and expect them to find the one solution.

But it wasn't all hopeless. As I interviewed more candidates, I started identifying the signals and differentiators that would make or break both the company and the product.

Before I share my cheat sheet, a small disclaimer: **every problem is different thus requires a different solution**. The summary below reflects QFEX's vision and the challenges we were trying to solve.

1. **Taste**: Ask them to walk through the most visually appealing and user-friendly design they've created. What was the story behind it?
2. **Culture fit**: Speedy and self-driven.
3. **Industry experience**: Experience in designing trading terminals.

Of course there were other important signals too, such as clear communication, comfort with ambiguity, and conflict resolution. But most of them could be inferred from the conversations above during the interview.

*Fast forward a few days*

Interviewed a few candidates, identified a few signals, made one offer, and they accepted it. We were over the moon!

(In hindsight, we weren't only lucky to hire a very good designer, but also our first-choice candidate accepted the offer. Usually, candidates use an offer to negotiate a better one elsewhere, but this wasn't the case here.)

# Gathering requirements

The designer started, it's his first day. He handed us sticky notes and a pen, and asked us to write down every feature we expected from a financial exchange.

I froze.

I knew Binance and Coinbase existed, but unlike the rest of the team, I'd never used a traditional finance trading platform before. When my boss asked why I wasn't writing anything, I replied, "I don't know what to write.". I hoped he wasn't too disappointed that day.

After an hour, we had an entire wall of sticky notes with features.

<figure>
	<img src="/how-i-built-qfexs-ui-from-0-to-1/sticky-notes-wall.png" alt="Wall covered in sticky notes listing exchange features">
		<figcaption>A part of the wall covered in sticky notes. This is the only photo I have.</figcaption>
</figure>

# Launching first to beat the market

We (mostly they) discussed the must-have features and kept narrowing the scope so we could launch as quickly as possible. There wasn't a major 24/7 exchange at the time, so we had to beat the market and be first. That was only possible with a small, fast-moving team and a focused MVP.

The MVP scope was:

- browse and search all products → **Product Search**
- view a product's price across time → **Price Chart**
- place a new order → **Order Ticket**
- view open orders and filled trades → **Blotter**
- use it on both mobile and desktop → **Responsive Design**

Great. Now let's make it work before we make it look sexy.

# Functionality first, then styling

While I was interviewing for a role at QFEX, I had already finished my interview process at Coinbase. The biggest learning was to focus on functionality first and styling second. I hadn't given that approach much thought before, but it changed how I tackled the requirements.

Going back to the QFEX requirements, I started with the product search component since it didn't require authentication, then broke it down into the simplest possible tasks:

How can the UI achieve the following?

- Show 1 product's price.
- Update prices in real time.
- Show 2 products' price.
- Show ~100 products' price.

The implementation detail was WebSockets.

<figure>
	<img src="/how-i-built-qfexs-ui-from-0-to-1/app-second-week.png" alt="Early QFEX app screenshot from the second week">
	<figcaption>A screenshot of the app in my second week</figcaption>
</figure>

From now on, it was mostly iterations.

- Keep it simple and small
- Make it work
- Make it sexy
- Make it scale

<figure>
	<img src="/how-i-built-qfexs-ui-from-0-to-1/ui-after-one-year.png" alt="QFEX UI after one year of development">
	<figcaption>How the <a href="https://qfex.com/trade">QFEX UI</a> looks after 1 year</figcaption>
</figure>

Each feature has a history behind it, and I can't cover it all in one post. I haven't decided on the next post yet, but it will be a technical one.

---