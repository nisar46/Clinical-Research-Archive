# Jira Agile Backlog Sample

This document shows how I write Jira User Stories and Epics to guide developers. These examples are based on the requirements I defined for the **OmniIngest** and **Clinosyn** projects.

---

## Epic: ABDM 2.0 Ingestion Pipeline (OmniIngest)
**Epic Key:** OMNI-1  
**Description:** As a healthcare clinic, we need a compliant data ingestion pipeline that normalizes unstructured clinical data, validates patient identities (ABHA IDs), and manages patient consent according to DPDP Act regulations.

### User Story 1: Patient Identity Validation (ABHA ID)
**Story Key:** OMNI-12  
**Story Description:**  
> **As an** Admission Clerk,  
> **I want** the system to automatically validate that patient ABHA IDs are 14 digits long,  
> **So that** we prevent typos and duplicate records from being written to our main database.

**Acceptance Criteria (Given-When-Then Format):**
* **Scenario: Valid ABHA ID entered**
  * **Given** a patient record is received at the Ingress gate,
  * **And** the ABHA ID is a 14-digit numeric string (e.g., `12345678901234`),
  * **When** the validation check runs,
  * **Then** the record should be marked as `VALIDATED` and allowed to move to the Dispatch queue.
* **Scenario: Malformed or missing ABHA ID**
  * **Given** a patient record is received with an ABHA ID that is not 14 digits (or is missing),
  * **When** the validation check runs,
  * **Then** the system must block the record from moving to the main database,
  * **And** flag the record with error code `ERR_ABHA_INVALID`,
  * **And** route the record to the clerk's Identity Desk triage queue for manual correction.

**Story Points:** 3  
**Priority:** High  
**Labels:** Compliance, Identity, Ingestion

---

### User Story 2: DPDP Act Consent Purge (Rule 8.3)
**Story Key:** OMNI-25  
**Story Description:**  
> **As a** Compliance Officer,  
> **I want** patient records to be permanently deleted if their consent status is updated to revoked,  
> **So that** we comply with the storage limitation guidelines of the India DPDP Act 2023.

**Acceptance Criteria:**
* **Scenario: Consent Status set to Revoked**
  * **Given** a patient record exists in our production tables with consent status `ACTIVE`,
  * **When** the patient submits a consent revocation request and the status changes to `REVOKED`,
  * **Then** the compliance engine must execute a hard-delete against all database rows linked to this patient ID,
  * **And** generate a timestamped audit entry in the Governance table indicating: `Patient ID [X] - Data Purged - Consent Revoked`,
  * **And** verify that no downstream queries can retrieve this patient's medical records.

**Story Points:** 5  
**Priority:** Critical  
**Labels:** Compliance, Security, Data-Governance

---

## Epic: Offline clinical AI Query Terminal (Clinosyn)
**Epic Key:** CLINO-1  
**Description:** As a clinical doctor, I need a secure, local query interface that answers questions about patient histories in plain language without sending patient data to cloud services.

### User Story 3: PII Masking on Clinical Dashboard
**Story Key:** CLINO-5  
**Story Description:**  
> **As a** Doctor searching patient records,  
> **I want** sensitive personal information (like names and contact details) to be masked on screen by default,  
> **So that** we prevent accidental data exposure to unauthorized onlookers in the ward.

**Acceptance Criteria:**
* **Scenario: Search results loaded**
  * **Given** a doctor runs a query on patient histories,
  * **When** the results are displayed on the clinical terminal screen,
  * **Then** the `Patient Name` field must display as masked (e.g., `N**** A****`),
  * **And** the `ABHA ID` must display as masked (e.g., `XXXX-XXXX-1234`),
  * **And** a "Reveal Details" button must be visible next to the record.
* **Scenario: Authorized reveal requested**
  * **Given** masked search results are visible,
  * **When** the doctor clicks the "Reveal Details" button,
  * **Then** the system must prompt for authentication,
  * **And** upon successful auth, display the unmasked PII.

**Story Points:** 3  
**Priority:** Medium  
**Labels:** UI, Privacy, Security
