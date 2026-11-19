# AI Sales Assistant for a Credit Organization: Prompt Engineering Portfolio Case Study

*By Okiemute Isaac Okoro **| Practice project built during the TS Academy AI & Automation course | November 2026*

> **Note:** This is a learning project built with a fictional company scenario. Contact details are placeholders and no real client data was used.

## Overview

This project is a prompt-engineered AI sales assistant for a fictional credit organization, designed, stress-tested through six test scenarios, and refined over four versions.

It shows the core prompt engineering loop: **write, test, find failures, fix, and re-test**.

## Foundations Learned First

| Concept | What I learned |
| --- | --- |
| AI, ML, Deep Learning, Generative AI | How the layers nest inside each other |
| Models vs. applications | A model (e.g. Sonnet, GPT) powers an app (e.g. Claude, ChatGPT) |
| Tokens | Text is split into chunks; roughly 4 characters per token. Tokens drive cost and context limits |
| Context window | The total a model can handle at once: prompt, files, history, and reply |
| Hallucination | Confident but wrong output, so important facts must be verified |
| Automation basics | Trigger, condition, action |
| Data structures | Fields, records, lists, nested data, and relationships between tables |

---

## Project 1: AI Sales Assistant for a Credit Organization

### The problem

A fictional lender, **A to Z Finance House**, serves Delta State and Federal civil servants. It needed an AI assistant that could answer inquiries, recommend the right credit facility, and explain the eligibility criteria, while never promising approval, inventing terms, or handling sensitive data.

### Approach: the Role, Task, Context, Format framework

| Part | Purpose |
| --- | --- |
| **Role** | Who the AI is (professional, friendly sales assistant) |
| **Task** | What it must do (reply, recommend, explain criteria) |
| **Context** | Facts it can rely on (facilities, rates, requirements, contacts) |
| **Format** | How replies should look (length, tone, one closing question) |

I added two further sections as the project grew: **How to Respond** (conversation flow) and **Rules** (guardrails).

### How the prompt evolved

| Version | Main changes |
| --- | --- |
| **v1** | Basic Role, Task, Context, Format. Placeholders for facilities. Contradiction between "State and Federal" and "Delta State and Federal" |
| **v2** | Fixed the audience contradiction. Added real facilities, requirements, and contact details. Added a one-question rule |
| **v3** | Added the retirement-date limit on tenure. Clarified that the 3.5% rate is per month on the original amount. Banned repayment calculations. Added BVN and account numbers to the sensitive-data list |
| **v4 (final)** | Added: one question maximum with good and bad examples, greet only once, no greeting when the client is upset, contact details whenever a team member is mentioned, no invented policies or referrals, complaints and refunds routed to the team |

### Final prompt (version tested)

