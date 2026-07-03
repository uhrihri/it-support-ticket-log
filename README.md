# 🖥️ IT Support Ticket Logs & Knowledge Base

Welcome to the central repository for my IT support incident logs, troubleshooting guides, & permanent fixes. This folder serves as both a historical record of resolved issues & a self-service knowledge base. Includes documented IT support tickets from my place of work, home lab, & practice scenarios.

---

## 🎯 Purpose of This Repository

- **Document Resolution:** To permanently record the root cause & fix for every unique technical issue encountered.
- **Enable Self-Service:** To allow viewers to search for existing solutions before escalating or reinventing the wheel.
- **Reduce MTTR (Mean Time To Resolve):** To drastically cut down troubleshooting time for recurring issues using pre-verified commands & steps.
- **Standardize Documentation:** To maintain a uniform structure across all tickets for clarity & auditability.

---

## 📁 Folder & File Naming Convention

To ensure files are easily searchable, all ticket logs must follow the **kebab-case** naming standard (lowercase, hyphens instead of spaces).

**Format:** 
`[ticket-serial-number]-[category]-[keyword]-[error-code]-[brief-description].md`

**Examples:**
- `networking-error-0x80070035-network-share-fix.md`
- `printing-wireless-printer-subnet-issue.md`
- `quickbooks-h202-database-connection-error.md`
- `software-outlook-profile-corruption-fix.md`

**Prefixes (Optional but recommended):**
- `incident-` (for a specific, one-time user report)
- `kb-` (for a permanent Knowledge Base article)
- `troubleshoot-` (for a guide focusing mainly on the investigation process)

---

## 📝 Mandatory Ticket Structure

Every Markdown (`.md`) file in this repo must contain the following sections to ensure consistency. Please use these exact headings.

| Section | Required? | Description |
| :--- | :--- | :--- |
| **Title (H1)** | ✅ Yes | Clear, descriptive title including error codes if present. |
| **Issue / Symptom** | ✅ Yes | What the user reported in their own words. |
| **Environment** | ✅ Yes | OS, software version, hardware model (e.g., Windows 10 Pro, QuickBooks 16.0). |
| **Troubleshooting** | ✅ Yes | The step-by-step diagnostic tests performed (e.g., ping, IP config, logs reviewed). |
| **Root Cause** | ✅ Yes | The specific technical reason the break occurred. |
| **Workaround (if applicable)** | ⚠️ Optional | Any temporary steps taken to get the user working while preparing the permanent fix. |
| **Permanent Fix** | ✅ Yes | The exact commands, scripts, or configuration changes applied to resolve the issue. |
| **Verification** | ✅ Yes | How you proved the issue was solved (e.g., "User printed test page successfully"). |
| **Resolution** | ✅ Yes | Final status and confirmation from the end-user. |
| **Related Links** | ⚠️ Optional | Cross-references to other tickets or external documentation. |

---

## 🔍 How to Use This Repository

### 1. Searching for an Existing Solution
- Use the **GitHub search bar** and type your error code (e.g., `0x80070035`) or a key symptom (e.g., `"network path"`).
- Browse the file list—filenames are designed to be self-explanatory.

### 2. Creating a New Ticket Log
1.  Download the template from the `/templates` folder (or manually create a new `.md` file).
2.  Name it using the **Kebab-Case naming convention** defined above.
3.  Fill in every section under the "Mandatory Ticket Structure".
4.  Commit the file to the appropriate sub-folder (e.g., `/networking/`, `/software/`, `/hardware/`).

---

## 🧠 Terminology Guide (For Consistency)

To avoid confusion in our logs, please use these terms strictly:

| Term | Definition |
| :--- | :--- |
| **Troubleshooting** | The *investigation phase*—testing, pinging, and gathering data to find the cause. |
| **Root Cause** | The single underlying technical reason for the failure. |
| **Workaround** | A *temporary bypass* that restores functionality without fixing the underlying defect. |
| **Fix** | The *permanent action* (script, config change, or update) that eliminates the root cause. |
| **Verification** | The evidence that proves the fix worked and the system is stable. |
| **Resolution** | The final closure status, combining the fix and successful verification. |

---

## 🗂️ Suggested Folder Structure (Optional)

Organizing by domain helps locate relevant files faster:

```text
📁 ticket-logs/
├── 📁 networking/        # Firewall, DNS, SMB, Wi-Fi, VPN issues
├── 📁 software/          # QuickBooks, Office 365, Browser issues
├── 📁 hardware/          # Printers, Docking Stations, Drives
├── 📁 security/          # Permissions, MFA, User Access
├── 📁 templates/         # Blank template.md for new tickets
└── 📄 README.md          # This file
