# Switch Environment Variables

A GitHub Action that enables switching environment variables based on different execution environments.

## Description

This action allows you to load environment variables from a YAML file based on a specified environment key. It's useful for managing different sets of environment variables for various deployment environments (e.g., development, staging, production) in your GitHub Actions workflows.

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `file` | Path to the environment variables YAML file | Yes |
| `env` | Environment key to use (e.g., 'dev', 'prod', 'staging') | Yes |

## Usage

### Example Workflow

```yaml
name: Deploy Application

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Set Environment Variables
        uses: HirokazuHara/switch_envs@v1
        with:
          file: './config/environments.yml'
          env: 'production'
      
      - name: Deploy with environment variables
        run: |
          echo "Deploying with API_URL=$API_URL"
          # Your deployment steps here
```

### Example YAML Configuration File

Create a YAML file (e.g., `environments.yml`) with the following structure:

```yaml
development:
  API_URL: "https://dev-api.example.com"
  DEBUG: "true"
  LOG_LEVEL: "debug"

staging:
  API_URL: "https://staging-api.example.com"
  DEBUG: "false"
  LOG_LEVEL: "info"

production:
  API_URL: "https://api.example.com"
  DEBUG: "false"
  LOG_LEVEL: "error"
```

## How It Works

The action uses [yq](https://github.com/mikefarah/yq) to parse the YAML file and extract the environment variables for the specified environment. It then adds these variables to the GitHub Actions environment using the `GITHUB_ENV` file.

## License

MIT
