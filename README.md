# Azure Integration with Azure DevOps 

## Project Overview

CAM_Automation is a lightweight repository that contains automation, configuration artifacts, and CI/CD pipeline definitions intended to simplify deployment and management tasks for cloud account management (CAM) and related Azure DevOps workflows. The repository stores pipeline configuration, subscription inventories, and helper scripts to accelerate onboarding and automated operations across Azure subscriptions.

This README provides an overview of repository structure, prerequisites, configuration, usage examples, and guidance for contributing.

## Repository Structure

- `azure-pipelines.yml` - Primary Azure DevOps pipeline definition used to run automation flows and validations.
- `subscriptions/` - Stores subscription inventory and configuration data (example: `subs.json`).
- `README.md` - Project documentation (this file).

Additional supporting files and scripts may be added to this repo over time. Keep the repository tidy and place environment-specific data under `subscriptions/` or a dedicated config folder.

## Key Concepts

- Pipeline-driven automation: pipeline definitions in `azure-pipelines.yml` orchestrate validation, deployment, and automation tasks.
- Subscription inventory: a canonical list of subscription IDs and metadata is maintained in `subscriptions/subs.json` to drive per-subscription operations.
- Idempotent operations: automation code and pipelines should be safe to run repeatedly without causing unintended side effects.

## Prerequisites

- An Azure DevOps organization with appropriate permissions to run pipelines.
- Service connection(s) configured in Azure DevOps to authorize operations against target Azure subscriptions (service principals or managed identities).
- Azure CLI and/or PowerShell (for local testing and running scripts).

Optional (for contributors and maintainers):

- `az` CLI installed and logged in: `az login`.
- Access to the subscription list in `subscriptions/subs.json` and any secrets/configuration stored in your organization.

## Configuration

1. Review `subscriptions/subs.json` and update it with your subscriptions and metadata following the existing JSON schema.
2. Ensure your Azure DevOps pipeline service connections have sufficient permissions to perform the actions defined in `azure-pipelines.yml`.
3. If the pipeline relies on secrets or variables, configure them in the Azure DevOps pipeline library or variable groups (do not commit secrets to this repo).

## Usage

Local testing (recommended before committing changes):

1. Clone the repository locally.
2. Inspect or modify `subscriptions/subs.json` for a test subset of subscriptions.
3. Run any helper scripts (if present) using PowerShell or Bash to validate behavior.

Example: validate JSON file using `jq` (local):

```bash
jq . subscriptions/subs.json
```

To run the automation in Azure DevOps:

1. Create a new pipeline in your Azure DevOps project and point it at this repository (or reference as a template).
2. Ensure the pipeline has access to the required service connections and variable groups.
3. Queue the pipeline from the Azure DevOps UI or configure triggers as desired.

## CI / CD

This repository includes an `azure-pipelines.yml` definition intended to be used in Azure DevOps. The pipeline may include stages such as:

- Lint/validate configuration files (JSON/YAML schema checks).
- Run infrastructure or account validation tasks against the listed subscriptions.
- Publish reports or artifacts to pipeline artifacts or external storage.

Customize and extend the pipeline to match your organizational workflows. Keep sensitive credentials out of the repo and use pipeline variable groups or Azure Key Vault integration instead.

## Contributing

Contributions are welcome. If you want to propose changes:

1. Fork the repository (or create a branch in your fork).
2. Make small, focused changes with clear commit messages.
3. Open a pull request describing the change, why it is needed, and any migration steps.

Guidelines:

- Do not commit secrets, credentials, or personal access tokens.
- Keep configuration files in `subscriptions/` scoped and well-documented.
- Update this README with any new top-level files or structural changes.

## Security and Secrets

Never store credentials or secrets in this repository. Use Azure DevOps secure pipelines variables, variable groups, or Azure Key Vault for any secrets required by pipelines or scripts.

## Troubleshooting

- If a pipeline fails, inspect the pipeline logs in Azure DevOps and validate the service connection credentials.
- Validate JSON/YAML files locally before pushing changes.

