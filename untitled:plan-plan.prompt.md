## Plan: Add FastAPI backend tests

We need new pytest tests for the FastAPI application located in `src/app.py`.

**TL;DR**: Create a `tests/` directory with a `test_app.py` that uses `TestClient` from FastAPI and pytest fixtures to reset the in-memory `activities` dictionary. Cover basic routes and error cases.

**Steps**
1. Add `tests/` directory at workspace root.
2. Create `tests/test_app.py` containing:
   - import of `TestClient` and `app` from `src.app`.
   - a pytest `autouse` fixture that backs up `app.activities` (deepcopy) and restores it after each test.
   - tests for `/` redirect, `/activities` retrieval, `signup` success and failure scenarios, and `unregister` success and failure scenarios.
3. Ensure `requirements.txt` already includes `httpx` (it does) and `pytest` will be used when running tests.
4. Optionally, add placeholder `__init__.py` in `tests` to make it a package (not strictly necessary due to pytest conventions, but it's a common practice).
5. Add instructions to README or project docs if needed (optional, maybe not required for plan).

**Relevant files**
- `/workspaces/skills-getting-started-with-github-copilot/src/app.py` (source to test)
- `/workspaces/skills-getting-started-with-github-copilot/pytest.ini` (already configures pythonpath)
- new files under `/workspaces/skills-getting-started-with-github-copilot/tests/`

**Verification**
1. Run `pytest -q` from project root; all tests should pass.
2. Confirm tests cover both happy and error paths by observing their assertions.
3. Ensure running tests multiple times does not accumulate state (activities reset between tests).
4. Possibly inspect coverage to ensure endpoints are hit (optional manual).

**Decisions**
- Tests manipulate the global `activities` dictionary and therefore require resetting state; chosen approach is an `autouse` fixture with `deepcopy` to preserve original state.
- Testing only backend logic; static content and frontend are out of scope.

**Further Considerations**
1. Should we add asynchronous tests or run with `httpx.AsyncClient`? Current app uses sync endpoints; sync client suffices. Consider async later if app evolves.
2. Might want to split tests into multiple files as feature set grows; start single file for now.
3. If tests grow, consider factoring common setup or using test data builders.
