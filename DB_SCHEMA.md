# Database Schema (Postgres)

We store one record per conversation/lead qualification result.

## Table: conversations
Required fields (must-have from task):
- email
- companyName
- relevance

Recommended fields (simple + useful):
- stage (current flow stage)
- leadProfile (JSON with collected fields)
- transcript (JSON array of messages)
- createdAt, updatedAt

### conversations columns
- id (uuid, primary key)
- email (text, not null)
- companyName (text, not null)
- relevance (enum: WEAK | HOT | NOT_RELEVANT | VERY_BIG)
- stage (enum: ASK_EMAIL | ASK_COMPANY | ASK_NEEDS | ASK_COMPANY_SIZE | ASK_BUDGET_TIMELINE | CLASSIFY_DONE)
- leadProfile (jsonb, nullable)
- transcript (jsonb, nullable)
- calendlyUrl (text, nullable)
- createdAt (timestamp)
- updatedAt (timestamp)
