# Storybook Tests

```bash
uv run --extra dev pytest tests/ --cov=src --cov-fail-under=90
```

The tests validate the YAML story contract, opposite-family character pairing,
single-page PNG rendering, PDF assembly, and script-level orchestration.
