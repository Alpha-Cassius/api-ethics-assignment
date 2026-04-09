# Problem Statement
You are a junior data analyst at a healthcare startup. Your team has built a Python script that calls a public health statistics API to collect patient-level records. The raw response data includes fields such as full name, email, date of birth, zip code, job title, and diagnosis notes. Your manager has asked you to clean and prepare this dataset responsibly before it is shared with an external research partner.

---
## Task 1 — Classify and Handle PII Fields
The dataset contains the following fields:

full_name, email, date_of_birth, zip_code, job_title, diagnosis_notes

Classify each field as either Direct PII or Indirect PII.
For each field, state whether you would drop it, mask it, or pseudonymize it before sharing, and briefly justify your choice.

---
## Task 2 — Audit the API Script for Ethical Compliance
Your team's data collection script is shown below:

'''
import requests

API_URL = "https://healthstats-api.example.com/records"
API_KEY = "free_tier_key_abc123"

records = []
for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    data = response.json()
    records.extend(data["results"])
'''

# Store all records permanently in company database
save_to_database(records)
Identify two ethical or TOS violations present in this script. For each violation, explain what the problem is and suggest a corrected version of the relevant code.

