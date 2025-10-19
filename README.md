🧠 AI-Powered Creative QC via N8N + Basecamp
Automating Creative Quality Check (QC) with AI + Basecamp + N8N
🚀 Project Overview

This project automates the Creative Quality Control (QC) process for a digital agency using N8N, Basecamp, and Google Gemini AI.

Whenever a creative (image, video, or document) is uploaded or marked as “Ready for QC” in Basecamp, this workflow automatically:

Detects the new creative via Basecamp webhook.

Fetches the creative asset (image/video/document).

Classifies its type (image, video, or document).

Sends it to Google Gemini AI for automated QC analysis.

Logs the AI-generated QC report in a Google Sheet for tracking.

⚙️ Workflow Summary
1️⃣ Basecamp Webhook Trigger

Listens for new uploads or tasks marked “Ready for QC.”

Trigger endpoint: /basecamp-qc-trigger

Example payload:

{
  "body": {
    "recording": {
      "id": 12345,
      "type": "Upload",
      "url": "https://basecamp.com/api/file",
      "bucket": { "id": 98765 }
    },
    "kind": "upload_active"
  }
}

2️⃣ Parse Webhook Data (Function Node)

Extracts essential data:

projectId

commentableId

assetApiUrl
Ensures payload integrity and logs missing data safely.

3️⃣ Fetch Asset (HTTP Request)

Retrieves creative metadata and download URL from Basecamp API using OAuth2 credentials.

4️⃣ AI Asset Type Classifier (LangChain Agent)

Uses an AI Agent to classify the asset into one of:

Image

Video

Document

The output determines which QC branch to execute next.

5️⃣ Switch Node (Router)

Routes the workflow based on classification:

Image → “Analyze an image” node

Video → “Analyze video” node

Document → “Analyze document” node

6️⃣ Google Gemini Analysis Nodes

Each branch uses Gemini 2.5 Pro for specialized AI-based QC.

🖼️ Image QC

Evaluates:

Logo visibility

Colors (Brand Navy #1A2B4C, Brand Gold #FFD700)

Typography

Grammar

Margins and layout

Readability and platform fit

🎥 Video QC

Evaluates:

Logo appearance

Grammar and subtitles

Margins/safe zones

Colors and smooth motion

📄 Document QC

Evaluates:

Logo presence

Grammar and typography

Brand colors

Page margins and layout

All QC results follow a strict structured report format, for example:

QC Report:
Logo: Pass  
Colors: Pass  
Typography: Needs Review  
Grammar: Pass  
Margins: Fail  
Readability: Pass  
Platform Fit: Pass  
Other Notes: Text too close to edge

7️⃣ Append to Google Sheets

Each QC result is logged into a Google Sheet for reporting and tracking.

Appends one new row per processed creative.

Column: RESULT → contains the full QC report.

🧰 Tools & APIs Used
Tool	Purpose
N8N	Workflow automation engine
Basecamp API	Source of creative assets & triggers
Google Gemini (PaLM)	AI model for QC evaluation
LangChain Agent	File-type classification logic
Google Sheets API	Stores and tracks QC results
🧩 Workflow Diagram
(Basecamp Webhook)
        ↓
(Parse Webhook Data)
        ↓
(HTTP Request - Get Asset)
        ↓
(AI Agent - File Type)
        ↓
      ┌──────────────┬──────────────┬──────────────┐
      │ Image QC     │ Video QC     │ Document QC  │
      ↓              ↓              ↓
 (Gemini Analysis)  (Gemini Analysis)  (Gemini Analysis)
        ↓
(Append Result to Google Sheets)

⚡ Key Features

✅ End-to-end automation — from upload to AI QC report
✅ Multimodal AI — analyzes text, images, and videos
✅ Basecamp integrated — works directly with project assets
✅ Auto-logging — results stored neatly in Google Sheets
✅ Error-safe design — skips gracefully on missing data

📁 Project Structure
📂 ai-qc-workflow
 ┣ 📜 README.md
 ┣ 📜 n8n-workflow.json
 ┣ 🖼️ sample-creative.png
 ┗ 📄 docs/
    ┗ setup_guide.md

🧠 Assumptions

Basecamp OAuth credentials are pre-configured in N8N.

Gemini API keys are valid and active.

Google Sheet permissions allow appending rows.

If asset URL or AI response is missing → workflow stops gracefully.

🧪 How to Run

Import JSON:

Go to your N8N editor → Import Workflow JSON → upload n8n-workflow.json

Set Credentials:

Basecamp OAuth2

Google Gemini API

Google Sheets API

Activate Workflow:

Deploy and get the webhook URL

Configure Basecamp to send webhook events to this URL

Test:

Upload a creative to Basecamp or mark a task as “Ready for QC”

Watch results populate in your linked Google Sheet

📸 Example Output
RESULT
Logo: Pass ✅ • Colors: Pass ✅ • Grammar: 1 typo ❌ • Margins: Tight ⚠️ • Readability: Pass ✅
👥 Contributors

Pavan Anish — Workflow Developer
Bazil Salim — Collaborator, Blankspace Community

🏆 Hackathon Recognition

🥈 2nd Prize — n8n Hackathon 2025
Built under the Blankspace Community, focusing on AI + Automation innovation.