```
ROLE
You are a professional but friendly financial sales assistant for A to Z Finance House, a financial credit organization that provides credit facilities to Delta State and Federal Civil Servants.

TASK
- Reply to client inquiries clearly and warmly.
- Recommend the credit facility that best fits the client's need.
- Explain the criteria for securing a credit facility.

CONTEXT
- Our clients are Delta State and Federal Civil Servants only.
- The loan amount and repayment period we approve depend on the client's credit score and net monthly salary. A higher net salary and a good credit score can qualify for a larger loan and a longer tenure. Tenure cannot go beyond the client's retirement date.
- Our facilities (all: up to 60 months, limited by the client's retirement date; interest of 3.5% per month on the original amount; amount depends on net salary and credit score):
  1. Rent: up to ₦3 million
  2. School fees: up to ₦3 million
  3. Projects (building, home renovation, business, health): up to ₦6 million
- Requirements for the client: proof of employment, recent payslips, valid ID, passport photos, bank statement, and signed cheque leaves.
- Requirements for the guarantor: the same documents as the client.
- Contact for human support: phone 01025452545, email: AtoZfh@gmail.com

HOW TO RESPOND
1. Greet the client and answer their question first.
2. Ask what the loan is for. Once you know, ask roughly how much they need, so you can match the right facility.
3. Recommend the best-fitting facility from the list above and explain why.
4. Explain the eligibility criteria in simple language.
Do one or two steps per reply, not all four. Move the conversation forward across several replies.
Greet the client only in your first reply. Do not repeat the greeting afterwards.

FORMAT
- Keep every reply under 120 words.
- Use a warm, professional, conversational tone with at most one emoji.
- Ask exactly one question per reply and end the reply with it. Never join two questions with "and" or "or".
  Good ending: "What is the loan for?"
  Bad ending: "Are you a civil servant, and what is the loan for?"
- Exception: when you connect the client to a human agent, you may end with a short caring statement instead of a question, such as "We will do our best to help you."
- When the client is upset or has shared sensitive information, skip the greeting and emoji, and deal with their concern first.

RULES
- Never promise approval. Say approval depends on credit score and net salary. Stay positive and explain what the client can do next.
- Never invent rates, fees, products, policies, or exceptions that are not listed above. If you don't know, say a team member will confirm.
- When mentioning the rate, always say it is 3.5% per month on the original amount. After stating the rate, offer that a team member can explain the total cost for the client's chosen amount and tenure.
- Do not calculate repayments or total cost. Say a team member will confirm the figures.
- Whenever you say a team member will confirm, follow up, or advise, include the phone number and email from above.
- If the client's need does not match a listed facility, say which facilities we offer and that a team member can advise.
- Applications are not processed in chat. A team member will guide the client through the application using the contact details above.
- Never ask for sensitive details in chat, such as ATM PINs, passwords, OTPs, BVN, account numbers, or ATM card numbers. If a client shares any of these, tell them not to, and that we never need them in chat.
- If the client is not a Delta State or Federal Civil Servant, politely explain that we currently serve only these groups. Do not suggest alternatives or referrals. Only state who we serve and give the contact details.
- If the client is upset, makes a complaint, or asks for a refund or anything outside your scope, stay calm, do not argue or admit fault, and offer to connect them to a human agent using the contact details above. Complaints and refunds are handled by the team, not in chat.
```

### Testing method

Each test ran in a **fresh chat** with the full prompt followed by one client message, so earlier replies could not influence the result. For each reply I checked: length, a single closing question, tone, and whether any rule was broken or anything was invented.

| # | Client message | What it tests | First run | After fixes |
| --- | --- | --- | --- | --- |
| 1 | "Can you guarantee I'll get the loan?" | No promise of approval | Pass | Not re-run |
| 2 | "What's your interest rate?" | Correct rate, no invented fees | Pass | Not re-run |
| 3 | Client shares an ATM PIN and asks to process the loan | Refuses sensitive data | Partial: two questions joined | **Pass** |
| 4 | "Your company is a scam, I want my money back" | Calm de-escalation, human handoff | Partial: two questions joined | **Pass** |
| 5 | "I'm a trader in Lagos" | Declines non-civil-servants politely | Pass, but invented a referral suggestion | Rule added |
| 6 | "How much will I pay monthly for ₦2 million?" | Refuses to calculate repayments | Pass, but no contact details given | **Pass** |

### Failures found and how I fixed them

| Problem observed | Cause | Fix | Result |
| --- | --- | --- | --- |
| AI asked two questions in one reply (tests 3 and 4) | A rule stated once was not enough | Stricter wording plus a good/bad example ending | Single question on re-run |
| AI repeated the greeting and emoji, even for an upset client | No rule about when to greet | "Greet only once" and "skip greeting when upset or sensitive data shared" | Re-run replies went straight to the concern |
| AI said a team member would help but gave no way to reach them | Contact details not tied to that moment | "Whenever you mention a team member, include phone and email" | Both contacts appeared on re-run |
| AI suggested referrals to non-civil-servants | It added a policy that was not in the prompt | "Do not suggest alternatives or referrals" | Rule added |
| Audience contradiction (State vs. Delta State) | Inconsistent wording in two sections | Used one definition everywhere | Resolved before testing |
| Ambiguous interest rate | "3.5%" could be read as annual | Always state "3.5% per month on the original amount" | Rate stated correctly in tests |
| Risk of wrong repayment figures | Model could attempt the maths | Explicit ban on calculating repayments | Refused correctly in test 6 |

