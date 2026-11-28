# Project 6 - AI Risk and Issue Management

## 1. Project Overview
The AI Risk and Issue Management workflow is an AI-powered project management automation built with n8n to support the identification, assessment and tracking of project risks and issues.
The workflow allows a Project Manager to manually submit a risk or issue through a form. The submitted information is analysed using Google Gemini, which helps classify the entry, assess its potential impact and urgency, recommend mitigation or resolution actions, and assign a priority level.
The assessment is then stored in a centralised Google Sheets register for review and monitoring.
The solution is designed to improve risk visibility, support timely decision-making and reduce the manual effort involved in maintaining project risk and issue records.

## 2. Problem Statement
Project risks and issues can arise at any stage of project delivery. When they are not properly documented, assessed and monitored, they can lead to delays, increased costs, reduced quality and difficulties in achieving project objectives.
Maintaining risk and issue registers manually can also become time-consuming, particularly when a Project Manager needs to assess the severity of multiple items, identify appropriate responses and keep stakeholders informed.
Without a consistent assessment process, risks and issues may be prioritised differently or important warning signs may be overlooked.
This project addresses these challenges by introducing an AI-assisted workflow that supports structured risk and issue assessment, recommends response actions and maintains a centralised register for monitoring and follow-up.
The AI provides decision-support recommendations, while the Project Manager remains responsible for reviewing assessments and deciding on the appropriate response.

## 3. Project Objectives

The objectives of this project are to:
- Automate the capture and documentation of project risks and issues.
- Use AI to classify risks and issues into relevant categories.
- Assess probability, impact and overall severity for identified risks.
- Evaluate the urgency and potential consequences of reported issues.
- Generate recommended mitigation strategies for risks and resolution actions for issues.
- Assign priority levels to support effective decision-making.
- Maintain a centralised risk and issue register using Google Sheets.
- Support timely monitoring, escalation and follow-up of outstanding risks and issues.
- Improve project visibility and strengthen risk management practices.

## 4. Tools and Technologies

| Tool | Purpose |
|---|---|
| n8n | Workflow automation and orchestration |
| Google Gemini 3 Flash | AI-powered risk classification, assessment and response recommendations |
| Google Sheets | Centralised storage and tracking of risks and issues |
| Gmail | Automated notifications and escalation alerts |
| JavaScript | Data processing and transformation where required |

## 5. Workflow Design
The automation is designed to capture project risks and issues, analyse submitted information using AI, and maintain a structured register for monitoring and follow-up.
The workflow will consist of the following stages:
1. **Risk and Issue Submission:** A Project Manager manually submits a risk or issue through an n8n form.
2. **Data Processing:** The submitted information is prepared for AI analysis.
3. **AI Assessment:** Google Gemini analyses the entry, identifies its category, evaluates its severity and priority, and recommends appropriate response actions.
4. **Assessment Review:** The generated assessment is made available for review by the Project Manager.
5. **Register Update:** The risk or issue and its assessment are saved in Google Sheets.
6. **Notifications and Escalations:** Relevant notifications are sent to support timely attention to high-priority risks, issues and outstanding actions.
7. **Scheduled Monitoring:** The workflow is designed to support periodic reviews of open risks, unresolved issues and pending mitigation or resolution actions.
The Project Manager retains responsibility for validating AI recommendations and making final risk and issue management decisions.
