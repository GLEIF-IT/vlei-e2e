# vLEI end to end (e2e) tests

This includes the SignifyPy + KERIA to Sally Verifier end-to-end tests of presenting a credential from a SignifyPy wallet to a Sally Verifier.

## Setting up your environment to run tests

Install dependencies and then test dependencies using `uv`:

```zsh
# Install dependencies
uv sync

# Install test dependencies
uv pip install -e ".[test]
```

Then run pytest:
```zsh
# Either
uv run 

```


License: Apache 2.0