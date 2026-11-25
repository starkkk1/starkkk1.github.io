---
title: "Event 3"
date: 2026-11-24
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# What I Took Away from Listening to Werner Vogels

> **Event:** Navigating the Future of Cloud & AI in Vietnam (Keynote by Dr. Werner Vogels)  
> **Date & Time:** 2026  
> **Location:** Ho Chi Minh City, Vietnam  
> **Role:** Attendee

I attended **"Navigating the Future of Cloud & AI in Vietnam"** to hear **Dr. Werner Vogels** (Amazon’s CTO) speak about the future of software engineering. As a student juggling coursework and side projects, the question *"Will AI replace programmers?"* feels very real to me. Looking around the auditorium, everyone was furiously taking notes—and whenever the speaker brought up AI, the entire room grew noticeably quieter.

---

### What Stuck with Me Most

- **The Peak-Shopping-Day Outage:** He shared a story from Amazon’s early days when their relational database was overwhelmed during the holiday rush, causing a day-long outage and massive losses. What stood out was their attitude: when the software vendor pointed out that Amazon had pushed the product far beyond its design limits, Vogels candidly admitted they were right. Instead of applying band-aid fixes, the engineering team stepped back to measure how the system was actually being used. They realized most queries were simple key-value lookups that didn’t need a heavy relational database. That insight led to the creation of **Dynamo**, and eventually **Amazon DynamoDB**.
- **Operational Discipline:** According to Vogels, relying on averages is an easy way to fool yourself, because averages hide the terrible experience of users at the extreme ends. He emphasized the mindset that *"everything fails all the time"* and explained how Amazon proactively runs drills by deliberately shutting down parts of their live infrastructure to test system resilience.
- **The "Renaissance Developer":** This was my favorite part. Programmers won't become obsolete if they evolve around five traits: **curiosity, systems thinking, ownership, broad knowledge paired with deep expertise, and communication skills**. He compared AI to a natural-language compiler—incredibly fast, yet prone to inaccuracies, meaning its output always requires human verification.

---

### 5 Lessons I’m Applying to My Own Work

1. **AI writes the code, but I own the responsibility:** I used to copy AI-generated code, run a quick test, and push it straight into my project if it worked. After this talk, I set a strict rule: never commit a single line of code I can't explain. Before accepting AI code, I need to check for edge cases, security vulnerabilities, and performance bottlenecks. If I don't understand it, I can't claim it as my own work.
2. **See the whole system, not just isolated features:** Most school assignments only focus on standalone modules. Soon, I plan to take a personal project, map out the entire data flow from the UI down to the database, and ask: *what happens if this single component dies?* Major outages almost always stem from unchecked assumptions.
3. **Understand the real workload before picking the tech stack:** I often choose tools just because they are popular or trending. Amazon’s story reminded me to start with actual usage patterns. From now on, before choosing a database or framework, I will list out the exact types of queries and data processing my app truly needs.
4. **Take communication skills seriously:** I used to treat communication as secondary, but I now see how it helps engineers uncover the core problem behind vague requests and explain technical trade-offs to non-technical stakeholders. I’m going to practice writing concise documentation for my work and presenting more often during group projects.
5. **Beware of *"we've always done it this way"*:** Dr. Werner Vogels closed with Grace Hopper’s famous warning about the most dangerous phrase in the language. To me, it’s a reminder to periodically audit and refresh how I learn and write code.

---

### Reflection & Next Steps

I walked out of the auditorium feeling much more relieved than anxious. The takeaway wasn't a hollow reassurance to "stop worrying about AI," but a clear look at what AI still cannot replace: **sound judgment, systems thinking, accountability, and professional pride**. Those are the exact skills I need to sharpen starting today.

My first step this week is to redraw the architecture diagram for my current project and audit every piece of code I previously let AI write for me. If you’re also early in your engineering journey like me, I hope these notes help.