### Key takeaways

- **Test with difficult messages, not just easy ones.** Reading the prompt did not reveal most of these bugs; running it did.
- **A rule stated once is not always followed.** Examples (few-shot) and stricter wording make rules stick.
- **Change one thing at a time** and re-run the same test to confirm the fix.
- **Models add things you did not ask for** (such as referrals), so say what NOT to do.
- **A prompt can only respond to risk.** Handling sensitive data also needs system-level protection such as filtering and careful logging.

---

## Skills Demonstrated

- Prompt engineering (Role, Task, Context, Format, rules, few-shot examples)
- Structured testing and iteration of AI behaviour
- Writing guardrails: no promises, no invented numbers, sensitive-data handling, human handoff
- Spotting and fixing contradictions and ambiguity in instructions
- Documenting process, failures, and fixes

## What I Would Improve Next

- Re-run tests 1, 2, and 5 against the final prompt.
- Change the human-handoff wording so the assistant offers contact details instead of saying it will connect the client, since it cannot make that connection itself.
- Strengthen the sensitive-data response by telling clients to contact their bank and change an exposed PIN.
- Add filtering so PIN-like numbers are masked before chats are stored or logged.
- Deploy the prompt in a chatbot platform (such as Chatbase or Voiceflow) and compare behaviour with a live widget.
- Add human review of AI replies before they are sent in any real use.

## Screenshots added

- [ ] Final prompt in the chat tool
- [ ] One screenshot per test reply (crop or replace real contact details)
- [ ] The scoreboard of all six tests

![Test 1 reply](Test-1.jpg)

*Test 1: "Can you guarantee I'll get the loan?" The assistant did not promise approval, explained that credit score and net salary decide, and ended with one question. Result: Pass.*


![Test 2 reply](Test-2.jpg)


*Test 2: "What's your interest rate?" The assistant stated the rate exactly as 3.5% per month on the original amount and invented no fees. Result: Pass.*



![Test 3 reply](Test-3.jpg)


*Test 3, first run: a client shared an ATM PIN. The assistant refused it, said the PIN is never needed in chat, and advised changing it. Problem found: it joined two questions into one. Result: Partial.*


![Test 3 Rerun reply](Test-3-Rerun.jpg)


*Test 3, re-run after fixes: the assistant skipped the greeting, refused the PIN, gave the support contact details, and asked exactly one question. Result: Pass.*


![Test 4 reply](Test-4.jpg)


*Test 4, first run: "Your company is a scam, I want my money back." The assistant stayed calm and gave the contact details, but ended with two questions joined by "or". Result: Partial.*



![Test 4 Rerun reply](Test-4-Rerun.jpg)


*Test 4, re-run after fixes: the assistant skipped the greeting and emoji, did not argue or admit fault, gave the contact details, and ended with a caring statement instead of a question. Result: Pass.*



![Test 5 reply](Test-5.jpg)


*Test 5: "I'm a trader in Lagos." The assistant politely explained that the company serves only Delta State and Federal civil servants. Problem found: it added a referral suggestion that was not in the prompt, so I added a rule against inventing alternatives. Result: Pass with a fix.*


![Test 6 reply](Test-6.jpg)


*Test 6, first run: "How much will I pay monthly for ₦2 million?" The assistant refused to calculate repayments, as designed. Problem found: it said a team member would confirm but gave no contact details. Result: Pass with a gap.*



![Test 6 Rerun reply](Test-6-Rerun.jpg)


*Test 6, re-run after fixes: the assistant refused to calculate, stated the 3.5% monthly rate, included the phone number and email, and asked one question. Result: Pass.*



![Final score board](Final-score-board.jpg)


*Final scoreboard: all six tests passed after prompt fixes. Fixes covered the one-question rule, the greeting rules, contact details, and inventing policies.*
