# Contributing to log-file-metric-exporter

Thank you for your interest in contributing! This guide explains how to submit changes to this project.

## PR Process

1. **Fork and Branch**: Create a branch from `main` with a descriptive name
   ```bash
   git checkout -b LOG-XXXX/short-description
   ```

2. **Make Changes**: Implement your feature or fix, following the conventions below

3. **Test Your Changes**: Run the full test suite locally before pushing
   ```bash
   make test
   make lint
   ```

4. **Commit**: Write clear commit messages that explain the *why* not just the *what*
   ```text
   LOG-XXXX: Brief description
   
   Longer explanation of the change and why it was made.
   ```

5. **Push and Open PR**: Push to your fork and open a PR against `main`
   - Link the related Jira issue (e.g., `Fixes LOG-9343`)
   - Describe what changed and why
   - Reference any new test coverage

6. **Review**: Address feedback from reviewers promptly

## Review Expectations

- **Functionality**: Does it solve the stated problem correctly?
- **Tests**: Are there tests covering the new behavior? Do all tests pass?
- **Performance**: Does it introduce any regressions? (see benchmarks in `pkg/symnotify`)
- **Documentation**: Is the behavior documented in code comments or README?
- **Compatibility**: Does it work with supported TLS configurations?

## Coding Conventions

### Go Style
- Follow Go's standard conventions as enforced by `gofmt`
- Run `make fmt` before committing
- Use `make lint` to check for issues
- Keep packages small and focused

### Naming
- Metric labels: lowercase, underscores (e.g., `log_logged_bytes_total`)
- Functions/types: PascalCase as per Go convention
- Constants: UPPER_SNAKE_CASE

### Comments
- Only comment the *why* – clear code documents the *what*
- No multi-line comment blocks; keep comments to one line when possible
- Omit comments for obvious implementations

### Imports
- Organize imports: stdlib, vendor, local packages
- Remove unused imports before committing

## Testing Requirements

### Unit Tests
- Test files use `_test.go` suffix
- Place tests in the same package as the code being tested
- Test both happy path and error cases

### Run All Tests
```bash
make test
```

### Run Specific Package Tests
```bash
go test ./pkg/logwatch -v
go test ./pkg/symnotify -v
go test ./cmd -v
```

### Benchmark Tests
Some packages include benchmarks (e.g., `symnotify`):
```bash
go test -bench=. ./pkg/symnotify
```

### Coverage
Coverage reports generate to `tmp/coverage/test-unit-coverage.html` after running `make test`.

## Code Organization

### Key Packages
- **`cmd/main.go`**: Entry point, flag parsing, server initialization
- **`pkg/logwatch`**: Core log file watching and metric update logic
- **`pkg/symnotify`**: Symlink-aware file system notification wrapper
- **`pkg/auth`**: Bearer token authentication for metrics endpoint

### Adding New Features
1. Add logic to the appropriate existing package, or create a new one in `pkg/`
2. Add tests alongside your code (`*_test.go` files)
3. Update `AGENTS.md` if the change affects architecture or implementation details
4. Update README or this file if it affects users or developers

## TLS and Security

When making changes to authentication or TLS handling:
- Verify against supported TLS versions (1.2, 1.3 minimum)
- Test with both standard and custom cipher suites
- Update tests if changing cipher suite mappings
- Document any changes in `AGENTS.md`

## Continuous Integration

PRs must pass:
- All unit tests (`make test`)
- Linter (`make lint`)
- Container build test

These run automatically on push. Fix any failures before requesting review.

## Questions?

- Check [AGENTS.md](AGENTS.md) for architecture details
- Check [ARCHITECTURE.md](ARCHITECTURE.md) for design decisions
- Open an issue in the repository for questions or suggestions
