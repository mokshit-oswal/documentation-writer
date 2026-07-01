# mlp_consumer_match

MatchAI is a Databricks-based platform for record linkage and deduplication using Splink probabilistic matching. The `mlp_consumer_match` package implements preprocessing, model training, parallel inference, post-processing, clustering, and data profiling workflows, driven by Hydra YAML configuration and orchestrated through Databricks jobs with optional AWS Lambda triggers. It is intended for MatchAI operators who configure and run client workflows, and for engineers who extend preprocessors, comparison levels, and pipeline stages.

## Documentation

- [User Playbook](user-playbook.md) — How to use this project: deploy, configure, and run MatchAI workflows.
- [Developer Playbook](developer-playbook.md) — How to work on this project: architecture, pipeline reference, add-a-feature guide, and engineering conventions.
