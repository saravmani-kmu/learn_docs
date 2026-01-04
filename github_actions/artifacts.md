## Artifacts
- Files or collections of files produced during a workflow run (binaries, test results, logs).
- Used to persist data beyond job completion or share data between jobs and workflows.
- **Why**: Each job runs on a **separate runner** with its own fresh workspace.
- Default retention: 90 days (configurable 1-90 for public, 1-400 for private).

### Actions
- `actions/upload-artifact@v4`: Save files/directories.
- `actions/download-artifact@v4`: Retrieve stored artifacts.

### Artifacts vs Caching
- **Artifacts**: Store outputs generated *by* the job (e.g., build binaries).
- **Caching**: Store dependencies (e.g., node_modules) to speed up future runs.

### Example: Uploading Artifact
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: mkdir dist && echo "build output" > dist/app.bin
      - uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
          retention-days: 5
```

### Example: Downloading Artifact
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build-output
          path: bin/
```
