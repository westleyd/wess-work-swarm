# CrewAI Local Setup Guide — Knowledge Work Agents with Human Approval

## Overview

This guide gets you from zero to a working CrewAI installation where you can define agent "job descriptions" in YAML, assign them tasks, and review/approve their output before anything proceeds. The setup runs entirely on your local machine (Windows/WSL) and can later be pointed at a cloud environment.

---

## 1. Environment Setup

CrewAI requires Python 3.10–3.13. Since you already have WSL/Ubuntu available, that's the cleanest path.

### In WSL/Ubuntu

```bash
# Verify Python version
python3 --version

# Create a project directory
mkdir ~/crewai-workspace && cd ~/crewai-workspace

# Create a virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install CrewAI with tools
pip install crewai crewai-tools

# Verify installation
python3 -c "import crewai; print(crewai.__version__)"
```

### On Windows (if preferred)

```powershell
# Same process, different activate command
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install crewai crewai-tools
```

> **Note:** If you hit a `chroma-hnswlib` build error on Windows, you'll need Visual Studio Build Tools with "Desktop development with C++". WSL avoids this entirely.

### Set Your API Key

Create a `.env` file in your project root:

```bash
# .env
ANTHROPIC_API_KEY=your-anthropic-api-key-here
```

---

## 2. Project Structure

CrewAI's recommended structure uses YAML for agent/task definitions (the "job descriptions") and Python for wiring them together. Scaffold it with:

```bash
crewai create crew knowledge_workers
cd knowledge_workers
```

This generates:

```
knowledge_workers/
├── pyproject.toml
├── README.md
├── .env                    # API keys go here
└── src/
    └── knowledge_workers/
        ├── __init__.py
        ├── main.py          # Entry point
        ├── crew.py          # Wires agents + tasks into a Crew
        ├── tools/
        │   ├── __init__.py
        │   └── custom_tool.py
        └── config/
            ├── agents.yaml   # "Job descriptions" live here
            └── tasks.yaml    # Work assignments live here
```

**Or** you can skip the scaffolding and build it manually (see Section 3 below for a complete standalone example).

---

## 3. Your First Crew — Manual Standalone Setup

This is a self-contained example you can run immediately. It defines three agents doing knowledge work with human approval gates.

### File: `agents.yaml`

Think of this file as your team's org chart — each entry is a job description.

```yaml
researcher:
  role: "Research Analyst"
  goal: >
    Find, evaluate, and synthesize information on {topic} from available
    sources. Produce structured research notes with citations, confidence
    levels, and identified knowledge gaps.
  backstory: >
    You are a meticulous research analyst who values accuracy over speed.
    You always distinguish between well-supported facts and speculation.
    When you're uncertain, you say so explicitly. You organize findings
    into clear categories and flag areas that need human verification.
  verbose: true
  llm: anthropic/claude-sonnet-4-5-20250929

analyst:
  role: "Strategy Analyst"
  goal: >
    Review research findings and develop actionable recommendations on
    {topic}. Weigh tradeoffs explicitly, identify risks, and present
    options rather than single answers.
  backstory: >
    You are a pragmatic strategy analyst who works for a small technology
    consulting firm. You understand that recommendations must be practical
    for a small team with limited resources. You always present at least
    two options with clear pros/cons rather than a single recommendation.
  verbose: true
  llm: anthropic/claude-sonnet-4-5-20250929

writer:
  role: "Document Author"
  goal: >
    Take research and analysis and produce a clear, well-structured
    document suitable for {audience}. The document should be ready to
    share externally after human review.
  backstory: >
    You are a clear, concise technical writer. You avoid jargon unless
    writing for a technical audience. You structure documents with
    executive summaries, clear sections, and specific next steps.
    You always include a "Questions for Review" section at the end
    listing items that need human decision-making.
  verbose: true
  llm: anthropic/claude-sonnet-4-5-20250929
```

### File: `tasks.yaml`

Each task is a work assignment. The `human_input: true` flag is your approval gate — the agent will pause and show you its work before the crew moves on.

