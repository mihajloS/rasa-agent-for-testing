# Test this out on hello rasa:

[![Launch on Hello Rasa](https://hello.rasa.com/launch.svg)](https://hello.rasa.com/go?repo=amalsgit/rasa-agent-for-testing)


# Basic Rasa Template

A simple, general-purpose conversational agent template that provides essential conversational capabilities.

## Install and run (with uv)

This repo pins dependencies in `uv.lock`. Use [uv](https://docs.astral.sh/uv/) so installs match that lockfile.

1. **Install uv** (if you do not have it yet): follow the [install instructions](https://docs.astral.sh/uv/getting-started/installation/).

2. **Create the environment and install dependencies** from the lockfile:

   ```bash
   uv sync
   ```

   This reads `uv.lock` and installs the exact versions recorded there (including `rasa-pro` from `pyproject.toml`). Python follows `.python-version` (3.10) when uv creates a project virtual environment.

3. **Run Rasa via uv** so commands use that environment:

   ```bash
   # Validate configuration and data
   uv run rasa data validate

   # Train a model
   uv run rasa train

   # Talk to the assistant 
   uv run rasa inspect --nextgen

   # Run the HTTP server (after training, or with a trained model path)
   uv run rasa run
   ```

   Add flags as needed (for example `uv run rasa train --fixed-model-name my-model`).

If you change dependencies in `pyproject.toml`, run `uv lock` to refresh `uv.lock`, then `uv sync` again.

## 🚀 What's Included

This template provides a foundation for building conversational agents with:
- **Basic conversational flows**: Greetings, help, feedback, and human handoff
- **Help system**: Users can ask for assistance and get guided responses
- **Feedback collection**: Gather user feedback to improve the agent
- **Human handoff**: Seamlessly transfer conversations to human agents when needed

## 📁 Directory Structure

```
├── actions/          # Custom Python logic for agent actions
├── data/            # Conversational flows and training data
├── domain/          # Agent configuration (slots, responses, actions)
├── docs/            # Knowledge base documents (optional)
├── prompts/         # LLM prompts for enhanced responses
└── config.yml       # Training pipeline configuration
```

