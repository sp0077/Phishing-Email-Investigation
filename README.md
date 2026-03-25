# Phishing Email Investigation – Malware Attachment Analysis

## 📌 Overview

This case study documents the investigation of a suspicious phishing email that attempted to socially engineer the recipient by claiming a successful financial transaction via SWIFT and delivering a malicious attachment disguised as a payment receipt.

The objective of this investigation was to determine the legitimacy of the email, identify Indicators of Compromise (IOCs), and confirm whether the attachment contained malware.

---

## 🎯 Investigation Objectives

* Analyze email headers to identify the true source of the email
* Verify sender domain authentication mechanisms (SPF and DMARC)
* Extract and analyze the attachment hash
* Confirm malware presence using threat intelligence platforms
* Identify actionable security recommendations

---

## 📧 Email Details

| Field                    | Value                                                 |
| ------------------------ | ----------------------------------------------------- |
| Subject                  | Transfer Reference Number (09674321)                  |
| Displayed Sender         | [info@mutawamarine.com](mailto:info@mutawamarine.com) |
| Attachment               | SWT_#09674321__PDF__.CAB                              |
| Social Engineering Theme | Financial transaction / urgency                       |

The email attempted to create trust and urgency by referencing a financial transfer and attaching a supposed payment receipt.

---

## 🔎 Step 1 — Email Header Analysis

The raw email headers were analyzed to determine the originating infrastructure.

### Key Findings

* Originating IP Address: **192.119.71.157**
* Hosting Provider: **Hostwinds LLC**
* Location: **Dallas, Texas (US)**

This indicates that the email was sent from a hosting environment rather than a legitimate enterprise mail server, which is a common tactic used in phishing campaigns.

---

## 🔎 Step 2 — Sender Policy Framework (SPF) Analysis

The sender domain SPF record was queried to validate authorized mail servers.

```
v=spf1 include:spf.protection.outlook.com -all
```

### Observation

Although SPF was configured, this does not fully guarantee legitimacy. Attackers may still send spoofed emails using alternative infrastructure or compromised systems.

---

## 🔎 Step 3 — DMARC Policy Analysis

The DMARC record for the sender domain was retrieved.

```
v=DMARC1; p=quarantine; fo=1
```

### Observation

The domain enforces a quarantine policy for failed authentication checks. However, phishing emails can still bypass protection if authentication alignment is manipulated or if detection controls are insufficient.

---

## 🔎 Step 4 — Attachment Analysis

The suspicious attachment was extracted and hashed for malware verification.

### File Details

* File Name: SWT_#09674321__PDF__.CAB
* Actual File Type: **RAR Archive (Disguised as PDF)**
* File Size: ~400 KB

### SHA256 Hash

```
2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b333525e25074fff3f
```

---

## 🦠 Malware Verification (VirusTotal)

Threat intelligence lookup confirmed malicious classification.

### Results

* **46 / 63 security vendors flagged the file as malicious**
* Classified as malware spreader attachment
* Archive-based payload delivery technique observed

This confirms that the email was part of a malware delivery phishing campaign.

---

## 🚨 Indicators of Compromise (IOCs)

| Type                  | Indicator                                                         |
| --------------------- | ----------------------------------------------------------------- |
| Originating IP        | 192.119.71.157                                                    |
| Malicious File Hash   | 2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b333525e25074fff3f |
| Suspicious Attachment | SWT_#09674321__PDF__.CAB                                          |
| Hosting ASN           | AS54290 (Hostwinds LLC)                                           |

---

## 🛡️ Recommended SOC Actions

* Block originating IP address at firewall / email gateway
* Add malicious hash to EDR and antivirus blocklists
* Alert users about financial phishing themes
* Improve attachment filtering and sandbox analysis
* Monitor environment for similar phishing attempts

---

## 🧠 MITRE ATT&CK Mapping

* **T1566.001 — Phishing: Spearphishing Attachment**

This attack demonstrates adversary use of malicious attachments to gain initial access.

---

## ✅ Conclusion

The investigation confirmed that the email was a malicious phishing attempt leveraging financial social engineering techniques and malware delivery through a disguised archive attachment.

Proper email authentication monitoring, threat intelligence usage, and user awareness remain critical in preventing such attacks.

---

## 📁 Repository Contents

* Investigation screenshots
* Header analysis evidence
* Threat intelligence lookup results
* Malware hash verification

---

## 👨‍💻 Author

Sarvesh Pandekar
Cybersecurity Enthusiast | SOC Analyst Aspirant