```yaml
research_task:
  description: >
    Research the following topic thoroughly: {topic}

    Produce structured research notes that include:
    - Key findings organized by subtopic
    - Source quality assessment (high/medium/low confidence)
    - Knowledge gaps or areas needing clarification
    - Any conflicting information found

    Save your findings in a clear, scannable format.
  expected_output: >
    A structured research brief with categorized findings,
    confidence levels, and identified gaps. Minimum 500 words.
  agent: researcher
  human_input: true

analysis_task:
  description: >
    Review the research findings and produce a strategic analysis:

    - Identify the 2-3 most important insights
    - Develop at least 2 distinct options/recommendations
    - For each option, list: pros, cons, estimated effort, risks
    - Flag any decisions that require human judgment
    - Note dependencies or prerequisites

    Be specific and practical. This is for a small consulting firm.
  expected_output: >
    A decision-ready analysis document with multiple options,
    tradeoff comparisons, and a clear "decisions needed" section.
  agent: analyst
  human_input: true
  context:
    - research_task

writing_task:
  description: >
    Using the approved research and analysis, produce a final document
    for {audience}.

    Structure:
    1. Executive Summary (3-5 sentences)
    2. Key Findings
    3. Recommendations with rationale
    4. Next Steps (specific, actionable)
    5. Questions for Review (items needing human decision)

    Write clearly and concisely. Aim for {document_length}.
  expected_output: >
    A polished, ready-to-review document with all required sections.
  agent: writer
  human_input: true
  context:
    - research_task
    - analysis_task
```

### File: `crew.py`

```python
import os
from dotenv import load_dotenv
from crewai import Agent, Task, Crew, Process
import yaml

load_dotenv()

def load_yaml(filepath):
    with open(filepath, 'r') as f:
        return yaml.safe_load(f)

def build_crew(inputs: dict):
    """Build and return a crew ready to kick off."""

    agents_config = load_yaml('agents.yaml')
    tasks_config = load_yaml('tasks.yaml')

    # --- Create Agents from YAML "job descriptions" ---
    researcher = Agent(
        role=agents_config['researcher']['role'],
        goal=agents_config['researcher']['goal'].format(**inputs),
        backstory=agents_config['researcher']['backstory'],
        verbose=agents_config['researcher'].get('verbose', True),
        llm=agents_config['researcher'].get('llm', 'anthropic/claude-sonnet-4-5-20250929'),
        allow_delegation=False,
    )

    analyst = Agent(
        role=agents_config['analyst']['role'],
        goal=agents_config['analyst']['goal'].format(**inputs),
        backstory=agents_config['analyst']['backstory'],
        verbose=agents_config['analyst'].get('verbose', True),
        llm=agents_config['analyst'].get('llm', 'anthropic/claude-sonnet-4-5-20250929'),
        allow_delegation=False,
    )

    writer = Agent(
        role=agents_config['writer']['role'],
        goal=agents_config['writer']['goal'].format(**inputs),
        backstory=agents_config['writer']['backstory'],
        verbose=agents_config['writer'].get('verbose', True),
        llm=agents_config['writer'].get('llm', 'anthropic/claude-sonnet-4-5-20250929'),
        allow_delegation=False,
    )

    # --- Create Tasks from YAML ---
    research_task = Task(
        description=tasks_config['research_task']['description'].format(**inputs),
        expected_output=tasks_config['research_task']['expected_output'],
        agent=researcher,
        human_input=tasks_config['research_task'].get('human_input', False),
    )

    analysis_task = Task(
        description=tasks_config['analysis_task']['description'].format(**inputs),
        expected_output=tasks_config['analysis_task']['expected_output'],
        agent=analyst,
        human_input=tasks_config['analysis_task'].get('human_input', False),
        context=[research_task],
    )

    writing_task = Task(
        description=tasks_config['writing_task']['description'].format(**inputs),
        expected_output=tasks_config['writing_task']['expected_output'],
        agent=writer,
        human_input=tasks_config['writing_task'].get('human_input', False),
        context=[research_task, analysis_task],
        output_file=f"output/{inputs.get('output_filename', 'document')}.md"
    )

    # --- Assemble the Crew ---
    crew = Crew(
        agents=[researcher, analyst, writer],
        tasks=[research_task, analysis_task, writing_task],
        process=Process.sequential,  # Tasks run in order
        verbose=True,
        memory=True,  # Agents retain context across tasks
    )

    return crew
```

### File: `main.py`

