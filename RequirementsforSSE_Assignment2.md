# CYBR/CSCI 8420, Fall 2022: Assignment 2 - Requirements for SSE


Scenario
-
Our scenario represents a privately-owned bank located in a small, Nebraskan town (<500 residents). Most of the bank's clientele are local farmers and ranchers, making loans very popular. The bank staff includes a President/Owner, Vice-President/Bank Manager, Loan Manager, 1 full-time Senior Teller, and 3 part-time tellers. The information stored in the Nextcloud application must be compliant with US federal banking regulations (e.g., PCI DSS, GLBA, etc.).


Assignment Overview
-
This document will review five essential interactions between our hypothetical bank staff and the Nextcloud application.


Essential Interaction #1
-

### User Story

- As a Senior Bank Teller, I want to schedule a meeting with the part-time tellers so that we can review a recent policy change to our money handling policy.

### System of Interest

- Calendar system

### Security Requirements

- Meeting details must remain private in transit over the network
- Adding participants should not affect system performance or cause an error displaying sensitive information
- Prevent malicious content such as Javascript, HTML, or external URLs from being entered in meeting details  

![JSAssignment2](https://github.com/unosec/project/blob/main/images/JSAssignment2.png).

Essential Interaction #2
-

### System of Interest

- NextCloud Files Feature

### Security Requirements

- Anti-virus scans of files
- Two-Factor Authentication
- Role Based Access Controls 
- Audit logs of changes and actions

![JSAssignment2](https://github.com/unosec/project/blob/main/images/FileManagementMisUseCase.drawio.png).
