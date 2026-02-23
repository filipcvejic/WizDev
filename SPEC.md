# Wiz Trial Task — Conversation Flow Spec (KanhaSoft AI Representative)

## Goal
Implement an AI representative that chats with new leads, collects key details, and classifies lead relevance.
Must store in DB for each conversation:
- Email
- Company name
- Lead relevance (weak lead / hot lead / not relevant / very big potential customer)

If lead is relevant (weak/hot/very big), send a dummy Calendly link.

---

## Data to collect (Lead Profile)
Required:
- email
- companyName

Qualification fields (used to classify relevance):
- needs (what they want to build / what help they need)
- companySize (approx. number of employees)
- budget (range or "low/none/unknown")
- timeline (urgent/weeks/months/unknown)

---

## Conversation Stages (State Machine)

### Stage: ASK_EMAIL
**Goal:** Collect lead email.
**Prompt example:**  
"To make sure I can follow up properly, what’s your email?"

Transition:
- If a valid-looking email is detected => next stage ASK_COMPANY
- Else => stay in ASK_EMAIL

Notes:
- No real email verification is required (syntax-level is enough).

---

### Stage: ASK_COMPANY
**Goal:** Collect company name.
**Prompt example:**  
"Thanks! What’s your company name?"

Transition:
- If companyName is detected => next stage ASK_NEEDS
- Else => stay in ASK_COMPANY

---

### Stage: ASK_NEEDS
**Goal:** Understand what they need help with.
**Prompt example:**  
"Great — what are you building, and where do you need help (backend, frontend, AI, infrastructure, etc.)?"

Transition:
- If needs is detected => next stage ASK_COMPANY_SIZE
- Else => stay in ASK_NEEDS

---

### Stage: ASK_COMPANY_SIZE
**Goal:** Understand company size / team size.
**Prompt example:**  
"How big is the company (approx. number of employees)?"

Transition:
- If companySize is detected => next stage ASK_BUDGET_TIMELINE
- Else => stay in ASK_COMPANY_SIZE

---

### Stage: ASK_BUDGET_TIMELINE
**Goal:** Get budget + timeline.
**Prompt example:**  
"What’s your timeline and budget range for this project?"

Transition:
- If at least one of (budget, timeline) is detected => next stage CLASSIFY_DONE
- Else => stay in ASK_BUDGET_TIMELINE

---

### Stage: CLASSIFY_DONE
**Goal:** Classify the lead and close the conversation.
Actions:
1) Determine relevance using rules below
2) Save to DB: email, companyName, relevance (minimum required)
3) If relevance != NOT_RELEVANT, send dummy Calendly link:
   - https://calendly.com/kanhasoft/intro-call

---

## Completion Criteria
Conversation can finish when:
- email + companyName are collected AND
- at least 2 of these are collected: needs, companySize, budget, timeline

(If the user provides enough info early, the bot may skip remaining questions and classify.)

---

## Lead Relevance Rules

### NOT RELEVANT
Examples / signals:
- student / learning only
- "no budget", "free help", "can you do it for free"
- personal hobby with no intent to pay

### WEAK LEAD
Examples / signals:
- side project, solo founder
- low budget or unclear budget
- timeline vague ("maybe later", "not sure yet")
- still exploring options

### HOT LEAD
Examples / signals:
- company is growing and needs developers now
- urgent timeline (days/weeks)
- budget exists / ready to pay
- clear pain point: "need to ship faster", "need to expand dev team"

### VERY BIG POTENTIAL CUSTOMER
Examples / signals:
- 1000+ employees OR clear enterprise language
- procurement/security review/SLA/vendor onboarding
- multiple teams, large scale product

---

## Bot Tone Guidelines
- Friendly, concise, professional
- Ask one question at a time
- Avoid long paragraphs
- If lead is relevant: encourage scheduling via Calendly link
