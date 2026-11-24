# NETWORKWALKS-RUTUJA-B083-WK4-PM-PENTEST-MEDIROZA
> Week 4 Black-Box Penetration Testing Project | Networkwalks Cybersecurity Internship | B083


# PENETRATION TESTING REPORT — MEDIROZA GENERAL HOSPITAL

A comprehensive black-box penetration testing report detailing findings across initial access, offline password cracking, sensitive data exposure, and remediation strategies for healthcare infrastructure.

<div align="center">

![Project](https://img.shields.io/badge/PROJECT-PENETRATION%20TESTING-8A2BE2?style=for-the-badge&labelColor=555555)
![Targets](https://img.shields.io/badge/TARGETS-MEDIROZA%20HOSPITAL-A52A2A?style=for-the-badge&labelColor=555555)
![OS](https://img.shields.io/badge/OS-KALI%20LINUX%20%2F%20LITESPEED-008000?style=for-the-badge&labelColor=555555)
![Toolkit](https://img.shields.io/badge/TOOLKIT-GOBUSTER%20%2F%20PDFCRACK%20%2F%20EXIFTOOL-0066CC?style=for-the-badge&labelColor=555555)
![Skill](https://img.shields.io/badge/SKILL-RECON%20%2F%20CRACKING%20%2F%20REPORTING-CC0000?style=for-the-badge&labelColor=555555)
![Internship](https://img.shields.io/badge/INTERNSHIP-NETWORKWALKS%20B083-556B2F?style=for-the-badge&labelColor=555555)
![GitHub](https://img.shields.io/badge/GITHUB-RUTUJA--MEDHEKAR-333333?style=for-the-badge&labelColor=555555)
![Author](https://img.shields.io/badge/AUTHOR-RUTUJA%20MEDHEKAR-8A2BE2?style=for-the-badge&labelColor=555555)

</div>

---


⚠️ Disclaimer

This project was performed strictly as part of an authorized cybersecurity training engagement under the Networkwalks Cybersecurity Internship program.

All testing activities were conducted only against the designated training target, Mediroza General Hospital, within the permitted educational environment.

The techniques, tools, commands, and methodologies demonstrated in this repository must never be used against systems, websites, networks, applications, or data without explicit authorization from the owner.

Sensitive information discovered during the assessment has been redacted or excluded from this public repository wherever appropriate. Only information required for educational documentation and assessment purposes is included.

---


🎯 Project Overview

This project documents a full black-box penetration testing assessment conducted against the simulated Mediroza General Hospital environment as part of Week 4 of the Networkwalks Cybersecurity Internship — Batch B083.

The assessment was performed in a controlled and authorized training environment and followed a structured penetration-testing methodology covering reconnaissance, enumeration, initial access, data security testing, critical exposure analysis, and professional security reporting.

The engagement was completed across four milestones:

| Milestone | Objective | Status |
|---|---|---|
| M1 – Initial Access | Recon the target and retrieve 3 confidential (encrypted) patient PDF reports | ✅ Complete |
| M2 – Data Extraction | Crack the encryption on all 3 retrieved PDFs | ✅ Complete |
| M3 – Critical Exposure | Find a further critical data exposure on the server | ✅ Complete |
| M4 – Reporting | Write a full professional penetration testing report | ✅ Complete |

---

🧭 Assessment Methodology

The assessment followed a black-box methodology.

                    START
                      │
                      ▼
             Scope & Authorization
                      │
                      ▼
              Reconnaissance
                      │
                      ▼
               Enumeration
                      │
                      ▼
             Initial Access
                      │
                      ▼
          Data Discovery & Analysis
                      │
                      ▼
          Security Testing / Validation
                      │
                      ▼
          Critical Exposure Analysis
                      │
                      ▼
             Evidence Collection
                      │
                      ▼
             Risk Assessment
                      │
                      ▼
         Remediation Recommendations
                      │
                      ▼
              Final Report
                      │
                      ▼
                     END

---

🛠️ Tools & Technologies

The following tools and technologies were used where applicable during the assessment:

Tool| Purpose
WHOIS| Domain and registration information gathering
nslookup| DNS information and resolution
WhatWeb| Web technology fingerprinting
Gobuster| Directory and resource enumeration
ffuf| Web content discovery and fuzzing
curl| HTTP requests and response analysis
sqlmap| Authorized SQL injection testing
pdf2john| PDF password hash extraction
John the Ripper| Password security testing
pdfcrack| Authorized PDF password testing
ExifTool| Metadata analysis
qpdf| PDF inspection and processing
pdftotext| Text extraction from PDF documents

«Tool usage was limited to the authorized training environment.»

---

🔍 M1 — Reconnaissance & Initial Access

Profiled the domain with WHOIS, nslookup, and WhatWeb, then enumerated directories/files with Gobuster and ffuf — had to exclude a soft-404 (HTTP 200) response length to get clean results. This led to discovering and accessing a restricted patient portal exposing 3 encrypted, downloadable pathology PDF reports.

<img width="1600" height="995" alt="1" src="https://github.com/user-attachments/assets/f9d1656e-533d-45fb-a4c4-76298ff7ff76" />
Recon

---

<img width="1600" height="880" alt="2" src="https://github.com/user-attachments/assets/b472f6f7-b35e-4da1-9471-d9814ca36245" />
Directory Enumeration

---


Patient portal access:
<img width="1599" height="912" alt="WhatsApp Image 2026-10-02 at 11 19 13 PM" src="https://github.com/user-attachments/assets/b1ceeb43-4823-40fa-8403-3b635cf3638b" />


{Full step-by-step evidence (nslookup, initial gobuster run, mobile confirmation) is in the "M1/" folder.}

---

 ## 🔍 M2 — Password Cracking

Extracted PDF hashes with `pdf2john.pl` for John the Ripper. John the Ripper returned `No password hashes loaded` across multiple wordlists despite PDF support being available.

Instead of repeatedly troubleshooting the same parser issue, the testing workflow was switched to `pdfcrack`, which attacks the PDF files directly.

Two PDF files were successfully tested using a small curated wordlist, while the third required a larger wordlist.

<img width="1600" height="976" alt="WhatsApp Image 2026-10-02 at 11 20 12 PM" src="https://github.com/user-attachments/assets/4b16cebf-e587-489a-b501-b2bcd152a313" />


**Lesson learned:** When one password-cracking tool encounters a file-format or parser compatibility issue, switching to an alternative tool designed for the target file format can help validate the finding.

_{
Troubleshooting evidence for the John the Ripper dead-end is in
`M2/01_pdf2john_and_john_troubleshooting.png`.
}_
---

 🔍 M3 — Critical Data Exposure

Reviewed metadata ("exiftool") on the decrypted PDFs — one file contained an internal staff comment referencing a backup directory left behind after a site migration. That directory turned out to have directory listing enabled and contained a full internal database backup, exposing a complete staff table and a shareholder/equity table — the two exact data points targeted in this milestone.

<img width="1600" height="719" alt="WhatsApp Image 2026-10-02 at 11 20 39 PM" src="https://github.com/user-attachments/assets/addfdfe8-1866-4e3b-a2d5-5767bc18eeec" />


Lesson learned: Document metadata is an underrated recon surface — a single forgotten comment field can reveal a much larger security exposure.

{Redacted evidence of the extracted salary/shareholder data is in "M3/02_staff_salary_and_shareholder_data_REDACTED.png"; raw proof files ("proof_directory_listing.html", "proof_file_headers.txt") are also in "M3/".}

---

📄 M4 — Professional Penetration Testing Report

Objective

Milestone 4 documents the complete penetration-testing engagement in a professional security assessment report.

The report includes:

- Executive Summary
- Scope
- Methodology
- Tools Used
- Assessment Timeline
- Technical Findings
- Evidence
- Risk Classification
- Business Impact
- Technical Impact
- Remediation Recommendations
- Conclusion

Final Reports
[Download Penetration Testing Report (PDF)](B083_Week4_Penetration_Testing_Report_COMPLETE.pdf)

---


📊 Key Findings (Summary)
| # | Finding | Risk |
|---|---|---|
| 1 | Weak/absent access control on the patient portal | 🔴 Critical |
| 2 | Trivially weak passwords protecting encrypted patient PDFs | 🔴 Critical |
| 3 | Sensitive internal comments left in document metadata | 🟠 High |
| 4 | Legacy backup directory publicly exposed with directory listing enabled | 🔴 Critical |
(Full evidence, screenshots, and remediation steps are in the report PDF — sensitive raw

«Risk classifications above are documented for the training assessment and should be interpreted together with the detailed evidence and impact analysis in the final report.»

---

🔎 Finding 1 — Access Control Exposure

Description

A weakness in access-control enforcement was identified during the authorized assessment.

The issue demonstrated that resources intended to have restricted access could potentially be accessed without sufficient authorization controls.

Potential Impact

Depending on the exposed resource, improper access control could result in:

- Unauthorized information disclosure
- Exposure of patient-related information
- Privacy violations
- Unauthorized resource access
- Increased attack surface

Recommendation

Implement strong server-side authorization controls and ensure that every sensitive endpoint verifies the user's authentication and authorization status before returning protected resources.

---

🔎 Finding 2 — Weak Protection of Encrypted Documents

Description

Protected documents were subjected to authorized password-security testing.

The assessment demonstrated the importance of using strong, unique passwords and appropriate cryptographic protection for sensitive documents.

Potential Impact

Weak document passwords may allow unauthorized individuals to recover sensitive information if encrypted files are obtained.

Recommendation

- Use strong, randomly generated passwords.
- Avoid predictable passwords.
- Use modern encryption standards.
- Apply appropriate key-management practices.
- Avoid distributing sensitive files through publicly accessible locations.
- Monitor access to protected documents.

---

🔎 Finding 3 — Sensitive Metadata Exposure

Description

Document metadata was reviewed during the assessment.

Metadata can unintentionally reveal internal information about documents, users, systems, workflows or organizational processes.

Potential Impact

Metadata exposure may assist attackers during reconnaissance and provide additional information about the target environment.

Recommendation

Organizations should:

- Remove unnecessary metadata before publishing files.
- Establish document-sanitization procedures.
- Review files before external distribution.
- Use automated metadata-cleaning processes where appropriate.
- Include metadata review in secure document-handling policies.

---

🔎 Finding 4 — Legacy / Backup Resource Exposure

Description

The assessment investigated potentially exposed legacy or backup resources within the authorized environment.

Legacy resources should not remain publicly accessible after migration or deployment changes.

Potential Impact

Exposed backup files or directories can potentially disclose:

- Application information
- Configuration information
- Database content
- Internal documentation
- User information
- Organizational information

Recommendation

- Remove unnecessary backup files from public directories.
- Disable directory listing.
- Store backups outside publicly accessible web roots.
- Apply strict access controls.
- Encrypt sensitive backups.
- Regularly audit legacy resources.
- Include backup exposure checks in deployment procedures.

---

## 💬 Troubleshooting Note

During Milestone 2, `pdf2john` was used to extract the password hashes from the encrypted PDF files for testing with John the Ripper. However, John the Ripper returned:

`No password hashes loaded`

even though PDF support was available.

Instead of repeatedly troubleshooting the same hash-parser issue, the testing approach was changed to `pdfcrack`, which works directly against the PDF file without requiring an intermediate hash file.

This alternative approach successfully validated the password security of the authorized test PDFs.

**Lesson learned:** When a security tool encounters a file-format or parser compatibility issue, verify the tool configuration first and then consider an alternative tool designed to work directly with the target file format.

---
🧪 Evidence Handling

All evidence collected during the engagement was handled according to the requirements of the training exercise.

Evidence was categorized into:

Reconnaissance Evidence
Enumeration Evidence
Access Evidence
Security Testing Evidence
Metadata Evidence
Exposure Evidence
Reporting Evidence

Sensitive information was redacted before being placed in the public GitHub repository.

---

🛡️ Remediation Summary

The following security improvements are recommended:

Access Control

- Implement server-side authorization.
- Apply least-privilege principles.
- Restrict sensitive endpoints.
- Test authorization controls regularly.

Data Protection

- Use strong encryption.
- Use strong passwords.
- Protect encryption keys.
- Avoid storing sensitive files in public locations.

Web Security

- Disable unnecessary directory listing.
- Remove legacy files.
- Remove unused endpoints.
- Regularly perform web-content discovery during security assessments.

Metadata Security

- Sanitize documents before external distribution.
- Remove unnecessary comments and internal information.
- Establish secure document-handling procedures.

Backup Security

- Store backups outside the public web directory.
- Encrypt backups.
- Restrict backup access.
- Monitor backup storage.
- Regularly remove obsolete backup files.

---


---

📝 Lessons Learned

This Week 4 assessment provided practical experience in:

- Performing structured reconnaissance
- Understanding web application attack surfaces
- Enumerating web resources
- Identifying access-control weaknesses
- Performing authorized password-security testing
- Analyzing document metadata
- Investigating legacy resources
- Handling sensitive security evidence
- Redacting confidential information
- Assigning risk levels
- Writing professional penetration-testing reports

A major lesson from the engagement was that security weaknesses are often connected.

A seemingly small exposure, such as an overlooked file, metadata field or legacy resource, can provide additional information that helps identify more significant security risks.

---

💡 Security Best Practices

Organizations should continuously:

1. Review public-facing assets.
2. Enforce strong authentication and authorization.
3. Protect sensitive documents.
4. Remove unnecessary metadata.
5. Secure backup files.
6. Disable directory listing where unnecessary.
7. Remove legacy resources.
8. Monitor exposed services.
9. Conduct regular vulnerability assessments.
10. Perform authorized penetration testing.

---

🎓 Internship Information

Program: Networkwalks Cybersecurity Internship
Batch: B083
Week: 04
Project: Full Black-Box Penetration Testing
Target: Mediroza General Hospital
Participant: Rutuja Medhekar

---

👩‍💻 Author

Rutuja Medhekar

Cybersecurity Intern
Networkwalks Cybersecurity Internship
Batch B083
🔗 LinkedIn: (https://lnkd.in/p/dsyk4jQG)

---

📌 Project Status

Component| Status
Reconnaissance| ✅ Completed
Enumeration| ✅ Completed
Initial Access| ✅ Completed
Data Security Testing| ✅ Completed
Metadata Analysis| ✅ Completed
Critical Exposure Analysis| ✅ Completed
Evidence Collection| ✅ Completed
Risk Assessment| ✅ Completed
Remediation Planning| ✅ Completed
Professional Report| ✅ Completed
README Documentation| ✅ Completed

---

⚠️ Responsible Disclosure & Ethics

This repository is intended strictly for educational and cybersecurity training purposes.

The techniques demonstrated here should only be performed:

- Against systems you own;
- Against systems for which you have explicit authorization; or
- Within a controlled cybersecurity training/laboratory environment.

Unauthorized access, data extraction, password attacks, vulnerability exploitation, or testing against third-party systems may be illegal and may cause real-world harm.

Always obtain explicit permission before performing security testing.

---

⭐ Final Note

This project demonstrates the practical application of penetration-testing concepts learned during Week 4 of the Networkwalks Cybersecurity Internship.

The engagement follows a structured approach from:

Reconnaissance → Enumeration → Initial Access → Security Testing → Exposure Analysis → Risk Assessment → Remediation → Professional Reporting

---

© 2026 Rutuja Medhekar | Networkwalks Cybersecurity Internship | Batch B083


