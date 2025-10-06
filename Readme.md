n8n AI-Powered Feedback Responder
Objective
The primary objective of this project is to create a simple and effective automation that instantly processes user feedback submitted through a web form. It uses a Large Language Model (LLM) to classify the user's comment, draft a personalized and professional response, and automatically email that reply back to the user. This ensures timely communication and enhances user engagement without manual intervention.

Workflow Description
This n8n workflow provides an end-to-end solution for handling customer feedback. The process is as follows:

Form Submission: The workflow is triggered when a user submits their Name, Email, and Comment via a public n8n Form Trigger.

AI Processing: The submitted comment is sent to a Google Gemini model. The AI classifies the comment as "Feedback," "Complaint," or "Suggestion" and drafts a complete email reply from the perspective of the company, "MysterioSocialConnector".

Automated Email Reply: The workflow takes the AI-generated draft and automatically sends it as an email to the user who filled out the form.

Features
Automated Form Handling: Generates a public form to capture user feedback.

AI-Powered Classification: Intelligently categorizes user comments.

Dynamic Email Drafting: Creates context-aware, professional email replies.

End-to-End Automation: No manual steps are required from submission to response.

Nodes Used
Form Trigger: To capture user input.

Google Gemini: For AI-based classification and text generation.

Send Email: To dispatch the automated reply.

Setup and Configuration
To use this workflow, follow these steps:

1. Import Workflow
Import the Smart Router.json file into your n8n instance.

2. Configure Credentials
Google Gemini: In the Message a model node, create a new credential using your Google Gemini API key and connect it.

SMTP: In the Send email node, create a new credential with your SMTP provider's details (Host, Port, User, Password) and connect it.

3. (CRITICAL) Update the Gemini and Email Nodes
The original workflow does not correctly send the AI's response. You must make the following changes.

A. Improve the AI Prompt for Cleaner Output
To ensure you can easily separate the subject and body for the email, update the prompt in the Message a model node. Change the final instruction in the prompt from:

Please output a JSON object with:
category: (...)
email_draft: (...)

to this:

Please output a JSON object with three keys: "category", "email_subject", and "email_body".

This will make the AI's output much easier and more reliable to work with.

B. Configure the "Send email" Node
Update the fields in the Send email node to use expressions that pull data from the previous nodes.

From Email:

your-sender-email@example.com

To Email (Expression):

{{ $('On form submission').first().json["Email address"] }}

Subject (Expression):

{{ $('Message a model').first().json.json.email_subject }}

Text (Expression):

{{ $('Message a model').first().json.json.email_body }}

4. Activate Workflow
Save the workflow and toggle the switch at the top right to Active.

How to Use
Once the workflow is active:

Click on the On form submission node to get the Production URL.

Use this URL as the action/endpoint for your live website form, or share the link directly with users.

Any submission to this form will now trigger the complete, automated AI response.
