# Confluence Product Requirement Document (PRD) Template

This document shows how I structure PRDs in Confluence to define system behavior and technical boundaries for engineering teams.

---

# PRD: ABDM 2.0 Ingestion Pipeline (OmniIngest)

| Document Info | Details |
|---|---|
| **Author** | Nisar Ahmed (Healthcare Business Analyst) |
| **Status** | Approved |
| **Target Release** | v1.0 |
| **Date** | July 2026 |

---

## 1. Document History & Approvals

| Version | Date | Description | Author | Approver |
|---|---|---|---|---|
| v1.0 | 06-Jul-2026 | Initial Release with Ingest & Purge rules | Nisar Ahmed | Clinical Director |

---

## 2. Objective & Scope
The goal of OmniIngest is to prevent manual data entry errors and compliance gaps at patient registration. Currently, 30% of records are entered incorrectly under peak loads. 

This pipeline acts as an airlock: it validates data in temporary memory, routes malformed records to triage queues, and writes only clean, DPDP-compliant data to our database.

---

## 3. High-Level Requirements

| Req ID | Title | Description | Priority | Mapped User Story |
|---|---|---|---|---|
| **FR-01** | Multi-format Adapter | System must ingest files in CSV, HL7, and JSON formats without crashing. | High | OMNI-10 |
| **FR-02** | ABHA Validation | Every record must have a 14-digit ABHA ID checked against regex. | High | OMNI-12 |
| **FR-03** | Triage Routing | Records with missing IDs must go to a manual correction queue. | High | OMNI-14 |
| **FR-04** | DPDP Compliance | If consent is revoked, records must be permanently purged. | Critical | OMNI-25 |

---

## 4. Workflows & Process Maps

```
[Incoming File]
      │
      ▼
┌──────────────┐
│ Intake Gate  │ ──► Load to memory
└──────────────┘
      │
      ▼
┌──────────────┐          Fail ABHA check
│ Triage Engine│ ──────────────────────────┐
└──────────────┘                           │
      │                                    ▼
      │ Pass check                 ┌───────────────┐
      ▼                            │ Identity Desk │ ──► Manual fix
┌──────────────┐                   └───────────────┘
│ Dispatcher   │                           │
└──────────────┘                           │
      │                                    ▼
      │                               Re-validate
      ▼
┌──────────────┐
│ SQLite DB    │
└──────────────┘
```

---

## 5. Security & Legal Guardrails

### 5.1 Data Privacy (DPDP Act Compliance)
* **PII Redaction:** Names, contact details, and ABHA IDs must be masked by default when loaded onto UI screens.
* **Consent Logs:** Every patient record must have an active consent flag. The system must record consent dates in the `Governance` audit log.

### 5.2 Key-Shredding Purge Logic
When a delete command is issued for a patient who has revoked consent:
- The system must identify all rows linked to that patient ID.
- Execute a `DELETE` command (hard-delete).
- Overwrite the memory space to ensure the records are unrecoverable.

---

## 6. Open Decisions & Questions

| Decision | Impact | Status | Owner |
|---|---|---|---|
| Should we allow partial admission if the ABHA ID is missing? | If yes, we risk running out of compliance. If no, we block patient care. Decided to allow 24-hour quarantine before billing hold. | Resolved | Clinical Director |
