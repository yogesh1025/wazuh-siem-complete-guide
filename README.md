# 🛡️ Wazuh SIEM — Complete Deployment & Security Guide

> A practical, step-by-step guide to deploying Wazuh SIEM on a VPS, connecting agents across Windows, Linux and macOS, and building a fully monitored secure network environment from scratch — no enterprise budget required.

---

## 👋 Welcome

Whether you're a cybersecurity student, a home lab enthusiast, or an IT professional looking to add real security monitoring to your network — this guide is for you.

**Wazuh is one of the most powerful free and open-source security platforms in the world.** It's the same technology used by security teams at organisations of all sizes to detect threats, monitor endpoints, investigate incidents, and maintain compliance. The best part? It costs nothing to deploy.

This repository walks you through everything — from spinning up a VPS to having a fully operational SIEM monitoring your devices in real time. No prior experience with SIEM platforms is required. Every step is documented with screenshots, commands, and plain-English explanations of what's happening and why.

---

## 🎯 What You Will Learn

By following this guide end to end, you will be able to:

- **Deploy Wazuh** on a cloud VPS from scratch
- **Connect any device** — Windows, Linux, or macOS — as a monitored endpoint
- **Read and interpret security alerts** like a real SOC analyst
- **Write custom detection rules** tailored to your environment
- **Use the MITRE ATT&CK framework** to understand and categorise threats
- **Harden your Wazuh server** and apply network security best practices
- **Build a home security lab** that mirrors enterprise SOC infrastructure

These are real, job-relevant skills. This is not a theoretical course — every section is hands-on and built around actual deployment experience.

---

## 🗂️ Guide Structure

The guide is broken into focused sections. You can follow it sequentially from start to finish, or jump to the section most relevant to you.

| Section | Title | Status |
|---|---|---|
| 01 | Introduction — What is Wazuh? | 🔜 Coming Soon |
| 02 | VPS Deployment — Installing Wazuh Server | 🔜 Coming Soon |
| 03 | Dashboard Overview — Understanding the UI | 🔜 Coming Soon |
| 04 | Agent Deployment — Windows, Linux & macOS | ✅ Windows Complete |
| 05 | Detection & Alerts — Reading Security Events | 🔜 Coming Soon |
| 06 | Custom Rules — Writing Your Own Detections | 🔜 Coming Soon |
| 07 | MITRE ATT&CK — Mapping Threats to Techniques | 🔜 Coming Soon |
| 08 | Hardening & Security — Securing Your Environment | 🔜 Coming Soon |
| 09 | Troubleshooting — Common Issues & Fixes | 🔜 Coming Soon |

> ⭐ **Star this repository** to get notified as new sections are published.

---

## 🔧 What You Need to Get Started

You don't need expensive hardware or software. Here's the minimum you need to follow along:

- A **VPS** from any provider (DigitalOcean, Linode, Vultr, AWS — any works) with at least 4GB RAM
- A **device to monitor** — any Windows, Linux, or macOS machine
- Basic comfort with the **command line** (no deep Linux knowledge required)
- A browser to access the Wazuh web dashboard

That's it. Everything else is free and open source.

---

## 🌐 Why Wazuh?

There are many SIEM platforms out there — Splunk, Microsoft Sentinel, IBM QRadar. Most of them cost thousands of dollars per year. Wazuh gives you enterprise-grade capabilities completely free, including:

- **Threat Detection** — real-time alerting on suspicious activity across all your endpoints
- **File Integrity Monitoring** — instant alerts when critical files are changed
- **Vulnerability Detection** — automatic identification of unpatched CVEs on your devices
- **Incident Response** — active response capabilities to automatically contain threats
- **Compliance Monitoring** — built-in support for PCI DSS, HIPAA, GDPR, NIST 800-53
- **MITRE ATT&CK Integration** — every alert mapped to real-world adversary techniques
- **Cloud Security** — native integrations with AWS, Azure, Google Cloud, and GitHub

For anyone learning cybersecurity, having Wazuh on your resume and in your portfolio is a genuine differentiator. It is directly referenced in job descriptions for SOC Analyst, Security Engineer, and Incident Responder roles.

---

## 📸 What the Final Setup Looks Like

Once you've followed this guide, your Wazuh dashboard will look like this — with all your devices reporting in as active agents, security events flowing in real time, and full MITRE ATT&CK visibility across your network.

```
Wazuh Dashboard
├── Agents (Active)
│   ├── 001 — MacBook         ● active
│   ├── 002 — Kali Linux      ● active
│   └── 003 — Windows 11      ● active
│
├── Last 24 Hours Alerts
│   ├── Critical:  0
│   ├── High:      0
│   ├── Medium:   62
│   └── Low:      22
│
└── MITRE ATT&CK Coverage
    ├── Defense Evasion
    ├── Discovery
    └── Credential Access
```

---

## 🤝 Who This Guide Is For

**Beginners** — If you have never touched a SIEM before, this guide starts from zero. Every concept is explained in plain English before any commands are run.

**Students** — If you are studying for CompTIA Security+, CySA+, or any cybersecurity certification, hands-on Wazuh experience will reinforce everything you're learning and give you real examples to reference.

**Home Lab Builders** — If you already have a home lab and want to add real security monitoring to it, jump straight to Section 02 and Section 04.

**Career Changers** — If you are transitioning into cybersecurity and building your portfolio, completing this guide gives you documented, demonstrable SIEM experience to talk about in interviews.

**IT Professionals** — If you manage a small business or personal network and want visibility into what's happening on your devices without a six-figure SIEM contract, this is exactly what you need.

---

## 📌 A Note on How This Guide Was Built

This guide was built through real, hands-on deployment — not copied from documentation. Every screenshot, every command, and every explanation comes from actually going through the process of setting up Wazuh from scratch on a real VPS, connecting real machines, and working through real issues.

That means when something went wrong during setup, the fix is documented here too. This is a practical guide, not a marketing brochure.

---

## 🔔 Stay Updated

This guide is actively being built. New sections are being added regularly.

- ⭐ **Star the repository** to follow progress
- 👁️ **Watch the repository** to get notifications when new sections drop
- 🍴 **Fork it** if you want your own copy to work through at your own pace

---

## 📄 Licence

This guide is open source and free to use for personal and educational purposes.

---

*Built with real hands-on experience. No fluff, no filler — just practical security knowledge.*
