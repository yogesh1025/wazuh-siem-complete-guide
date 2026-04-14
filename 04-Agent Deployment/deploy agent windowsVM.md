# Deploying a Wazuh Agent on Windows

## Overview

One of Wazuh's most powerful features is its **agent-based monitoring** system. A Wazuh agent is a lightweight piece of software you install on any device — Windows, Linux, or macOS — that continuously collects security data and forwards it to your Wazuh SIEM server in real time.

This guide walks you through the complete process of deploying a Wazuh agent on a **Windows machine** and enrolling it into your Wazuh dashboard.

---

## What a Wazuh Agent Does

Once installed, the Wazuh agent on a Windows machine will:

- **Collect Windows Event Logs** — login attempts, process creation, account changes, privilege use
- **Monitor File Integrity (FIM)** — alert when critical system files are created, modified or deleted
- **Detect Vulnerabilities** — identify unpatched CVEs on the machine automatically
- **Map to MITRE ATT&CK** — tag suspicious activity with real adversary technique IDs
- **Run Configuration Assessments** — check for security misconfigurations against benchmarks like CIS
- **Enable Active Response** — allow Wazuh to automatically block IPs or kill processes when threats are detected

All of this happens silently in the background with minimal impact on system performance.

---

## Prerequisites

Before starting, make sure you have:

- [ ] Wazuh server deployed and running on your VPS
- [ ] Access to the Wazuh web dashboard
- [ ] A Windows machine (physical or virtual) with internet access
- [ ] Administrator access on the Windows machine

---

## Step-by-Step Deployment Guide

### Step 1 — Check Your Current Agents in the Dashboard

Before adding a new agent, log into your **Wazuh dashboard** and take note of the current state.

On the **Overview** page you can see the **Agents Summary** widget in the top left. This shows how many agents are currently active and disconnected.

In this example, we have:
- **Active (1)** — one machine already connected
- **Disconnected (1)** — a previously enrolled agent that is no longer reporting

The Windows machine we are about to add will become a new active agent once enrolled.

![Step 1 - Wazuh Dashboard Overview showing current agent status](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%201.png)

---

### Step 2 — Navigate to Endpoints and Click Deploy New Agent

1. In the left sidebar, click **Endpoints**
2. This page shows all enrolled agents by status, operating system, and group
3. At this point only existing agents are listed
4. Click the **"Deploy new agent"** button in the top right of the agents table

This opens the agent deployment wizard — a built-in tool in Wazuh that generates a ready-to-run installation command customised for your server.

![Step 2 - Endpoints page with Deploy New Agent button highlighted](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%202.png)

---

### Step 3 — Configure the Agent Deployment Settings

The deployment wizard has four sections. Work through each one:

**Section 1 — Select the package:**

Choose the operating system of the machine you are enrolling:
- Click **WINDOWS**
- Select **MSI 32/64 bits**

> Wazuh supports Windows 7 and above. For most modern machines, MSI 32/64 bits is the correct choice.

**Section 2 — Server address:**

This is the most important field. Enter the **IP address or hostname of your Wazuh VPS** — this is the address the agent will use to communicate with your SIEM server.

- Type your VPS IP address in the **"Assign a server address"** field
- Enable the **"Remember server address"** toggle

> Make sure you enter your actual Wazuh manager IP here — not your local machine IP. The agent needs to reach your VPS over the internet or your network.

**Section 3 — Optional settings:**

- In **"Assign an agent name"**, give this machine a clear, descriptive name
- For this example we are naming it: `WindowsVM`

> ⚠️ **Important:** The agent name must be unique across all your agents. It cannot be changed after the agent is enrolled — choose carefully.

- Leave **"Select one or more existing groups"** as **Default** for now

![Step 3 - Deploy New Agent configuration wizard](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%203.png)

---

### Step 4 — Copy the Installation Commands

Once you have filled in all the fields, scroll down to **Section 4** where Wazuh automatically generates a complete PowerShell installation command.