```python
#!/usr/bin/env python3
"""
Run a knowledge work crew.

Usage:
    python main.py

Modify the 'inputs' dict below to change the assignment.
"""

import os
from crew import build_crew

def main():
    # Ensure output directory exists
    os.makedirs('output', exist_ok=True)

    # --- This is your "work order" ---
    # Change these values to assign different work
    inputs = {
        'topic': 'Best practices for network monitoring alerting thresholds in small ISP environments',
        'audience': 'technical staff at a small ISP',
        'document_length': '1,000-1,500 words',
        'output_filename': 'alerting-thresholds-guide',
    }

    print(f"\n{'='*60}")
    print(f"  CREW ASSIGNMENT: {inputs['topic'][:50]}...")
    print(f"  Audience: {inputs['audience']}")
    print(f"{'='*60}\n")

    crew = build_crew(inputs)
    result = crew.kickoff()

    print(f"\n{'='*60}")
    print("  CREW WORK COMPLETE")
    print(f"  Output saved to: output/{inputs['output_filename']}.md")
    print(f"{'='*60}\n")
    print(result)

if __name__ == '__main__':
    main()
```

---

## 4. Running It

```bash
# Make sure you're in your project directory with .venv activated
cd ~/crewai-workspace

# Create the output directory
mkdir -p output

# Run the crew
python main.py
```

**What happens:**

1. The **Researcher** agent works on the topic, then pauses and shows you its findings. You can approve, provide feedback, or ask for changes.
2. Once approved, the **Analyst** receives the research and develops recommendations, then pauses for your review.
3. Once approved, the **Writer** produces the final document, pauses for your review, then saves to `output/`.

Each pause is your approval gate. You type feedback directly in the terminal.

---

## 5. Writing New "Job Descriptions"

To add a new agent, just add a new entry to `agents.yaml`:

```yaml
outage_monitor:
  role: "Network Operations Analyst"
  goal: >
    Monitor OutageScout alert data and produce daily summaries of
    network health trends, recurring issues, and recommended
    threshold adjustments for {network_segment}.
  backstory: >
    You are an experienced NOC analyst who has worked with small and
    mid-size ISPs. You understand that alert fatigue is a real problem
    and you focus on actionable insights rather than raw data dumps.
    You prioritize alerts by business impact.
  verbose: true
  llm: anthropic/claude-sonnet-4-5-20250929
```

Then add a corresponding task in `tasks.yaml` and wire it into `crew.py`.

---

## 6. Cost Awareness

Each agent call uses your Anthropic API credits. For Sonnet 4.5:
- Input: $3 per million tokens
- Output: $15 per million tokens

A typical three-agent sequential crew with human review might use 10,000–30,000 tokens per run, costing roughly $0.05–$0.30 depending on topic complexity and output length. Using `claude-haiku-4-5-20251001` for research/drafting tasks and reserving Sonnet for analysis can cut costs significantly.

Set per-agent models in `agents.yaml` to control this:

```yaml
researcher:
  llm: anthropic/claude-haiku-4-5-20251001   # cheaper for gathering
analyst:
  llm: anthropic/claude-sonnet-4-5-20250929  # stronger for reasoning
writer:
  llm: anthropic/claude-haiku-4-5-20251001   # cheaper for drafting
```

---

## 7. Next Steps — Growing the System

Once the basic crew is running, here's the natural progression:

**Add tools** — Give agents the ability to read files, search the web, or query databases:
```python
from crewai_tools import FileReadTool, DirectoryReadTool

researcher = Agent(
    ...,
    tools=[FileReadTool(), DirectoryReadTool(directory='./knowledge-base')],
)
```

**File-based communication** — Have agents write their output to a shared `workspace/` directory that other crews can read from later. This is your low-tech "message queue."

**Flows for complex pipelines** — CrewAI Flows let you chain multiple crews together with conditional logic (if researcher finds X, route to crew A; otherwise crew B).

**Memory** — CrewAI supports short-term, long-term, and entity memory. Enable `memory=True` on the Crew to let agents build up knowledge across tasks within a run. Long-term memory persists across runs.

**Move to cloud** — The entire setup is just Python files + YAML configs. Copy to a VPS, set up your .env with API keys, and run. No infrastructure changes needed.

---

## Quick Reference

| What you want to do | Where to do it |
|---|---|
| Add/edit an agent's role | `agents.yaml` |
| Add/edit a work assignment | `tasks.yaml` |
| Change which model an agent uses | `llm:` field in `agents.yaml` |
| Require your approval before proceeding | `human_input: true` in `tasks.yaml` |
| Give an agent access to files/tools | `tools=[]` parameter in `crew.py` |
| Change the workflow order | `context:` dependencies in `tasks.yaml` |
| Save output to a file | `output_file=` parameter on the final Task |
