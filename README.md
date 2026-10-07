# linux-log-filter-demo

# Linux Log Filtering & Access Auditing: `grep` and Piping

## Project Overview
This repository contains documentation from a practical lab completed as part of the **Google Cybersecurity Certificate**. The objective was to utilize advanced Linux command-line filtering techniques—specifically the `grep` command and piping (`|`)—to extract critical information from server logs and user data files. 

In enterprise environments, security analysts and compliance teams cannot manually read through thousands of log entries. Mastery of text filtering is essential for rapid incident response, targeted log extraction, and efficient access control auditing.

## Key Skills & Tools Demonstrated
* **Environment:** Qwiklabs Virtual Machine (Linux Bash Shell)
* **Log Parsing:** `grep` (Global Regular Expression Print)
* **Command Chaining:** Piping (`|`) standard output to standard input
* **Core Competencies:** Incident triage, audit trail verification, absolute path execution, user lifecycle auditing, and string matching.

---

## Lab Execution & Operational Walkthrough

### 1. Triaging Server Errors
During an incident investigation, filtering out the noise is critical. I utilized `grep` to isolate and extract only the system error events from the master log file, successfully bypassing standard info and warning messages.
```bash
analyst@24cef853c299:~/logs$ ls
server_logs.txt
analyst@24cef853c299:~/logs$ grep error server_logs.txt
2022-09-28 13:56:22 error   The password is incorrect
2022-09-28 15:56:22 error   The username is incorrect
2022-09-28 16:56:22 error   The password is incorrect
2022-09-29 13:56:22 error   An unexpected error occurred
2022-09-29 15:56:22 error   Unauthorized access
2022-09-29 16:56:22 error   Unauthorized access
```

### 2. Locating Targeted Compliance Reports (Absolute Path Execution)
To audit quarterly access controls, I needed to locate specific user files. Instead of navigating through the directory tree, I queried the directory remotely using an absolute path, piping the `ls` output directly into `grep`. This filtered the directory to show only the files relevant to Quarter 1 (`Q1`) and files associated with `access`.
```bash
analyst@24cef853c299:~/logs$ ls /home/analyst/reports/users | grep Q1
Q1_access.txt
Q1_added_users.txt
Q1_deleted_users.txt
analyst@24cef853c299:~/logs$ ls /home/analyst/reports/users | grep access
Q1_access.txt
Q2_access.txt
Q3_access.txt
Q4_access.txt
```

### 3. Auditing User Lifecycle Events
Investigating insider threats and maintaining compliance requires verifying specific user modifications. After navigating to the target directory and listing the available reports, I used targeted `grep` searches to query a specific employee username (`jhill`) against the deleted users log. I also utilized string encapsulation to audit all users added to the `"Human Resources"` department in Quarter 4.
```bash
analyst@24cef853c299:~$ cd /home/analyst/reports/users
analyst@24cef853c299:~/reports/users$ ls
Q1_access.txt       Q2_access.txt       Q3_access.txt       Q4_access.txt
Q1_added_users.txt  Q2_added_users.txt  Q3_added_users.txt  Q4_added_users.txt
Q1_deleted_users.txt Q2_deleted_users.txt Q3_deleted_users.txt Q4_deleted_users.txt
analyst@24cef853c299:~/reports/users$ grep jhill Q2_deleted_users.txt
1025        jhill   Sales
analyst@24cef853c299:~/reports/users$ grep "Human Resources" Q4_added_users.txt
1151        sshah   Human Resources
1145        msosa   Human Resources
```

---

## Result & Business Impact
* **Rapid Incident Triage:** Demonstrated the ability to instantly isolate critical server errors from vast log files using `grep`, drastically reducing the time required to identify system failures and unauthorized access attempts.
* **Streamlined Auditing:** Successfully utilized piped commands and absolute paths to filter remote directory contents, proving operational efficiency in locating specific compliance and access control reports.
* **Precise Access Verification:** Extracted exact user lifecycle events (additions and deletions) by department and username, a fundamental process for enforcing identity and access management (IAM) policies and passing security audits.
