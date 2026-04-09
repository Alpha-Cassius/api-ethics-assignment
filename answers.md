API Ethics and Data Privacy AssignmentTask 1 — Classify and Handle PII FieldsAs a healthcare data analyst, protecting patient confidentiality is a legal and ethical mandate. Below is the strategy for handling the dataset before sharing it with external research partners.FieldClassificationActionJustificationfull_nameDirect PIIPseudonymizeReplaced with a unique UUID. This allows for data linkage across different sets without revealing the actual identity of the individual.emailDirect PIIDropEmails are high-risk identifiers that offer no statistical value for public health research. Removing them prevents unauthorized contact or phishing.date_of_birthIndirect PIIMask / GeneralizeSpecific birthdates can be linked to public records to identify individuals. I would convert these to Age Ranges or Year of Birth.zip_codeIndirect PIIMask / TruncateFull zip codes can geolocate patients in sparsely populated areas. I would truncate this to the first 3 digits (ZIP3) to provide region-level data only.job_titleIndirect PIIGeneralizeNiche job titles can identify a specific person. I would map these to broad SIC (Standard Industrial Classification) codes or general sectors (e.g., "Healthcare").diagnosis_notesIndirect PII (PHI)Sanitize / RedactFree-text notes often contain patient names or family mentions. I would use a Named Entity Recognition (NER) tool to scrub these before export.Task 2 — Audit the API Script for Ethical ComplianceBelow is an audit of the current data collection script, identifying violations of security best practices and API Terms of Service (TOS).Violation 1: Exposure of Secret CredentialsProblem: The API_KEY is hardcoded directly into the script. This violates security protocols because if this code is committed to a repository (like GitHub), the key becomes public. An attacker could use the key to steal data or incur costs on the company's account.Corrected Code:import os
from dotenv import load_dotenv

# Load sensitive keys from an environment variable file (.env)
load_dotenv()
API_KEY = os.getenv("HEALTH_STATS_API_KEY")

# Now use the variable in the request
# response = requests.get(API_URL, params={"page": page, "key": API_KEY})
Violation 2: Lack of Rate Limiting and "Privacy by Design"Problem: The script attempts to pull 100 pages in a rapid-fire loop. This is considered "aggressive scraping" and could trigger a server ban or be viewed as a Denial of Service (DoS) attempt. Furthermore, the script saves raw PII to the database before any cleaning, increasing the risk of a data breach.Corrected Code:import time
import requests

records = []
for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    
    if response.status_code == 200:
        data = response.json()
        records.extend(data["results"])
    
    # Respect the API provider by adding a 1-second delay between calls
    time.sleep(1)

# Apply PII cleaning logic BEFORE saving to the permanent database
cleaned_records = sanitize_patient_data(records)
save_to_database(cleaned_records)
ConclusionBy implementing these changes, we ensure compliance with HIPAA/GDPR principles, respect the infrastructure of our data providers, and maintain the trust of the patients whose data we analyze.
