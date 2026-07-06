# Clinical Data Analytics Journey (2020–2025)

**Role:** Clinical Data Analyst → Healthcare Business Analyst  
**Author:** Nisar Ahmed | RK Nursing Home, Bangalore Rural

---

## What This Is

This repository documents five years of hands-on clinical data work at RK Nursing Home. I was contracted to manage the hospital's data workflows, patient billing, and floor operations. Over those five years, every problem I ran into on the clinical floor became a data project.

I'm not a developer who learned healthcare. I'm a healthcare operations person who learned to work with data because the problems in front of me required it. This archive shows that journey, year by year.

---

## Year by Year

### Year 1 — Cleaning Up the Records Room (2020–2021)

**The problem:** We had thousands of paper patient records being manually entered into Excel. The formats were inconsistent — different staff members spelled names differently, used different date formats, left different fields blank. Running any report meant spending half the day cleaning the data first before you could actually analyze it.

**What I did:** Built normalization scripts in Python to standardize the records into a consistent format. Defined field-by-field rules for what valid data looked like and automated the flagging of records that didn't meet those rules.

**What I learned:** Data quality is not a technology problem — it's a workflow problem. The scripts worked, but they kept finding the same types of errors because the data entry process itself hadn't changed. That realization shaped how I thought about every project after this.

---

### Year 2 — Finding the Revenue Leaks (2021–2022)

**The problem:** The hospital's monthly billing reconciliation kept producing gaps. Money that should have been collected from insurance claims wasn't showing up. Nobody could tell exactly where it was going wrong.

**What I did:** Built audit scripts that compared the clinical records (what services were delivered) against the billing records (what was charged) and flagged the mismatches. Found three consistent error patterns: services rendered that never got billed, billing codes that didn't match the clinical diagnosis, and insurance pre-authorization numbers that were entered incorrectly.

**What I learned:** Billing errors in hospitals almost never happen because someone was dishonest. They happen because the person entering the bill is working from a paper form that was filled out by someone else under time pressure. The fix is upstream — better data capture at the point of care, not better auditing after the fact.

---

### Year 3 — Reducing Patient Dropout in Maternal Care (2022–2023)

**The problem:** The hospital runs a maternal health program. A significant number of patients who registered for antenatal care stopped coming after the first or second visit. This was a clinical risk — missed appointments during pregnancy carry real health consequences.

**What I did:** Analyzed the appointment records to find the dropout pattern. Built a simple predicted due date (EDD) tracker that identified patients who were getting close to critical milestones but hadn't booked their next appointment. Generated a weekly call-back list for the nursing staff.

**What I learned:** Health outcomes data is not about dashboards. It's about giving the right person the right information at the right moment so they can do something about it. The nurses didn't need a report — they needed a list of three names to call on Monday morning.

---

### Year 4 — Operational Reporting That Actually Worked (2023–2024)

**The problem:** Hospital management wanted monthly operational reports. The existing process involved three different staff members manually pulling numbers from three different systems, combining them in Excel, and presenting a summary that was often inconsistent month to month.

**What I did:** Automated the data collection and report generation. Built dashboards in Power BI that pulled from the cleaned data directly. Management could see bed occupancy, OPD volumes, average consultation times, and billing recovery rates in real time instead of waiting three days for a monthly report.

**What I learned:** The real value wasn't the automation. The value was that the numbers were now consistent and trusted. When management stopped questioning whether the data was right, they could actually start making decisions based on it.

---

### Year 5 — Getting Ready for ABDM (2024–2025)

**The problem:** India's Ayushman Bharat Digital Mission was coming. Hospitals would eventually be required to link patient records to ABHA IDs and handle data in ways that complied with the DPDP Act. RK Nursing Home wasn't ready — our records didn't have ABHA IDs, our consent documentation was inconsistent, and we had no audit trail for data access.

**What I did:** Built compliance audit scripts to identify the gap between our current records and what ABDM compliance would require. Documented the data gaps, mapped out what would need to change in our intake process, and wrote the functional requirements for what an ABDM-compliant intake system would need to do.

**What I learned:** Compliance isn't a checkbox — it's a workflow redesign. You can't just add an ABHA field to an Excel form and call it done. The entire patient intake process needs to change so that consent is documented, ABHA is verified, and the data is structured correctly from the first moment of contact. This is what eventually became the OmniIngest project.

---

## What Came Next

The five years of problems in this repository are the reason the OmniIngest Clinical Engine exists. Every design decision in that project — the three-phase pipeline, the 7-pillar schema, the DPDP compliance engine — came from something I ran into in real clinical floor work.

See the full OmniIngest project: [github.com/nisar46/OmniIngest-Clinical-Engine](https://github.com/nisar46/OmniIngest-Clinical-Engine)

---

**Nisar Ahmed** — Healthcare Business Analyst | [LinkedIn](https://www.linkedin.com/in/nisar-ahmed-8440763a3) | [Portfolio](https://nisar46.github.io/portfolio/)
