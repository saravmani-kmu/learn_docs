## Simple Workflow

Basic workflow structure with `actions/checkout`.

## Runners
- **GitHub-hosted**: Managed VMs (Ubuntu, Windows, macOS). Each job gets a **fresh VM**.
- **Self-hosted**: Your own infrastructure (VM or Container).
- **Architecture**: Technically, standard GitHub runners are **Virtual Machines** (Azure VMs).
- **Container Job**: Use `container: <image>` to run your job steps inside a Docker container on the runner VM.

```yaml
jobs:
  my_job:
    runs-on: ubuntu-latest
    container: node:18-alpine # Job runs inside this container on the VM
    steps:
      - run: node -v
```

### Building Blocks of Workflow
- **name**: Workflow name.
- **on**: Trigger event (e.g., `push`).
- **jobs**: Defined tasks.
- **steps**: Sequence of actions.

```yaml
name: Simple Checkout
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Run scripts
        run: |
          echo "Code checked out!"
          echo Workspace: ${{ github.workspace }}
          echo Action: ${{ github.action }}
```

### Passing Parameters
- **with**: Pass inputs to an action (e.g., `submodules: recursive`).
- **id**: Required to reference a step's outputs.

### Passing Data Between Steps
Use `$GITHUB_OUTPUT` to share values.

1. **Set Output**: `echo "key=value" >> $GITHUB_OUTPUT` (requires step `id`).
2. **Access Output**: `${{ steps.<id>.outputs.<key> }}`.

```yaml
- name: Generate Output
  id: producer
  run: echo "my_var=hello-world" >> $GITHUB_OUTPUT

- name: Consume Output
  run: echo "The value is ${{ steps.producer.outputs.my_var }}"
```
