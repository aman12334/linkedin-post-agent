# AI Agent: LinkedIn Post Generator (Free Setup with Make.com)

This project automates the process of fetching AI-related news, generating LinkedIn posts, and logging them to Google Sheets.
It is built on Make.com using free-tier APIs for both data and AI generation.

## Overview

The agent performs the following steps:
1. Fetches recent AI news from the NewsData.io API.
2. Iterates through the top 4 articles.
3. Sends the content to the Mistral model on OpenRouter to generate a LinkedIn-style post.
4. Logs each generated post to Google Sheets.
5. Sends an email with the post content using Gmail.
6. Uses error handling so the scenario continues even if one call fails.

This workflow requires no paid APIs or servers.

## Components and Tools

| Component | Description | Cost |
|------------|-------------|------|
| Make.com | Workflow automation platform | Free (1,000 ops/month) |
| NewsData.io | News API | Free |
| OpenRouter (Mistral) | AI text generation | Free tier available |
| Google Sheets | Data storage | Free |
| Gmail | Notifications | Free |

## Required API Keys

### 1. NewsData.io API Key
- Go to https://newsdata.io/register
- Sign up and retrieve your API key from the dashboard.
- In Make.com, use it in the query string parameter `apikey`.

### 2. OpenRouter (Mistral) API Key
- Go to https://openrouter.ai/settings/keys
- Click "Create API Key".
- Copy the key (starts with `sk-or-`).
- Use it in the Make.com HTTP module header:
  ```
  Authorization: Bearer sk-or-yourkey
  Content-Type: application/json
  HTTP-Referer: https://www.make.com
  ```

## Step-by-Step Setup in Make.com

1. Trigger: Scheduler → Run Once (for testing). Later, set it to run daily.
2. Fetch News: HTTP GET https://newsdata.io/api/1/news
3. Iterator: Tools → Iterator (array: results[])
4. Filter: Bundle position ≤ 4
5. OpenRouter API Call: HTTP POST to https://openrouter.ai/api/v1/chat/completions with headers and body as shown above.
6. Error Handler: Resume (Status Code: 200, Data: custom JSON fallback).
7. Google Sheets: Add Row (Title, Post, Status, Error Message, Status Code, Link, Timestamp).
8. Gmail: Send Email (post summary to your email).

## Example Google Sheet Output

| Title | Post | Status | Error Message | Status Code | Link | Timestamp |
|--------|------|--------|----------------|--------------|------|------------|
| AI in Supply Chains | LinkedIn post text... | Success |  | 200 | https://docs.google.com/spreadsheets/d/1a0fVggR1sB5nwX9wBaoEdrZwehbhSlnk8YbvFrG2NyI/edit?usp=sharing | 2025-11-03 22:40 |

## Notes

- Processes up to 4 posts per run.
- Add a deduplication filter to skip repeated articles.
- Error handler ensures failed calls do not stop the entire scenario.
- Extendable to Trello or Jira with free-tier APIs.

## Repository Structure

README.md
agent_workflow.json
notes.txt
MyMovie1.mp4
screenshots/

## Author

Aman Tiwari  
Product Manager | Building agentic workflows with zero budget automation  
LinkedIn: linkedin.com/in/yourprofile  
GitHub: github.com/yourusername
