# API Contract (Minimal)

No auth.

## POST /api/conversations
Creates a new conversation.
Response:
{
  "conversationId": "uuid"
}

## POST /api/conversations/:id/messages
Send a user message and get the assistant reply + updated state.

Request:
{
  "message": "string"
}

Response:
{
  "assistantMessage": "string",
  "stage": "ASK_EMAIL | ASK_COMPANY | ASK_NEEDS | ASK_COMPANY_SIZE | ASK_BUDGET_TIMELINE | CLASSIFY_DONE",
  "leadProfile": { ... },
  "done": true/false,
  "result": {
    "email": "string",
    "companyName": "string",
    "relevance": "WEAK | HOT | NOT_RELEVANT | VERY_BIG",
    "calendlyUrl": "string | null"
  }
}
