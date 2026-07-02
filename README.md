# Automated Catalog Intelligence Pipeline

An AI-powered, schema-agnostic data intelligence pipeline built on **n8n**. It ingests 
tabular data from Google Sheets, MySQL, or Excel/CSV, automatically detects the column 
structure (numeric vs. categorical) at runtime, runs multi-agent AI analysis with 
**Mistral Large**, generates data visualizations, and delivers a consolidated HTML 
report via email — with no hardcoded schema, so it adapts to any dataset without code changes.

## 🎯 What It Does

Point it at any dataset (Amazon product catalog, sales orders, survey responses, etc.) and it will:

1. **Ingest** data from Google Sheets, MySQL, or Excel/CSV
2. **Auto-detect** which columns are numeric vs. categorical — no schema assumptions
3. **Analyze** the data using three parallel AI agents (Mistral Large):
   - Statistical analysis (top segments, gap analysis)
   - Business interpretation (patterns, relationships, standout records)
   - Executive summary (plain-English briefing for leadership)
4. **Visualize** key metrics as auto-generated charts (QuickChart)
5. **Deliver** a single polished HTML report via Gmail, with charts embedded as 
   base64 images so they render reliably across email clients

## 🏗️ Architecture
<img width="1917" height="908" alt="image" src="https://github.com/user-attachments/assets/21eacbb0-f45f-4d15-81c2-2db8a5b1540f" />
<img width="1918" height="912" alt="image" src="https://github.com/user-attachments/assets/8f7e8098-1025-4e5d-ae6b-c8fb744290f8" />
<img width="1918" height="906" alt="image" src="https://github.com/user-attachments/assets/5c4e3a21-807e-4f4a-acd1-f5fa2e91d29a" />
<img width="1918" height="911" alt="image" src="https://github.com/user-attachments/assets/f29a30df-2b40-4141-b5ef-0a6702ec1388" />
<img width="1918" height="908" alt="image" src="https://github.com/user-attachments/assets/b9d8a045-d922-4d98-9090-a641c27c2979" />
<img width="1918" height="905" alt="image" src="https://github.com/user-attachments/assets/1619bdfe-3703-4cc6-b095-ec4fac780286" />
<img width="1918" height="905" alt="image" src="https://github.com/user-attachments/assets/9eb46f0f-49cf-4008-8a2c-4a69059c4ee4" />
<img width="1918" height="907" alt="image" src="https://github.com/user-attachments/assets/5ce0d60b-c19b-4450-88fc-43bdcccdb849" />


**Pipeline stages:**
Data Source (Sheets/MySQL/Excel)
→ Schema Detection & Cleaning
→ Statistical Summary Generation
→ [Parallel AI Agents: Analysis | Interpretation | Visualization]
→ Merge Outputs
→ Executive Summary Agent
→ Build Final HTML Report
→ Email Delivery

## 📊 Sample Output

![Report Sample](screenshots/report-email-sample.png)

## 🛠️ Tech Stack

- **n8n** — workflow orchestration
- **Mistral Large** — AI analysis agents
- **QuickChart** — chart generation
- **Google Sheets API** — data ingestion
- **Gmail API** — report delivery

## 🧠 Key Engineering Challenges Solved

- **Schema-agnostic data cleaning**: Instead of hardcoding field names, the pipeline 
  samples each column at runtime and classifies it as numeric or categorical, then 
  dynamically picks the most meaningful columns to analyze and visualize — so the 
  same pipeline works on completely different datasets without modification.
- **n8n expression-mode debugging**: Diagnosed and fixed a subtle bug where AI agent 
  nodes were receiving literal `{{ }}` template strings instead of resolved data, 
  because the prompt fields weren't set to expression mode — causing the LLM to 
  hallucinate placeholder responses instead of analyzing real data.
- **Gmail image rendering**: Solved silent image-loading failures by converting 
  chart URLs to base64 data URIs using n8n's HTTP helper (rather than an unreliable 
  bare `fetch()` call inside the sandboxed Code node), so visualizations render 
  immediately without requiring "display external images" permission.
- **Google Sheets cell-size limits**: Fixed a `400: max 50000 characters` error 
  caused by logging full HTML reports into spreadsheet cells — refactored to log 
  only summary metadata instead.

## 🔒 Workflow File

The full n8n workflow JSON isn't published in this repo to keep implementation 
details private. If you'd like to see it, discuss the architecture, or use it as a 
reference for your own project, feel free to reach out:

📧 **siddharthpandere@gmail.com** — mention "Catalog Intelligence Pipeline" in the subject  
🔗 **[LinkedIn](https://www.linkedin.com/in/siddharthpandere/)**
I'm happy to walk through the design or share the workflow on a case-by-case basis.

## 🚀 Setup (if you receive the workflow file)

1. Import the workflow JSON into your n8n instance
2. Add credentials: Mistral Cloud API, Gmail OAuth2, Google Sheets OAuth2
3. Point the data source node at your dataset
4. Run manually, or attach a Schedule Trigger for automated runs

---

Built by [Siddharth Pandere](https://www.linkedin.com/in/siddharthpandere/) · Final-year Computer Engineering, DBATU
