## Jobs Execution
- **Parallel**: Default behavior; jobs run concurrently.
- **Sequential**: Use `needs` to define dependencies.
- **Isolation**: Each job runs in a **separate runner** and a **fresh workspace**. They do not share files directly.
 

## Passing Data Between Jobs
To pass values between jobs, define `outputs` at the job level.

1. **Map Step to Job Output**: `outputs: { key: ${{ steps.<id>.outputs.<key> }} }`
2. **Access in Next Job**: `${{ needs.<job_id>.outputs.<key> }}` (requires `needs`).

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      my_shared_val: ${{ steps.step1.outputs.val }}
    steps:
      - id: step1
        run: echo "val=hello-from-job1" >> $GITHUB_OUTPUT

  job2:
    needs: job1
    runs-on: ubuntu-latest
    steps:
      - run: echo "Received: ${{ needs.job1.outputs.my_shared_val }}"
```