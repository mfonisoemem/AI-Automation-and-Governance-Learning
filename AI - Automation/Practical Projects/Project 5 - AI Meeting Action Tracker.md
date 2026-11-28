## Project 5 - AI Meeting Action Tracker

1. Project Overview
The AI Meeting Action Tracker is an AI-powered workflow automation built with n8n to simplify meeting documentation and action-item tracking.
The workflow accepts meeting details and transcripts through an online form, analyses the submitted information using Google Gemini, and automatically generates structured meeting minutes and individual action items.
The extracted action items are saved to Google Sheets, while the formatted meeting minutes are delivered through Gmail.
The goal is to reduce the manual effort involved in reviewing meeting transcripts, preparing minutes, identifying responsibilities and documenting follow-up activities.

2. Project Objectives
- Automate the conversion of meeting transcripts into structured meeting minutes.
- Extract key discussions, decisions, issues, risks and next steps.
- Identify action items, responsible persons, deadlines and priorities.
- Store extracted action items in a centralised Google Sheets register.
- Automatically generate and email formatted meeting minutes.
- Improve meeting documentation, accountability and follow-up.

## 3. Tools and Technologies
| Tool | Purpose |
|---|---|
| n8n | Workflow automation and orchestration |
| Google Gemini 3 Flash | Analyses meeting transcripts and generates structured meeting minutes and action items |
| Google Sheets | Stores extracted action items in a structured register |
| Gmail | Delivers formatted meeting minutes via email |
| JavaScript | Parses the AI-generated JSON response and converts meeting minutes into HTML |
| HTML | Structures meeting minutes for email presentation |


## 4. Workflow Architecture
The workflow consists of two main processing branches after the AI analysis and JSON parsing stages.

### Main Workflow
1. **Form Trigger:** Captures meeting information and the meeting transcript submitted through the n8n form.
2. **Google Gemini – Message a Model:** Analyses the meeting information and extracts structured meeting minutes and action items.
3. **Code (JavaScript):** Parses the JSON response from Gemini into structured data.
4. **Split Out:** Separates the action items array into individual records.
### Branch 1: Action Item Tracking
5. **Google Sheets – Append Row:** Saves each extracted action item as an individual row in the Action Items spreadsheet.
### Branch 2: Meeting Minutes Generation
5. **Edit Fields:** Extracts the structured meeting minutes object.
6. **Code (JavaScript):** Converts the meeting minutes into formatted HTML.
7. **Gmail – Send a Message:** Sends the formatted meeting minutes to the designated email address.
The two branches operate from the same parsed AI response, allowing the workflow to generate meeting minutes and record action items from a single form submission.

## 5. Form Input Structure

The workflow begins with an n8n Form Trigger that collects meeting information and the transcript or notes for analysis.
The form contains the following fields:
| Field | Input Type | Description |
|---|---|---|
| Meeting Title | Text | Name or subject of the meeting |
| Meeting Category | Dropdown | Classification of the meeting |
| Project Name / Department | Text | Associated project or department |
| Meeting Date | Date | Date the meeting took place |
| Start Time | Text | Meeting start time |
| End Time | Text | Meeting end time |
| Meeting Chair / Facilitator | Text | Person leading the meeting |
| Attendees | Text | Names of meeting participants |
| Meeting Transcript / Notes | Textarea | Meeting discussions, updates, decisions and follow-up commitments |
The submitted information provides the context Gemini needs to analyse the meeting accurately and generate the required outputs.

The form supports different meeting categories, including project meetings, team meetings, product demos, client meetings, planning meetings, review meetings and technical meetings.

## 6. AI Processing and Information Extraction
Google Gemini 3 Flash processes the meeting information and transcript submitted through the form.
The AI is instructed to return a structured JSON response containing two main components:
### A. Meeting Minutes
The workflow extracts the following information:

