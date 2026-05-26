# Dude: A Dual-Detection Multi-Agent System for Paper-Code Discrepancy Detection

## Overview

**Dude** is a multi-agent system designed to automatically detect discrepancies between academic papers and their corresponding code implementations. The system employs a dual-detection approach with specialized agents that work collaboratively through a negotiation-based workflow to identify inconsistencies, missing implementations, and deviations from paper specifications.

### Key Features

- **Multi-Agent Architecture**: Specialized agents for different detection modes (code analysis, paper analysis)
- **Role-Based Teams**: Each agent has specific roles (Claimer, Negotiator, Verifier)
- **Multi-LLM Support**: Configurations for Claude, Deepseek, GPT, and Kimi models
- **Orchestrated Workflow**: Structured three-stage pipeline for systematic discrepancy detection
- **Negotiation-Based Validation**: Agents collaborate to resolve discrepancies through negotiation rounds

## Project Structure

```
Dude/
├── agents/                              # Multi-LLM agent configurations
│   ├── Claude/                          # Claude model agents
│   │   ├── code_claimer.md             # Extracts claims from code
│   │   ├── code_negotiator.md          # Negotiates code claim validity
│   │   ├── code_verifier.md            # Verifies code claims
│   │   ├── paper_claimer.md            # Extracts claims from papers
│   │   ├── paper_negotiator.md         # Negotiates paper claim validity
│   │   └── paper_verifier.md           # Verifies paper claims
│   ├── Deepseek/                        # Deepseek model agents (same structure)
│   ├── GPT/                             # GPT model agents (.toml)
│   └── Kimi/                            # Kimi model agents
│
├── orchstrator-prompt/                  # Orchestrator workflow prompts
│   ├── orchestrator_initialization_prompt.md   # Stage 1: Initialize analysis
│   ├── orchestrator_negotiation_prompt.md      # Stage 2: Resolution negotiation
│   └── orchestrator_code-summary_prompt.md     # Stage 3: Generate summary
│
├── info.md                              # Configuration metadata
├── README.md                            # This file
```

## How It Works

### Agent Roles

**Claimer**
- Analyzes code repositories or papers
- Extracts atomic, auditable claims about implementations or specifications
- Produces structured JSON records of identified claims
- Focuses on claims relevant to paper-code verification

**Negotiator**
- Takes claims from multiple sources (code and paper)
- Identifies discrepancies between claims
- Conducts negotiation rounds (default: 2 rounds) to resolve conflicts
- Synthesizes consensus through iterative dialogue

**Verifier**
- Validates the final set of claims
- Ensures consistency and accuracy
- Produces audit-ready verification reports

### Workflow Stages

#### Stage 1: Paper-oriented Discrepancy Detection
- Extract research claim from paper manuscript
- Verify paper-code consistency by searching for corrsponding code implementation
- Runs negotiation cycles between agents
- Resolves identified discrepancies through agent dialogue
- Updates claim records based on negotiation outcomes

#### Stage 2: Code-oriented Discrepancy Detection
- Extract research claim from code repository
- Conduct anchor-guided filtering  using category-wise prior knowledge
- Verify paper-code consistency by searching for corrsponding paper description
- Conduct evidence-based filtering.


#### Stage 3: Final Report Generation
- Generates comprehensive analysis summaries
- Produces final discrepancy report
- Creates audit documentation

## Configuration

### Setup

Edit `info.md` to configure:

```markdown
paper_path = "<path to paper PDF or document>"
code_path = "<path to code repository>"
Maximum_round for negotiation: 2
```

### Model Selection

Choose your preferred LLM provider by selecting agents from:
- **Claude**: `agents/Claude/`
- **Deepseek**: `agents/Deepseek/`
- **GPT**: `agents/GPT/` (TOML format)
- **Kimi**: `agents/Kimi/`

Each provider folder contains pre-configured agent prompts for all six roles (3 code + 3 paper agents).

## Installation Requirements

- **jq**: Required for processing and manipulating JSON files. Install via:
  ```bash
  # macOS
  brew install jq
  
  # Linux
  sudo apt-get install jq
  ```

## Important Notes

### 1. Paper Upload with MinerU
MinerU uploads may fail during peak hours. If you encounter upload failures:
- Change `paper_path` to `paper_url` in `info.md`
- Provide a direct URL to the paper instead of a local file path
- Example:
  ```markdown
  paper_url = "https://arxiv.org/pdf/XXXX.XXXXX.pdf"
  ```

### 2. Codex Agent Configuration
When using **Codex** as your agent framework, ensure stable agent activation by configuring your agent information:
- Add your agent credentials to `~/.codex/config.toml`
- This guarantees stable and reliable agent activation
- Without proper configuration, agents may fail to initialize consistently
