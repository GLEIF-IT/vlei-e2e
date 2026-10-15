# vLEI end to end (e2e) tests

This includes the SignifyPy + KERIA to Sally Verifier end-to-end tests of presenting a credential from a SignifyPy wallet to a Sally Verifier.

## Setting up your environment to run tests

Install and test the locked GLEIF dependency chain:

```sh
uv sync --locked --extra test --python 3.12
uv pip check
uv run --no-sync pytest -q --tb=short
```

The lockfile pins a GLEIF-specific SignifyPy 0.4.0 commit in
`kentbull/signifypy`. Its metadata requires `gleif_keri==1.2.16` and
`gleif_hio==0.6.21`; it also accepts pending KERIA operations with omitted
result fields. Do not install upstream `keri` or `hio` alongside those
packages: they own the same Python import paths. The ownership regression
check rejects that mixed installation.

The end-to-end test starts local witnesses, KERIA, and Sally and exercises
credential issuance and presentation. Service threads are stopped on both
success and failure.

License: Apache 2.0