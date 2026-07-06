# User Acceptance Testing (UAT) Plan Sample

This document shows how I coordinate UAT (User Acceptance Testing) sessions with clinical and admin staff to verify software features before they go live.

---

# UAT Plan: OmniIngest v1.0 Ingestion and Compliance Gate

| Test Info | Details |
|---|---|
| **Author** | Nisar Ahmed (Healthcare Business Analyst) |
| **Test Environment** | UAT-Stage-HMS-01 |
| **Participants** | 2 Reception Clerks, 1 Billing Officer, 1 Compliance Lead |

---

## Test Cases

### Test Case 1: Processing a Valid OPD File Ingress
* **Objective:** Verify that a standard OPD data file containing complete patient details and active consent is loaded correctly.
* **Pre-conditions:** Test database is clean. The input CSV file has valid 14-digit ABHA numbers and active consent flags.

| Step | Action | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 1 | Upload `valid_opd_data.csv` through the Ingress portal. | File upload completes without errors. | Pass | |
| 2 | Check the Ingress Dashboard. | Status shows "Processed: 10, Failed: 0". | Pass | |
| 3 | Query the database demographics table. | New patient records are visible with masked name and ABHA fields. | Pass | |

---

### Test Case 2: ABHA ID Typo Detection and Triage Routing
* **Objective:** Verify that a record with a malformed ABHA ID (e.g., 12 digits instead of 14) is blocked from the main database and sent to the Identity Desk.
* **Pre-conditions:** The input file contains one record with ABHA `1234567890`.

| Step | Action | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 1 | Upload `malformed_abha_data.csv`. | Upload completes, but status shows "Triage Required: 1". | Pass | |
| 2 | Open the Identity Desk triage screen. | The malformed record is visible in the queue with error code `ERR_ABHA_INVALID`. | Pass | |
| 3 | Click "Edit", enter a correct 14-digit ABHA, and click "Submit". | Record disappears from the triage queue and is written to the database. | Pass | Tested by reception clerk. |

---

### Test Case 3: Permanent Data Purge on Consent Revocation
* **Objective:** Verify that a patient's records are completely erased when consent is revoked, and that a log is written to the audit database.
* **Pre-conditions:** Patient record exists in the system with active consent.

| Step | Action | Expected Result | Pass/Fail | Notes |
|---|---|---|---|---|
| 1 | Open the Compliance Dashboard and find Patient ID `PT-8809`. | Patient details and active consent status are visible. | Pass | |
| 2 | Click the "Revoke Consent & Purge" Kill-Switch. | Confirmation dialog appears. | Pass | |
| 3 | Click "Confirm Purge". | Status updates to "Purged", details are redacted. | Pass | |
| 4 | Run database query `SELECT * FROM patients WHERE id='PT-8809'`. | The query returns 0 rows (hard delete verified). | Pass | Verified by database audit. |
| 5 | Check the Governance log table. | A log entry showing the timestamp and ID deletion event is present. | Pass | |

---

## UAT Sign-Off Criteria

The release will be signed off for production deployment only if:
1. **100% of Critical and High priority test cases pass** (including ABHA validation, triage routing, and consent purging).
2. **Zero data corruption occurs** during the triage editing process.
3. The compliance officer confirms that no unmasked patient data is exposed on default views.
