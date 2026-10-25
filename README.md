[![Pylint](https://github.com/mariospapadimas/hash-verifier/actions/workflows/pylint.yml/badge.svg?branch=main)](https://github.com/mariospapadimas/hash-verifier/actions/workflows/pylint.yml)
[![Python App](https://github.com/mariospapadimas/hash-verifier/actions/workflows/python-app.yml/badge.svg?branch=main)](https://github.com/mariospapadimas/hash-verifier/actions/workflows/python-app.yml)
# Hash Verifier
A Python/Tkinter app for comparing hashes and verifying files.

Supports SHA-256 (default), SHA-512, SHA-1 and MD5. Both modes require the
selected algorithm's exact hexadecimal digest length and ignore surrounding
whitespace and letter case.

Use Python `>=3.9,<=3.11.9` with Tkinter. In particular, Python 3.11.10 and newer
are outside the current constraint. Python 3.11.9 is a suitable exact version.
The Windows Python installer includes Tkinter when Tcl/Tk is selected; other
platforms may need their distribution's Tk package and a desktop display.

Clone and launch from the repository root:

```powershell
git clone https://github.com/mariospapadimas/hash-verifier.git
cd hash-verifier
python -m src.main
```

In **Verify Files Mode**, select a file, choose the algorithm, paste the expected
hash and click **Verify File**. Hashing runs in the background with byte progress
and reads 1 MiB at a time. **Clear** resets the selection and result. You can
switch modes while a file is being hashed.

The expected hash should come from a source you trust. A match confirms that the
file has that digest; it does not establish who published the file.

For development, install [uv](https://docs.astral.sh/uv/) and run:

```powershell
uv lock --check
uv sync --dev --locked --python 3.11.9
uv run --locked pytest -v
uv run --locked pylint --fail-under=9.0 src tests
```

Code is under `src/`, tests under `tests/`. The tests use real file hashing and
worker threads with lightweight Tk widget doubles, so they also run headlessly.

Both `.github/workflows/python-app.yml` (Python App) and
`.github/workflows/pylint.yml` (Pylint) run on pushes to every branch, including
maintenance branches, and on pull requests targeting `main`. Tests exercise
Python 3.9, 3.10, and exactly 3.11.9; lint uses 3.11.9. CI installs the committed
lockfile using uv 0.13.0 and fails on test errors or zero collected tests.
Badges above report `main`; branch runs are visible in GitHub Actions.

There is also an optional rendered Tk layout test. On a machine with a desktop:

```powershell
$env:HASH_CHECKER_UI_TESTS = "1"
uv run --locked pytest tests/test_tk_ui.py -v
```

`HASH_CHECKER_UI_TESTS` retains its existing name for compatibility. This optional
desktop test is skipped during the ordinary headless suite; all other tests run.

The project metadata is named `hash-verifier`; execution remains `python -m src.main`.
There are no third-party runtime dependencies. Use the uv development group for
pytest and Pylint rather than the legacy runtime-only `requirements.txt` export.

The existing [Apache License 2.0](LICENSE) is unchanged. Shared JetBrains inspection
profiles and VCS mapping remain tracked; generated module/SDK configuration is
local and ignored.

***All code must follow python naming conventions and be PEP8 compliant.***
