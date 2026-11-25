---
title: "Event 1"
date: 2026-10-24
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Saturday Morning on the 26th Floor of Bitexco: One Morning, Five Sessions, and a Few Habits I Want to Change

> **Event:** First Cloud AI Journey  
> **Date & Time:** 09:00 – 12:00, Saturday, October 24, 2026  
> **Location:** 26th Floor, Bitexco Financial Tower, Ho Chi Minh City  
> **Role:** Attendee

On Saturday morning, October 24, I headed up to the 26th floor of the Bitexco Financial Tower for the **"First Cloud AI Journey"** meetup. Packed into just three hours (from 9:00 AM to 12:00 PM), the event featured five back-to-back sessions on Cloud and AI Agents led by Tech Leads and engineers actively working in the industry. As a student, I went in simply to listen and get a feel for the space. The atmosphere was relaxed, yet the moment a live demo or Q&A began, the entire room fell dead silent as everyone started furiously taking notes.

---

### What Stood Out Most

- **The Voice AI Demo:** The opening session covered building a Vietnamese voice call center on AWS. The speaker emphasized latency—even a split-second delay makes a conversation feel unnatural. The highlight was a live demo where the AI placed a real phone call to conduct an initial screening interview for a cloud engineer role in Vietnamese. Hearing it converse so smoothly proved that this technology has moved well beyond slide decks into real-world production.
- **Two Sessions on Security:** The first focused on cross-account resource sharing within an AWS Organization, stressing strict permission boundaries so no user exceeds their intended scope. The second tackled shifting infrastructure security left: scanning Infrastructure-as-Code (IaC) right at the pull request stage and deploying an adversarial user-persona agent to probe staging environments before going live. Both shared the same core takeaway: the earlier you catch a risk, the cheaper it is to fix.
- **Taking Agents to Production:** The session on **Amazon Bedrock AgentCore** walked through running agents in production environments and managing agent memory—separating short-term and long-term memory so the agent retains relevant user context.
- **Kiro Crew and the Closing Q&A:** This was my favorite session, focusing on running continuous AI Agents to assist with code reviews and system checks. The closing Q&A was the most practical part of the morning: the speakers shared how they handle full context windows, track token consumption per component, prune stale memory to keep costs from ballooning, and set up guardrails so AI never accidentally deletes infrastructure or leaks sensitive data.

---

### 5 Habits I Want to Change After That Morning

1. **The more autonomous the agent, the stronger the guardrails:** Across every talk—from IAM permissions to guardrails—serious practitioners always ask *"What if it goes wrong?"* before asking *"What can it do?"* For my own small coding-assistant project, I will audit all access keys, ensure zero secrets sit inside the repository, and restrict tool permissions to the bare minimum (least privilege).
2. **Context and memory are expensive resources:** I used to paste massive chunks of data into AI prompts thinking more was always better. Hearing the Q&A made me realize bloated context both burns money and easily derails the model. Moving forward, I will break tasks into smaller pieces, open fresh sessions per task, and feed in only what is strictly relevant.
3. **Move security checks to the start of the workflow:** Catching flaws during code review made me rethink my habit of getting a project working first and worrying about security later. I want to build the habit of reviewing infrastructure configurations and permission lists before every single deployment—even for test builds.
4. **Learn from people building in production:** Listening to engineers share real-world bottlenecks showed me how wide the gap is between classroom assignments and production systems. I plan to pick one topic from the meetup—such as building a lightweight agent with proper logging—and write down what I learn along the way.
5. **Be more proactive about networking:** I am still a bit hesitant to strike up conversations with strangers, but this morning reminded me that a community only brings value when you actually step into it. Next time I attend, my goal is to ask at least one question during Q&A and introduce myself to two new people.

---

### Reflection & Next Steps

I left Bitexco with my head slightly overloaded, in the best possible way. What stuck with me wasn't a specific tool, but an engineering mindset: getting AI to work is only step one; keeping it secure, cost-effective, and under control is the real job.

This week, I am starting with two concrete actions: auditing permissions and secrets in my current project, and practicing leaner context management whenever I work with AI. If you are just starting your cloud journey too, try attending a meetup like this—those three hours on a Saturday morning were worth far more than I expected.
