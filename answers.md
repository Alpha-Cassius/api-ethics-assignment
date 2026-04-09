# API Ethics and Data Privacy Assignment

## Task 1 — Classify and Handle PII Fields

When sharing healthcare data with external partners, it is critical to follow the principle of **Data Minimization**—only providing the data necessary for the research while protecting patient identity.

| Field | Classification | Action | Justification |
| :--- | :--- | :--- | :--- |
| **full_name** | Direct PII | **Pseudonymize** | Replace with a unique `patient_id`. This allows researchers to track the same patient over time without knowing their identity. |
| **email** | Direct PII | **Drop** | Email addresses are high-risk identifiers and are rarely necessary for health statistics research. |
| **date_of_birth** | Indirect PII | **Mask** | Specific dates can be used for "linkage attacks." I would convert this to **Year of Birth** or **Age Range** to preserve privacy. |
| **zip_code** | Indirect PII | **Mask** | In small towns, a ZIP code + Job Title can identify a person. I would truncate this to the **first 3 digits** (ZIP3). |
| **job_title** | Indirect PII | **Generalize** | Specific titles (e.g., "Mayor of Springfield") are identifying. Map these to broad categories (e.g., "Government," "Education"). |
| **diagnosis_notes** | Indirect PII | **Mask/Redact** | Notes often contain names or dates. These must be scrubbed using Named Entity Recognition (NER) before sharing. |

---

## Task 2 — Audit the API Script for Ethical Compliance

### Violation 1: Hardcoded API Credentials
**Problem:** The `API_KEY` is hardcoded directly into the source code. This is a security risk; if this script is pushed to a public repository, the key is exposed, potentially leading to unauthorized access and billing issues.

**Corrected Code:**
```python
import os
from dotenv import load_dotenv

# Load key from an environment variable for security
load_dotenv()
API_KEY = os.getenv("HEALTH_STATS_API_KEY")
