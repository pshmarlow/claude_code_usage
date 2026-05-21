## Conventions

- Python, formatted with black, type-hinted, PEP8-compliant.
- Dependency mgmt: uv. Test runner: pytest.
- Tests in `/tests`, mirroring source layout.
- Pydantic for data validation; python-dotenv for env vars.
- Google-style docstrings on every public function.
- Module layout: separate files by responsibility (e.g. `agent.py` / `tools.py` / `models.py` for an agent project).