- Meeting overview, including title, category, project, date, time, facilitator and attendees.
- Meeting objective.
- Agenda.
- Meeting summary.
- Key decisions.
- Issues and risks.
- Next steps.

### B. Action Items
For every identified action item, Gemini extracts:

| Field | Description |
|---|---|
| Action ID | Unique sequential identifier, e.g. ACT-001 |
| Task | Specific activity or expected deliverable |
| Responsible Person | Person assigned to the task |
| Deadline | Stated or identifiable target date |
| Priority | High, Medium or Low |
| Status | Initial task status, usually Not Started |

### Information Accuracy Rules

The AI is instructed to:
- Use only information provided in the meeting details and transcript.
- Avoid inventing decisions, responsibilities, deadlines or commitments.
- Distinguish confirmed decisions from suggestions and proposals.
- Use "Not Specified" when an action owner or deadline cannot be established.
- Identify dependencies and potential project risks where supported by the discussion.
The structured JSON response makes it possible for subsequent n8n nodes to process meeting minutes and individual action items separately.

## 7. Action Item Tracking with Google Sheets

After Gemini generates the structured response, a JavaScript Code node parses the JSON output into usable data.
The Split Out node then separates the `action_items` array into individual records.
Each record is passed to Google Sheets, where the Append Row operation creates a new row in the Action Items worksheet.
### Google Sheets Register
The spreadsheet contains the following columns:

| Column | Description |
|---|---|
| Meeting Title | Meeting associated with the action |
| Project / Department | Relevant project or department |
| Action ID | Unique action identifier |
| Task | Action item or expected deliverable |
| Responsible Person | Assigned individual |
| Deadline | Target completion date |
| Priority | High, Medium or Low |
| Status | Current action status |

This creates a centralised record of meeting commitments, making it easier for a Project Manager to review assigned responsibilities, deadlines and priorities.
Each new form submission adds its extracted action items to the existing register.

## 8. Challenges and Resolutions

During the development and testing of the workflow, a few challenges were encountered and resolved.
| Challenge | Resolution |
|---|---|
| Gemini initially returned its response in an unstructured Markdown format instead of the required JSON. | Updated the AI prompt with explicit JSON-only output instructions and a defined response structure. |
| The AI response was returned as a JSON string, making its contents difficult for subsequent nodes to access individually. | Used a JavaScript Code node with `JSON.parse()` to convert the response into structured data. |
| Multiple action items were returned within a single array. | Implemented the Split Out node to separate the action items into individual records before saving them to Google Sheets. |
| Meeting minutes needed to be presented in a readable email format rather than as raw structured data. | Used a JavaScript Code node to generate HTML from the meeting minutes object. |
| The email needed to render the formatted meeting minutes correctly. | Configured the Gmail node to send the generated HTML content using the HTML email type. |

These resolutions enabled the workflow to process meeting transcripts, generate structured outputs, save individual action items and deliver formatted meeting minutes successfully.

## 9. Testing and Results
The workflow was tested using sample meeting transcripts from two different project scenarios:
1. Website Redesign Progress Review.
2. Weekly Project Team Stand-up for a Mobile Banking Application.
### Test Results
| Test Area | Result |
|---|---|
| Meeting information submission | Successful |
| AI meeting analysis | Successful |
| Meeting minutes generation | Successful |
| Action item extraction | Successful |
| Responsible person identification | Successful |
| Deadline and priority extraction | Successful |
| Individual action item storage in Google Sheets | Successful |
| HTML meeting minutes generation | Successful |
| Email delivery through Gmail | Successful |

### Outcome
Both test submissions were processed successfully.
The workflow generated structured meeting minutes, extracted individual action items and saved them to the Google Sheets register. The formatted meeting minutes were also delivered successfully through Gmail.
The end-to-end test confirmed that both workflow branches operated as expected from a single form submission.

## 10
![Project 5 - AI Meeting Action Tracker](Project5-workflow.png)
