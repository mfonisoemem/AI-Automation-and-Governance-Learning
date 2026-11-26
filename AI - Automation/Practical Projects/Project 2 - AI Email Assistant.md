# Project 2 - AI Email Assistant

## 1. Project Overview

The AI Email Assistant is an n8n automation workflow that uses Google Gemini to analyse incoming emails, classify their content, generate concise summaries, identify the sender's main intention, and draft appropriate responses.

The workflow integrates Gmail with Google Gemini to reduce the time spent manually reviewing and responding to emails.

Rather than sending responses automatically, the workflow creates email drafts in Gmail for human review and approval before sending.

This approach combines AI-assisted productivity with human oversight.

## 2. Problem Statement

Managing incoming emails can become time-consuming, particularly when dealing with different categories of messages, including professional enquiries, business communications, personal messages, support requests, marketing emails, and spam.

Manually reviewing each email, understanding its purpose, and preparing an appropriate response can reduce productivity and delay communication.

There is also a risk of generating inaccurate or inappropriate responses when using AI without sufficient instructions or human oversight.

The project addresses these challenges by introducing an automated email processing workflow that uses AI to analyse incoming messages and prepare suggested responses while retaining human control over the final communication.

## 3. Project Objectives

The objectives of this project are to:

1. Automatically detect incoming emails through Gmail.
2. Retrieve the complete email content for analysis.
3. Use Google Gemini to classify incoming emails.
4. Generate concise summaries of email content.
5. Identify the sender's main request or intention.
6. Generate professional and context-appropriate suggested replies.
7. Create email drafts in Gmail without sending them automatically.
8. Maintain human oversight by requiring the user to review and approve every suggested response.
9. Reduce the time spent manually processing and responding to emails.

## 4. Tools and Technologies

| Tool | Purpose |
|---|---|
| n8n | Workflow automation and integration |
| Gmail | Receiving emails and creating response drafts |
| Google Gemini API | AI-powered email analysis and response generation |
| Google AI Studio | Generating and managing the Gemini API key |
| JavaScript | Extracting and structuring AI-generated responses |
| Docker Desktop | Running n8n locally |

## 5. Workflow Design

The workflow follows a sequential automation process:

1. **Gmail Trigger:** Detects incoming emails.
2. **Edit Fields:** Extracts and structures the initial email information.
3. **Gmail – Get a message:** Retrieves the complete email content.
4. **Edit Fields1:** Organises the email body, subject, sender, date, and email ID.
5. **Google Gemini – Message a model:** Analyses the email and generates a classification, summary, main request, and suggested reply.
6. **Edit Fields2:** Extracts the AI-generated response into separate structured fields.
7. **Gmail – Create a draft:** Creates a reply draft in Gmail for human review.

The workflow is designed to process emails individually and preserve human approval before any response is sent.

## 6. Workflow Implementation

### 6.1 Gmail Trigger

The Gmail Trigger node was configured with the Message Received event to detect incoming emails.

The Gmail account was connected to n8n using Google OAuth authentication.

The trigger captures essential email information, including:
- Email ID
- Thread ID
- Sender
- Subject
- Email snippet

### 6.2 Initial Edit Fields

An Edit Fields node was added to extract and organise the information received from the Gmail Trigger.

The following fields were configured:

- `sender`
- `subject`
- `email_body`
- `received_at`
- `email_id`

### 6.3 Retrieve Complete Email Content

A Gmail Get a message node was added to retrieve the full content of the incoming email.

The email ID from the previous node was used to identify the specific message.

The Simplify option was disabled to expose the complete message data, including the readable plain-text email body.

### 6.4 Structure Email Information

A second Edit Fields node was configured to prepare the complete email information for AI analysis.

The fields included:

- `email_body`
- `email_subject`
- `sender`
- `received_at`
- `email_id`

The plain-text email content was passed to Gemini rather than the HTML version.

### 6.5 Google Gemini AI Analysis

The Google Gemini node was configured using the Message a Model operation and the Gemini 3.8 Flash model available in the n8n model selection.

A prompt was created to instruct Gemini to perform four tasks:

1. Classify the email as Work, Business, Personal, Support, Marketing, or Spam.
2. Provide a concise summary.
3. Identify the sender's main request or intention.
4. Draft a professional and appropriate reply.

Additional response instructions were included to reduce unsupported assumptions and prevent the AI from inventing qualifications, experience, availability, commitments, or personal information.

The assistant was also instructed to use professional links only when relevant and to sign off with the name Mfoniso Emem.

### 6.6 Extract AI-Generated Responses

A third Edit Fields node was added after Gemini to extract the generated response into separate fields.

The following fields were configured:

- `ai_response`
- `classification`
- `summary`
- `main_request`
- `suggested_reply`

JavaScript expressions were used to extract the relevant sections from Gemini's response.

### 6.7 Create Gmail Draft

The final node was configured using the Gmail Draft resource and Create operation.

The following information was mapped:

- **Recipient:** Original email sender.
- **Subject:** Original email subject.
- **Thread ID:** Original email thread.
- **Message:** AI-generated suggested reply.

The workflow creates a draft in Gmail rather than sending the response automatically.

## 7. Workflow Execution and Testing

The workflow was tested using different sample emails to evaluate how the AI assistant handles different communication contexts.

### Test 1: Business Enquiry

A sample business enquiry was sent asking about project management services, professional experience, and availability to coordinate a software development project.

**Expected behaviour:**
- Identify the email as a professional or business enquiry.
- Summarise the sender's request.
- Generate an appropriate professional response.
- Include relevant professional links where useful.
- Avoid inventing availability or commitments.

**Result:** The assistant generated a professional response, included relevant LinkedIn and portfolio links, and requested additional project details before discussing potential availability.

### Test 2: Personal Email

A personal message was sent by someone checking in and asking about recent activities.

**Expected behaviour:**
- Recognise the personal nature of the message.
- Generate a warm and natural response.
- Avoid unnecessary professional links.

**Result:** The assistant generated a friendly response and used only the user's name in the sign-off.

### Test 3: Professional Meeting Request

A sample email was sent requesting a meeting to discuss project progress, deliverables, outstanding tasks, risks, and the next project phase.

**Expected behaviour:**
- Recognise the professional context.
- Acknowledge the meeting request.
- Generate an appropriate response.
- Avoid inventing the user's availability.

**Result:** The assistant generated a suitable response and used placeholders for the meeting date and time rather than making unsupported scheduling commitments.

### Overall Testing Result

All three test scenarios successfully produced suggested responses, and Gmail drafts were created for review.

The workflow demonstrated that it could adapt its response style to different email contexts while retaining human oversight.

## 8. Challenges and Resolutions

### 8.1 Gmail API Authentication

**Challenge:** Connecting the Gmail account to n8n required Google OAuth configuration.

**Resolution:** A Google Cloud project was configured, the Gmail API was enabled, and OAuth credentials were created and connected to n8n.

### 8.2 Retrieving the Complete Email Body

**Challenge:** The initial Gmail Trigger output provided a snippet rather than the complete email body.

**Resolution:** A Gmail Get a message node was introduced, and the Simplify option was disabled to expose the full email content.

### 8.3 Gemini Model Availability

**Challenge:** The initially selected Gemini 2.5 Flash model returned a model availability error.

**Resolution:** The model was changed to Gemini 3.8 Flash, which was available in the n8n model selection and executed successfully.

### 8.4 Temporary Gemini API Errors

**Challenge:** Some requests returned temporary service-unavailable errors.

**Resolution:** The Gemini node's Retry On Fail setting was enabled, with Max Tries set to 3 and Wait Between Tries set to 1000 milliseconds.

### 8.5 Inaccurate AI-Generated Assumptions

**Challenge:** An initial business response included unsupported claims about accepting projects and having attachments.

**Resolution:** Additional instructions were added to the Gemini prompt to prevent invented qualifications, availability, commitments, achievements, and attachments.

### 8.6 Extracting Structured AI Responses

**Challenge:** The generated response needed to be separated into individual fields for the Gmail Draft node.

**Resolution:** JavaScript expressions were configured in the third Edit Fields node to extract the classification, summary, main request, and suggested reply. The expressions were also adjusted to recognise headings with or without Markdown bold formatting.

## 9. Project Outcomes and Business Value

The project demonstrates how AI and workflow automation can improve email management and communication efficiency.

Key outcomes include:

- Automated detection and retrieval of incoming emails.
- AI-powered email classification and summarisation.
- Identification of the sender's main request or intention.
- Context-aware suggested email responses.
- Automatic creation of Gmail drafts.
- Reduced need for repetitive manual email processing.
- Human review and approval before responses are sent.

From a business perspective, this workflow provides a practical example of using AI to support administrative productivity while maintaining control over external communication.

It also demonstrates the integration of workflow automation, APIs, AI prompting, data transformation, and email services.

## 10. Workflow Screenshots and Resources

### Workflow Screenshot

![Project 2 - AI Email Assistant Workflow](project-2-workflow.png)

### Resources

- [n8n](https://n8n.io/)
- [Google AI Studio](https://aistudio.google.com/)
- [Gemini API Documentation](https://ai.google.dev/gemini-api/docs)

