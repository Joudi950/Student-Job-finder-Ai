# Student Job Finder AI Agent

A n8n workflow that helps students find part-time jobs or internships based on their schedule, skills, and location. It uses Groq (free LLM) to filter and rank the results.

I built this because most job boards don't account for student schedules — you either get full-time offers or completely irrelevant results. This tries to fix that.

---

## How it works

The workflow receives a POST request with the student's profile, searches two job APIs in parallel (Adzuna and Remotive), merges the results, then sends everything to Groq to filter and return only the relevant opportunities.

Here's the flow step by step:

1. Webhook receives the student's input
2. Input is validated (schedule, field of study, location are required)
3. Adzuna and Remotive are queried simultaneously
4. Results are merged and normalized
5. Groq analyzes the jobs and filters based on the student's profile
6. A clean JSON response is returned

---

## Input parameters

| Field | Required | Description |
|-------|----------|-------------|
| `schedule` | yes | Weekly schedule (e.g. "Monday 8am-12pm, Wednesday free") |
| `study_field` | yes | Field of study (e.g. "Computer Science") |
| `location` | yes | City and country (e.g. "Tunis, Tunisia") |
| `skills` | no | Skills separated by commas (e.g. "Python, Excel") |
| `availability_hours_per_week` | no | Hours available per week (default: 15) |
| `preferred_job_type` | no | remote, hybrid, or on-site |
| `monthly_budget_needed` | no | Target monthly income (default: 500) |
| `language` | no | Response language: `fr` or `en` (default: `fr`) |

Example request:

```json
{
  "schedule": "Monday 8am-12pm, Tuesday 8am-6pm, Wednesday free, Thursday 10am-4pm",
  "study_field": "Computer Science",
  "location": "Tunis, Tunisia",
  "skills": "Python, Excel, Communication",
  "availability_hours_per_week": 20,
  "preferred_job_type": "remote or hybrid",
  "monthly_budget_needed": 600,
  "language": "en"
}
```

---

## Stack

- [n8n](https://n8n.io/) — workflow automation
- [Groq](https://groq.com/) — free LLM API for filtering and analysis
- [Adzuna API](https://developer.adzuna.com/) — job listings aggregator
- [Remotive API](https://remotive.com/api/readme) — remote job listings

---

## Setup

### Requirements

- A running n8n instance (self-hosted or cloud)
- A free Groq API key from [console.groq.com](https://console.groq.com)
- Free Adzuna API credentials from [developer.adzuna.com](https://developer.adzuna.com)

### Steps

1. Clone the repo:

```bash
git clone https://github.com/Joudi950/Student-Job-finder-Ai.git
cd Student-Job-finder-Ai
```

2. In n8n, go to **Workflows > Import** and select `Student_Job_Finder_AI_Agent_v2.json`

3. Update the credentials:
   - In the **Adzuna Job Search API** node: replace `app_id` and `app_key` with your own
   - In the **Groq AI** node: add your Groq API key

4. Activate the workflow and copy the webhook URL

5. Test it:

```bash
curl -X POST https://your-n8n-instance.com/webhook/student-job-finder \
  -H "Content-Type: application/json" \
  -d '{"schedule": "Monday 8am-12pm", "study_field": "Computer Science", "location": "Tunis, Tunisia"}'
```

---

## Repository structure

```
Student-Job-finder-Ai/
├── Student_Job_Finder_AI_Agent_v2.json   # n8n workflow to import
├── derniere.html                          # Frontend interface
└── README.md
```

---

## Notes

- If no input is provided, the workflow falls back to a demo profile for testing
- The Adzuna search is currently set to French job listings — you can change the country code in the node parameters
- Groq's free tier is more than enough for this use case

---

## License

MIT
