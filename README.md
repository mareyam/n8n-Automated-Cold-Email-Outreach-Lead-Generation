# n8n-Automated-Cold-Email-Outreach-Lead-Generation
Automated lead scraping & email campaign system built in n8n for:

Google Maps business lead extraction (Apify)
Email validation & deduplication
AI-powered cold email generation
Automated Gmail sending
Real-time sheet updates


Features
1. Lead Generation (Daily 9 AM)

Reads search terms from control sheet
Calls Apify API to scrape Google Maps (Sydney builders)
Extracts: name, email, phone, address, rating, website
Deduplicates against existing emails
Limits to 33 new leads max per run
Writes to main sheet with "UNSENT" status


2. Email Validation & Sending (Daily 10 AM)

Reads all "UNSENT" emails from sheet
Validates email format (regex + edge cases)
Generates personalized cold emails via OpenAI
Sends via Gmail with AI subject lines
Updates sheet: marks "SENT", logs validation status


3. Smart Deduplication

Compares new leads against existing sheet data
Case-insensitive email matching
Prevents duplicate outreach
Extracts nested email arrays from Apify response


Workflow Architecture
Lead Scraping Flow
Schedule 9 AM
    ↓
Get Search Terms
    ↓
Call Apify API (Google Maps)
    ↓
Flatten & Parse Leads
    ↓
Dedupe Against Existing
    ↓
Batch Limit (33 max)
    ↓
Write to Sheet

Email Sending Flow
Schedule 10 AM
    ↓
Read UNSENT Emails
    ↓
Batch Process (1 at a time)
    ↓
Validate Email Format
    ↓
AI Generate Cold Email
    ↓
Parse JSON Response
    ↓
Send Gmail
    ↓
Update Sheet Status

Tech Stack

n8n
JavaScript/Node.js
Google Sheets API
Apify API (Google Maps scraper)
Gmail API
OpenAI (gpt-3.5-turbo)


Key Nodes
NodePurposeRead All Rows1Fetch UNSENT emailsValidate Email1Regex validationAI AgentGenerate email copyCode in JavaScriptJSON parsingLoop Over ItemsBatch email sendingSend Email1Gmail deliveryCall ApifyLead scrapingDedupe & FilterRemove duplicates

Email Generation
Uses OpenAI with system prompt to:

Personalize using business name/rating
Write natural, conversational tone
Keep under 120 words
Focus on tool hire + skilled crews (ACE Solutions)
End with soft CTA (question-based)

Returns strict JSON:
json{
  "subject": "...",
  "email": "..."
}

Scheduling
WorkflowTriggerFrequencyLead ScrapingDaily 9 AMOnce per dayEmail CampaignDaily 10 AMOnce per day

Data Flow
Input: Google Sheets (UNSENT emails)
↓
Processing: Email validation + AI generation
↓
Output: Gmail sent + Sheet updated with status

Error Handling

onError: continueRegularOutput on API calls
Retry logic on Apify (5 attempts, 5s delay)
Email validation prevents invalid sends
Batch processing prevents rate limits


Future Improvements

A/B test subject lines
Track open/click rates
AI response detection
Webhook integrations
Slack notifications
Multi-region scraping

