# 🤖 UiPath REFramework – ACME Queue Automation (Dispatcher & Performer)

![UiPath](https://img.shields.io/badge/UiPath-REFramework-blue?logo=uipath&style=for-the-badge)
![Automation](https://img.shields.io/badge/Automation-Orchestrator%20Trigger-success?style=for-the-badge)


##  Project Overview

This project implements a **complete UiPath REFramework solution** designed for **queue-based automation** using the **ACME website** as a case study.  


This project starts **automatically** when the **robot account receives an email** with a specific subject line.  

The **Email Event Trigger** in **UiPath Orchestrator** monitors the robot mailbox, and when an incoming message matches the configured subject (e.g., `"Start Dispatcher"`), the trigger launches the **Dispatcher process**.

It includes two interconnected processes:

-  **Dispatcher** – Extracts data from ACME and uploads it to an **Orchestrator Queue**.  
-  **Performer** – Processes each queued item, performs business actions, and generates a report.

Both processes communicate seamlessly through a **Queue Trigger** in Orchestrator — ensuring that once the Dispatcher finishes uploading data, the Performer starts automatically.

This design allows **hands-free automation** — no manual start needed.

---

##  Dispatcher Process

**Main Responsibilities:**
1.  Login to **ACME System** using credentials stored in **Orchestrator Assets**.  
2.  Navigate to Work Items and **extract data** filtered by specific Business Rules or Task IDs.  
3.  **Check for duplicates** before uploading (prevents reprocessing).  
4.  **Add new Queue Items** dynamically with structured Transaction Data.  
5.  Logout and close browser after upload is complete.  

---

##  Performer Process

**Main Responsibilities:**
1.  Triggered automatically when new items are added to the queue.  
2.  Login to ACME again using credentials from Orchestrator Assets.  
3.  Navigate to the corresponding **Work Item** from the Queue Transaction.  
4.  Perform required **business actions** such as:
   - Adding or updating comments  
   - Changing the item’s status  
   - Making field updates based on business needs    
5.  **Generate a detailed Excel report** summarizing:
   - Transaction ID  
   - Processing result (Success / Failed)  
   - Exception message (if any)  
6.  **Email the report** automatically to configured recipients.

---
## Configuration & Dynamic Assets

- Config.xlsx → stores URLs, Queue names, Asset keys, Email lists.
- Orchestrator Assets → used for credentials and environment variables.
- Orchestrator Queue → acts as the bridge between Dispatcher and Performer.

All values can be updated either in **Config.xlsx** or **Orchestrator Assets** without touching the workflow code.

---

##  Reporting

At the end of each run, the Performer automatically generates a **Transaction Report**:

| Transaction ID | Status | Reason | Timestamp |
|----------------|---------|---------------------|------------|
| 12345 | Success |  | 2025-10-31 11:22 |
| 12346 | Failed | Element not found | 2025-10-31 11:23 |

---

##  Tech Stack

| Technology | Purpose |
|-------------|----------|
| **UiPath Studio** | Main automation development |
| **UiPath Orchestrator** | Queue management, assets, and triggers |
| **REFramework** | Transaction handling and error management |
| **Excel Activities** | For report generation |
| **SMTP** | For email notifications |

---

##  Business Value

-  Reduces manual data entry and processing effort.  
- Prevents duplicate records through smart pre-check logic.  
-  Provides full visibility of processed transactions via detailed reporting.  
-  Enhances scalability with dynamic configurations and modular workflows.  
-  Saves time and minimizes human error.




