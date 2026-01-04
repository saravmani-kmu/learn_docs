## Types of Variables
- **Environment variables**: Standard keys used to pass info to scripts/steps within a workflow. Defined using `env:`.
- **Secrets**: Encrypted sensitive data (API keys, tokens). They are masked in logs.
- **Variables**: Non-sensitive configuration data (URLs, ports) stored at Repo/Org/Env levels.
- **Output variables**: Data passed from one step to another, or shared between jobs.

## Secrets
- Encrypted environment variables used to store sensitive information (API keys, tokens, passwords).
- Encrypted at rest and only exposed to runners when requested in a workflow.
- **Masking**: GitHub automatically masks secrets in the logs (replaces them with `***`).
- **Permissions**: Once created, secrets cannot be viewed or edited (only updated or deleted).

### Levels of Secrets
- **Repository**: Available to all workflows in a specific repository.
- **Environment**: Available only to jobs that reference a specific environment (useful for staging/prod separation).
- **Organization**: Shared across multiple repositories within an organization.

### Accessing Secrets
- Accessed via the `${{ secrets.SECRET_NAME }}` syntax.
- Must be explicitly passed to an action or set as an environment variable.

### Example: Using Secrets in a Workflow
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to Cloud
        env:
          API_KEY: ${{ secrets.MY_API_KEY }}
        run: ./deploy.sh
```

### Example: Using Secrets in an Action
```yaml
      - name: Login to Docker
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
```

## Variables
- Used for non-sensitive configuration data (e.g., port numbers, environment names, URLs).
- Not encrypted and visible in plain text in the workflow logs.
- Available at **Repository**, **Environment**, and **Organization** levels.

### Accessing Variables
- Accessed via the `${{ vars.VARIABLE_NAME }}` syntax.

### Comparison: Secrets vs Variables
- **Secrets**: Encrypted, masked in logs, for sensitive data.
- **Variables**: Plain text, visible in logs, for configuration data.

### Example: Using Secrets and Variables together
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to Cloud
        env:
          API_KEY: ${{ secrets.MY_API_KEY }}      # Secret
          DEPLOY_URL: ${{ vars.DEPLOYMENT_URL }}  # Variable
        run: ./deploy.sh --url $DEPLOY_URL
```

### Example: Using Variables in an Action
```yaml
      - name: Setup Application
        uses: ./.github/actions/setup
        with:
          version: ${{ vars.APP_VERSION }}
          api_token: ${{ secrets.API_TOKEN }}
```
