# Secludy PII Injection and Detection Tool

A containerized solution for detecting and preventing PII leakage in Large Language Models, available on AWS Marketplace.

## Overview

This tool helps organizations:
- Inject canary PII sequences into training data
- Detect potential PII leakage in model outputs
- Generate comprehensive analysis reports

## Quick Start

### Prerequisites
- Docker installed
- Access to AWS Marketplace
- Local data directory for input/output files

### Installation

Pull the container from AWS Marketplace: 
bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin [ECR_REPOSITORY_URI]
docker pull [ECR_REPOSITORY_URI]/pii_leak:latest


### Usage

1. **Inject Canary Sequences**
docker run -v $(pwd)/data:/app/data pii_leak inject \
--dataset data/input.jsonl \
--output data/injected.jsonl

2. **Train Your Model**
   - Use the injected dataset (`injected.jsonl`) with your LLM training pipeline
   - Follow your organization's standard training procedures

3. **Generate Test Outputs**
   - Use your fine-tuned model to generate test outputs
   - Save results in JSONL format with a 'content' field

4. **Detect PII Leakage**


docker run -v $(pwd)/data:/app/data pii_leak detect \
--dataset data/llm_generated_results.jsonl \
--output-dir data


## Input Requirements

- Place your dataset file in a local `data` directory
- Input dataset must be in JSONL format with 'content' field containing model outputs
- The container will mount your local `data` directory to `/app/data` inside the container

## Output Files

The tool generates the following files in your data directory:
- `leakage_results_ahocorasick.json`: Detailed detection results
- `per_category_stats_ahocorasick.json`: Statistical analysis by PII category
- `leakage_report.pdf`: Executive summary with visualizations


## Support

For technical support or questions:
- Email: support@secludy.com
- Create an issue on our [GitHub repository](https://github.com/secludy/pii-inject-detect)

## License

This project is licensed under the [Apache 2.0 License](LICENSE)