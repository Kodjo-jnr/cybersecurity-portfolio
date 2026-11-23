# Incident Response Coursework — Network Traffic Analysis, Handler's Journal, Phishing Playbook

**Analyst:** Isaac Osei Mensah
**Programme:** Google Cybersecurity Certificate coursework (2023–2024)
**What this is:** Incident-response work products from certificate coursework, built from fictional scenarios and lab traffic captures. Training work — not employment, and no real organisation's network or data is involved.

---

## 1. Cybersecurity incident report: network traffic analysis

**Scenario:** Customers of a fictional company reported they could not access its website, yummyrecipesforme.com, and saw a "destination port unreachable" error after waiting for the page to load.

**Summary of the problem (from the DNS and ICMP traffic log):** Analysis of the traffic showed the DNS request never completed — when a UDP packet was sent requesting the site's IP address, the ICMP response came back with the error **"UDP port 53 unreachable."** Port 53 is the DNS service port, so the message never went through the DNS server and no IP address was returned for the site.

**Investigation, as documented in the report:**

- **Time of incident:** 1:24 pm
- **How the team became aware:** Customer reports of the website being unreachable, with the "destination port unreachable" error.
- **Actions taken:** The IT team reproduced the error by visiting the website themselves, then captured traffic with the network analyser **tcpdump** while loading the site again. The captured packets confirmed the pattern: UDP packets sent out, ICMP response returned containing "udp port 53 unreachable."
- **Key finding:** UDP port 53 unreachable — the request did not reach the DNS server.
- **Likely cause recorded in the report:** a DDoS attack against the service.

**Skills this demonstrates:** Reading DNS/ICMP traffic in a packet capture, distinguishing a name-resolution failure from a web-server failure, reproducing a user-reported fault before analysing it, and writing up an incident in a structured summary-plus-analysis format.

## 2. Incident handler's journal — ransomware scenario entry

The journal uses the standard handler's format (description, tools, the 5 W's, notes). One entry is completed in full, dated **Sunday, 11 February 2024**:

- **Description:** A ransomware attack on a healthcare clinic, blocking access to critical files on the company's computers and halting operations until payment.
- **The 5 W's, as recorded:** *Who* — an organised group of unethical hackers. *What* — employees could not access critical patient files and software needed to do their jobs, and a ransom note was displayed on their computers. *When* — Tuesday morning at 9:00 am. *Where* — on the premises of the healthcare clinic. *Why* — targeted phishing emails to employees, with malware installed from the attached files.

The remaining journal tables in the document were left as blank templates, and a second copy of the journal in Drive is entirely blank — so this page claims the one completed entry only.

**Skills this demonstrates:** Structuring an incident record around the 5 W's and identifying phishing as the initial-access vector in a ransomware event.

## 3. Phishing incident response playbook (course material)

My Drive also holds the certificate programme's **Phishing Playbook, Version 1.0** — a level-one SOC analyst playbook retained from the coursework. I am describing it as course material rather than my own authorship: the two copies in Drive are identical and carry no personalisation I can point to as my own writing.

Its workflow, which the coursework trained me to follow:

1. **Receive the phishing alert** ticket.
2. **Evaluate the alert** — alert severity (low: no escalation; medium: may require escalation; high: immediate escalation), receiver and sender email/IP details, subject line, message body, attachments and links. Never open links or attachments outside an authorised, isolated environment.
3. **If the email contains links or attachments,** check their reputation through hash values using threat intelligence tools such as **VirusTotal**. If confirmed malicious, update the ticket with a summary of findings, set the status to Escalated, and notify a level-two SOC analyst.
4. **Close the ticket** — with a brief summary of findings and the reason — when the email has no links or attachments, or when they are confirmed not malicious.

**Skills this demonstrates:** A disciplined alert-triage sequence for phishing: evaluate before acting, verify payload reputation before escalating, and document the reason for every escalation or closure.
