## Matrix Strategy
- Runs a single job across multiple combinations of variables (OS, versions, platforms).
- Each combination runs as a **separate job** in parallel.
- **Fail-fast**: By default, if one job fails, GitHub cancels all other jobs in the matrix. Set `fail-fast: false` to prevent this.
- **Max-parallel**: Limits the number of jobs that can run simultaneously.

### Strategy Keywords
- `matrix`: Defines the configuration options.
- `include`: Adds specific extra combinations (useful for edge cases).
- `exclude`: Removes specific combinations from the generated matrix.

### Example: Multiple OS and Versions
```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
```

### Example: Include and Exclude
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node: [16, 18]
    include:
      # Adds a specific combo not in the original matrix
      - os: ubuntu-latest
        node: 20
        is-experimental: true
    exclude:
      # Removes a specific combo
      - os: windows-latest
        node: 16
```

### Real-World Use Cases
- **Cross-Platform Support**: Verifying CLI tools or desktop apps work on Windows, macOS, and Linux.
- **Runtime Compatibility**: Testing a library against multiple versions of a language (e.g., Node.js 18, 20, 22).
- **Dependency Testing**: Ensuring compatibility with different versions of a core dependency (e.g., React 17 vs 18).
- **Database Variations**: Running integration tests against different DB versions (Postgres 14, 15, 16).
- **Parallel Test Splitting**: Dividing a massive test suite into smaller chunks to run them all at once (e.g., `group: [1, 2, 3, 4]`).