The command will look like this (with your actual server IP included):

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.4-1.msi -OutFile $env:tmp\wazuh-agent; msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='YOUR_VPS_IP' WAZUH_AGENT_NAME='WindowsVM'
```

**Breaking down what this command does:**

| Part | What It Does |
|---|---|
| `Invoke-WebRequest -Uri ...` | Downloads the Wazuh agent MSI installer from Wazuh's official package server |
| `-OutFile $env:tmp\wazuh-agent` | Saves the installer to the Windows temp folder |
| `msiexec.exe /i ... /q` | Silently installs the MSI with no UI prompts |
| `WAZUH_MANAGER='YOUR_IP'` | Tells the agent which server to connect to |
| `WAZUH_AGENT_NAME='WindowsVM'` | Sets the agent name as it will appear in the dashboard |

Also take note of **Section 5 — Start the agent:**

```powershell
NET START Wazuh
```

**Copy both commands** before switching to the Windows machine.

> Requirements noted by Wazuh: Administrator privileges are required, and PowerShell 3.0 or greater must be available (all modern Windows versions meet this).

![Step 4 - Installation and start commands generated by Wazuh](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%204.png)

---

### Step 5 — Open PowerShell as Administrator on the Windows Machine

Switch to your **Windows machine**. The Wazuh agent installation requires Administrator privileges — running PowerShell without them will cause the install to fail silently.

1. Click the **Start menu** (Windows icon in the taskbar)
2. Type `power` in the search bar
3. **Windows PowerShell** will appear as the top result under "Best match"
4. On the right panel, click **"Run as administrator"**

> Do not open the standard PowerShell from the taskbar or by double-clicking — it will open without administrator rights and the installation will fail.

![Step 5 - Searching for PowerShell and selecting Run as Administrator](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%205.png)

---

### Step 6 — Accept the User Account Control (UAC) Prompt

Windows will display a **User Account Control (UAC)** security prompt:

> *"Do you want to allow this app to make changes to your device?"*
> **Windows PowerShell** — Verified publisher: Microsoft Windows

Click **Yes** to grant PowerShell administrator privileges.

> **What is UAC?** User Account Control is a Windows security feature introduced in Vista that prevents unauthorised software from making system-level changes. It requires explicit user approval before any process can run with elevated privileges. In a real SOC environment, unexpected UAC prompts — especially ones triggered without user action — are a significant detection signal for privilege escalation attacks.

![Step 6 - UAC prompt requesting administrator permission](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%206.png)

---

### Step 7 — Run the Installation Command

In the **Administrator: Windows PowerShell** window that opens:

1. Paste the full installation command you copied from the Wazuh dashboard
2. Press **Enter**

PowerShell will:
1. Download the Wazuh MSI installer (~50 MB) from Wazuh's official package server
2. Run the installer silently in the background
3. Automatically configure the agent with your Wazuh server address and agent name

The command prompt will return once the installation is complete. This typically takes **1–3 minutes** depending on your internet connection speed.

> If you see a red error message saying "Access is denied" — PowerShell was not opened as Administrator. Close it and repeat Steps 5 and 6.

![Step 7 - Running the Wazuh agent installation command in PowerShell](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%207.png)

---

### Step 8 — Start the Wazuh Agent Service

Once the installation command completes successfully, run the second command to start the Wazuh agent as a Windows service:

```powershell
NET START Wazuh
```

Press **Enter**.

You should see:
```
The Wazuh service was started successfully.
```

**Why do we need to start the service separately?**

The installation command installs the Wazuh agent software but does not automatically start it. Running `NET START Wazuh` starts the agent as a **Windows Service** — meaning:
- It runs silently in the background
- It starts automatically every time Windows boots
- It keeps running even when no user is logged in

This is exactly how endpoint agents work in enterprise environments.

![Step 8 - Running NET START Wazuh to start the agent service](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%208.png)

---

### Step 9 — Verify the Agent is Active in the Wazuh Dashboard

Switch back to your **Wazuh dashboard** in the browser.

Go to **Endpoints** and click on your newly enrolled agent — **WindowsVM** — to open its detail page.

You should see the agent showing as **● active** with the following details:

| Field | Expected Value |
|---|---|
| Status | ● active |
| Operating System | Microsoft Windows 11 Pro |
| Wazuh Version | v4.14.4 (or your installed version) |
| Group | default |
| Registration Date | Today's date and time |
| Last Keep Alive | Within the last minute |

The agent dashboard will immediately begin showing real data:

- **Events count evolution** — a graph of security events over time
- **MITRE ATT&CK** — tactics already detected from normal Windows activity
- **Vulnerability Detection** — CVEs identified on the machine
- **FIM: Recent Events** — file integrity monitoring events

> You may notice that even on a freshly installed Windows machine, Wazuh immediately detects activity mapped to MITRE ATT&CK tactics such as "Defense Evasion". This is normal — Windows performs many background operations that match known technique signatures. This is exactly why SOC analysts tune detection rules — to reduce noise and focus on genuine threats.

![Step 9 - WindowsVM agent active in Wazuh with live telemetry](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%209.png)

---

### Step 10 — Verify All Agents Are Reporting

Navigate back to the main **Endpoints** page to see a full overview of all connected agents.

With the Windows agent now enrolled, the dashboard shows the complete picture of all monitored machines:

| ID | Name | Operating System | Role | Status |
|---|---|---|---|---|
| 001 | M2 | macOS | Host Machine | ● active |
| 003 | 2015 | Kali GNU/Linux | Security Testing Machine | ● active |
| 004 | WindowsVM | Microsoft Windows 11 Pro | Target / Monitored Endpoint | ● active |

The **Top 5 OS** chart updates to reflect: darwin (1), kali (1), windows (1) — showing cross-platform coverage across your monitored environment.

All agents show **● active** — your Wazuh SIEM is now receiving live security telemetry from all machines simultaneously.

![Step 10 - All agents active in Wazuh Endpoints view](https://github.com/yogesh1025/wazuh-siem-complete-guide/blob/main/04-Agent%20Deployment/images/windowsVM/Step%2010.png)

---

## ✅ Verification Checklist

Use this checklist to confirm everything is working correctly:

- [ ] Wazuh agent installed on Windows machine via PowerShell (no errors)
- [ ] `NET START Wazuh` completed with "started successfully" message
- [ ] Agent appears in Wazuh Endpoints page with **● active** status
- [ ] Agent correctly identified as **Microsoft Windows 11 Pro**
- [ ] Registration date and Last Keep Alive timestamps are current
- [ ] Events are visible in the agent's dashboard
- [ ] MITRE ATT&CK panel is populating with tactics

---

## Troubleshooting

**Agent installs but shows as Disconnected in the dashboard:**

The agent cannot reach your Wazuh server. Check:
- Your VPS firewall allows inbound traffic on **port 1514 (TCP/UDP)** — this is the Wazuh agent communication port
- The server address in the installation command is your VPS public IP, not a local IP
- Your VPS is running and the Wazuh manager service is active

**"Access is denied" error when running the install command:**

PowerShell was not opened as Administrator. Close it, right-click PowerShell, and select "Run as administrator".

**Agent does not appear in the dashboard at all:**

The installation may have failed silently. Re-run the installation command in an Administrator PowerShell. Check that your VPS IP was correctly included in the command.

**NET START Wazuh returns "service does not exist":**

The installation did not complete. Re-run the full `Invoke-WebRequest` installation command first, then run `NET START Wazuh` again.

**Agent shows active but no events are coming through:**

Wait 2–3 minutes after starting the service — there is a short delay before the first events are forwarded. If still no events after 5 minutes, restart the Wazuh service:
```powershell
NET STOP Wazuh
NET START Wazuh
```

---

## Key Concepts to Remember

**Agent vs Agentless monitoring** — Agent-based monitoring (what we just set up) gives deep endpoint visibility including process execution, registry changes, and file modifications. Agentless monitoring only captures network traffic and cannot see what is happening inside a machine.

**The agent runs as a Windows Service** — This means it starts automatically with Windows, runs in the background without any user interaction, and continues monitoring even when the machine is idle or no one is logged in.

**Wazuh agents are version-matched** — The agent version should match or be compatible with your Wazuh server version. Wazuh's deployment wizard always generates the correct version automatically.

**Agent names are permanent** — Once enrolled, an agent name cannot be changed. If you need to rename an agent, you must remove it and re-enrol it with a new name.

---

## What's Next

Now that your Windows machine is enrolled and reporting to Wazuh, explore these sections to get the most out of your setup:

- **[05 — Detection & Alerts](../05-Detection-and-Alerts/understanding-alerts.md)** — Learn how to read and interpret the security events now flowing from your Windows machine
- **[07 — MITRE ATT&CK](../07-MITRE-ATTCK/mitre-integration.md)** — Understand the ATT&CK tactics already appearing in your agent dashboard
- **[06 — Custom Rules](../06-Custom-Rules/writing-custom-rules.md)** — Write your own detection rules to catch specific activity on this machine

---

*Wazuh SIEM Complete Guide | Section 04 — Agent Deployment | Windows*
