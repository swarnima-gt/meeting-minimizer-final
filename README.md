# Meeting Minimizer Suite

## 📝 Overview
The **Meeting Minimizer Suite** is an autonomous agent system designed to reduce time spent in Agile ceremonies (standups, retros, sprint planning, and sprint reviews).  
Instead of relying on long synchronous meetings, the suite leverages **agentic AI** to produce structured, actionable artifacts — summaries, blockers, sprint goals, and executive-friendly decks — directly from raw team inputs.  

By automating repetitive ceremonies, teams can save **30–40% of their meeting time**, improve consistency, and ensure traceable, high-quality outputs that integrate into enterprise workflows.

---

## ⚙️ Tools and Services Used
The solution is built using the mandatory **Agentic AI stack** along with core AWS services:

- **Amazon Bedrock**:  
  - Primary reasoning engine (Claude 3 Sonnet) for generating summaries, retros, sprint plans, and reviews.  
- **AgentCore** (deployment target):  
  - Designed for isolated, scalable agent runtime. The current prototype runs in CloudShell, but architecture is AgentCore-ready.  
- **Strands SDK (concepts simulated)**:  
  - Provides orchestration patterns. Here, the orchestrator coordinates agents and maintains state.  
- **MCP (Model Context Protocol) (simulated)**:  
  - Agents publish MCP-style events (`standup.summary.ready`) to demonstrate communication.  
- **Amazon S3**:  
  - Secure storage for artifacts (JSON, Markdown, PPTX decks).  
- **Python (CloudShell runtime)**:  
  - Glue code for orchestration and exporting results.  
- **python-pptx**:  
  - Builds an executive-ready PowerPoint deck from outputs.  

---

## 🚀 Setup Instructions (AWS CloudShell)

1. **Clone or upload the repo**
   ```bash
   cd ~/environment
   # if repo zipped, upload and unzip here

2. **Create python virtual environment**
 ```bash
cd meeting-minimizer-starter
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

3. **Run the demo**
 ```bash
python src/runner.py demo-all --input data/inputs/standup/smaple.json

4. **Inspect outputs**
 ```bash
cat outputs/standup_*.json | head -n 80
cat outputs/events.log | tail -n 3
cat outputs/retro_*.json | head -n 40
cat outputs/planner_*.json | head -n 40
cat outputs/review_*.md | head -n 40

5. **Build executive deck**
 ```bash
python src/exporters/make_deck.py

6. **Verify in S3**
 ```bash
aws s3 ls s3://mm-suite-artifacts/outputs/

## Domain Alignment
**Hackathon Domain:** Service Delivery Excellence
**Problem Addressed:** Time lost in Agile ceremonies, inconsistent documentation and meeting fatigue
**Value:**
- Speeds delivery due to reduction in time spent on agile ceremonies leading to increased productivity
- Provides consistent, structured outputs across teams and projects
- Integrates seamlessly with enterprise tools like slack, Jira, Confluence
- Accelerated delivery cycles and reduced coordination overhead

## Security Considerations
- IAM least privilege: Only bedrock:InvokeModel and scoped s3:PutObject, s3:GetObject, s3:ListBucket permissions
- All outputs stored in a dedicated s3 bucket (mm-suite-artifacts)
- Session isolation by ensuring each agent run creates its own timestamped outputs
- Secrets management: No credentials are hard-coded; relies on IAM roles/environment variables
