# Project 1: Project Task Generator

## 1. Project Overview

The Project Task Generator is a workflow automation project designed to simplify how project tasks are created, processed, identified, and recorded.

The workflow uses n8n to capture task information, process the submitted data, generate a unique task ID and timestamp, and store the resulting task record in Google Sheets.

The project demonstrates how workflow automation can support everyday Project Management activities by reducing repetitive manual data entry and improving task record management.

## 2. Problem Statement

In many project environments, task information is manually created, organised, and transferred into project trackers or spreadsheets.

This process can become repetitive and time-consuming, particularly when managing multiple tasks across different teams. Manual data entry can also result in inconsistent records, missing information, and difficulties tracking individual tasks.

There is therefore a need for a simple automated process that can capture task details, organise the information, generate unique task identifiers, and store records in a central location.

## 3. Project Objectives

The objectives of this project are to:

- Automate the creation and recording of project tasks.
- Capture essential task information, including task name, assignee, priority, and status.
- Generate a unique identifier for each task.
- Automatically record the date and time each task is created.
- Store task information in Google Sheets.
- Reduce repetitive manual data entry.
- Demonstrate how n8n can be used to automate routine Project Management processes.

## 4. Tools and Technologies

**n8n** : Workflow automation platform used to design and execute the automation. 
**Docker Desktop**:  Runs the self-hosted n8n environment locally. 
**JavaScript**  Processes task information and generates task identifiers and timestamps. 
**Google Sheets** Stores and organises the generated task records. 
**GitHub** Documents the project, learning process, implementation, and outcomes. 


## 5. Workflow Design

### Workflow Implementation

The workflow was built in n8n using six nodes:

1. **Manual Trigger:** Starts the workflow manually for testing and execution.
2. **Edit Fields:** Captures the task information, including task name, assigned person, priority, status, project name, start date, due date, estimated hours, task progress, approval status, and review requirement.
3. **Google Sheets – Get Row(s):** Retrieves existing task records from the Google Sheet to check previously generated task IDs.
4. **Merge:** Combines the new task information with the existing spreadsheet records using Append mode.
5. **Code (JavaScript):** Processes the incoming records, identifies the highest existing task ID, generates the next sequential ID, and adds a creation timestamp.
6. **Google Sheets – Append Row:** Saves the completed task record as a new row in the PM Task Tracker spreadsheet.

### Workflow Execution and Testing

The workflow was executed multiple times to verify that task information was processed and stored correctly.

The following tests were performed:

- **Task creation:** Confirmed that task details were successfully captured and saved to Google Sheets.
- **Sequential task IDs:** Verified automatic ID generation, progressing from PM-001 to PM-006.
- **Timestamp generation:** Confirmed that each task received an automatically generated creation timestamp.
- **Task status:** Tested Not Started, In Progress, and Done.
- **Boolean fields:** Tested true and false values for the approval and review fields.
- **Google Sheets integration:** Confirmed that each execution appended a new task record without overwriting previous records.

All tests were successful.

### Challenges and Resolutions

During implementation, a few issues were encountered:

1. **Google Sheets API configuration:** The spreadsheet was initially unavailable in n8n because the Google Sheets API had not been enabled in the Google Cloud project. This was resolved by enabling the API and reconnecting to the spreadsheet.

2. **Field name mismatch:** A capitalization difference between `Priority` and `priority` caused the priority value to appear in a separate spreadsheet column. This was resolved by standardising the field name.

3. **Missing field values:** Some task fields were initially missing from the Edit Fields configuration. They were added and tested successfully.

4. **Sequential task ID generation:** The initial JavaScript implementation used a fixed task ID. This was improved by retrieving existing spreadsheet records, merging the data, and using JavaScript to generate the next available sequential ID.

### Project Outcomes and Business Value

The project successfully demonstrated how n8n can automate basic project task registration and record management.

Key outcomes include:

- **Centralised task records:** Task information is stored in Google Sheets, providing a single location for task records.
- **Automated task identification:** Each new task receives a sequential ID, making records easier to identify and track.
- **Consistent record keeping:** Creation timestamps are generated automatically.
- **Improved task visibility:** Task owners, priorities, statuses, deadlines, estimated hours, and approval requirements are captured in a structured format.
- **Reduced manual data entry:** The workflow automates task registration and spreadsheet updates.

The project provided practical experience with workflow automation, JavaScript, data processing, and Google Sheets integration using n8n.

## 10. Workflow Screenshots and Resources
The following screenshots will be included to demonstrate the completed implementation:

Full n8n workflow showing the six connected nodes.

Code node showing the JavaScript logic for sequential task ID generation.

PM Task Tracker Google Sheet showing the generated task records.

Project Resource:
https://docs.google.com/spreadsheets/d/1VTwpv_JJeK7DgNxkibar6M4zsyi_njPs9b8_IHm1Vaw/edit?gid=0#gid=0

![Project 1 - Project Task Generator](project-1-Workflow.png)

