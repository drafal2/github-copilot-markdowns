---
name: pytest-unit-tests
description: Write high-quality Python unit tests with pytest for a given module, function, or class. Use when the user asks to write, add, or improve unit tests for Python code.
---

# Write pytest unit tests

Write unit tests for the Python code the user points at (a file, function, class, or recent change). Produce tests that verify behavior, read clearly, and fail with useful messages.

## Workflow

1. **Read the code under test first.** Understand its public behavior: inputs, outputs, side effects, raised exceptions, and edge cases. Also read any existing tests to match the project's conventions (file layout, naming, fixture style, existing `conftest.py`).
2. **Locate where tests belong.** Follow the project's existing layout (`tests/` mirroring the package, or `test_*.py` next to modules). If no tests exist yet, create `tests/test_<module>.py`.
3. **Plan the cases before writing.** For each public function/method, enumerate:
   - the happy path(s)
   - edge cases: empty inputs, boundary values (0, 1, max, off-by-one), `None`, invalid types, duplicates, unicode/whitespace in strings
   - error paths: every documented or reachable exception
   - state/side effects: files written, calls made, mutations
4. **Write the tests** following the rules below.
5. **Run the tests** (`python -m pytest <test file> -q`) and fix failures. A test suite you haven't run isn't done. If a test fails because it exposed a real bug in the code under test, report the bug to the user — do not silently change the test to pass, and do not change production code unless asked.

## Test-writing rules

### Structure and naming
- One behavior per test. Prefer several small tests over one test with many unrelated assertions. (Multiple assertions verifying facets of *one* behavior are fine.)
- Follow **Arrange–Act–Assert**: set up, do the one thing, verify. Separate the phases with a blank line; only add `# Arrange/Act/Assert` comments when the phases aren't obvious.
- Name tests after the behavior, not the function: `test_calculate_discount_applies_10_percent_for_loyal_customer`, not `test_discount_2`. A reader should know what broke from the name alone in a failure report.
- Group related tests for a class or feature in a `class TestX:` block (no `unittest.TestCase` inheritance) when it aids navigation; otherwise plain functions.
- Tests must be independent and order-agnostic: no shared mutable module-level state, no test relying on another having run.

### Assertions
- Use plain `assert` with expressive expressions — pytest's introspection makes `assert result == expected` informative on failure.
- Assert on specific values, not just truthiness: `assert items == ["a", "b"]`, not `assert items`.
- For exceptions, use `pytest.raises` and check the message when it matters:
  ```python
  with pytest.raises(ValueError, match="negative amount"):
      withdraw(account, -5)
  ```
- For floats, use `pytest.approx`. Never compare floats with `==`.
- Don't wrap asserts in try/except and don't put conditionals or loops around assertions — if you're looping over cases, parametrize instead.

### Parametrization
- Use `@pytest.mark.parametrize` whenever the same behavior is checked across multiple inputs. Give cases readable `ids` when values alone aren't self-explanatory:
  ```python
  @pytest.mark.parametrize(
      ("raw", "expected"),
      [("", []), ("a", ["a"]), ("a,b", ["a", "b"]), (" a , b ", ["a", "b"])],
      ids=["empty", "single", "pair", "whitespace"],
  )
  def test_split_tags(raw, expected):
      assert split_tags(raw) == expected
  ```
- Don't parametrize cases that assert *different* behaviors (e.g. success vs. exception) into one test — split them.

### Fixtures
- Use fixtures for shared setup instead of duplicating arrangement code; put fixtures used by multiple files in `conftest.py`.
- Prefer built-in fixtures over hand-rolled setup: `tmp_path` for filesystem work, `monkeypatch` for env vars/attributes, `capsys` for stdout/stderr, `caplog` for logging.
- Keep fixtures small and single-purpose; a fixture that returns a fully-built object beats one that returns half-configured pieces.
- Default to function scope. Only widen scope (`module`/`session`) for genuinely expensive, immutable resources.

### Isolation and mocking
- Unit tests must not touch the network, a real database, the clock, or randomness. Mock or fake those boundaries: `unittest.mock` / `mocker` (pytest-mock), inject a fixed `datetime`, seed or stub `random`.
- Patch where the name is *looked up*, not where it's defined: `mocker.patch("app.orders.get_rate")`, not `"app.rates.get_rate"`.
- **Don't over-mock.** Mock external boundaries only; never mock the unit under test or pure in-process collaborators — a test that mocks everything verifies nothing but its own wiring. If a function needs heavy mocking to test, note that as a design smell (worth telling the user) rather than piling on patches.
- Test behavior, not implementation: assert on return values and observable effects, not on internal call sequences, unless the call itself *is* the contract (e.g. "sends exactly one email").

### Scope and coverage
- Cover the public interface; don't test private helpers directly unless they hold isolated complex logic — they're exercised through the public API.
- Aim for meaningful coverage of branches and failure modes, not a percentage. The error paths and boundaries are where bugs live; a suite that only tests happy paths is decorative.
- Skip testing trivial code (plain dataclasses, straight pass-through wrappers) — say so instead of padding the suite.
- Keep tests deterministic. If you must use time or randomness, control it (freeze time, fix seeds).

### Style
- No logic in tests: no branching, no computed expected values that re-implement the code under test. Expected values are literals.
- A docstring or comment only for genuinely non-obvious scenarios (regression tests should reference the bug: `"""Regression: #142 — empty cart crashed checkout."""`).
- Import the module under test at the top; keep test files runnable with a bare `pytest` from the project root.

## Output

After writing and running the tests, summarize for the user: what's covered, what was deliberately not tested and why, any bugs or design smells the tests exposed, and the pytest command to run them.
