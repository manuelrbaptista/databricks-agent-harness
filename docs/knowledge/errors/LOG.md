# Error log

Canonical error history. New errors are appended here; recurring symptoms update `Seen` and add a `Seen again` line. Individual error files are not used.

<!-- Migrated from 2026-11-27-1050-tools-specify-blocked.md -->

---
date: 2026-11-27 10:50
area: environment-setup
status: open
seen: 1
---

# Spec Kit executable was blocked by Windows Application Control

- Symptom: Running `specify.exe` returned "Uma política de Controlo de Aplicações bloqueou este ficheiro" after `uv tool install specify-cli`.
- Cause: unknown; Windows Application Control policy on this machine prevented execution of the installed executable.
- Fix: none yet; do not bypass or weaken machine policy.
- Prevention: The environment setup skill should keep a separate blocked status and stop Spec Kit verification without trying alternate downloads or execution paths.

---

<!-- Migrated from 2026-11-27-1051-environment-venv-blocked.md -->

---
date: 2026-11-27 10:51
area: environment-setup
status: open
seen: 1
---

# Windows Application Control blocked the Python virtual environment executable

- Symptom: `uv pip install -r requirements-dev.txt -r requirements-docs.txt` failed while uv inspected `.venv\Scripts\python.exe`: "Uma política de Controlo de Aplicações bloqueou este ficheiro" (os error 4551).
- Cause: unknown; Windows Application Control policy on this machine prevented execution of the virtual environment interpreter.
- Fix: none yet; do not bypass or weaken machine policy.
- Prevention: If the same policy blocks Python executables in the project directory, stop setup and require an approved environment or administrator policy change rather than changing security settings.

---

<!-- Migrated from 2026-11-27-1145-databricks-copilot-project-skills.md -->

---
date: 2026-11-27 11:45
area: databricks-official-skills
status: open
seen: 1
---

# GitHub Copilot rejected project-scoped Databricks skills

- Symptom: `databricks aitools install --scope project --agents copilot --skills-only --skills databricks-core,databricks-pipelines,databricks-dabs` failed with `Skipped GitHub Copilot: does not support project-scoped skills`.
- Cause: GitHub Copilot is not a supported project-scope target for the installed Databricks CLI 1.28.0.
- Fix: none; do not bypass the project guardrail or install globally during initialization.
- Prevention: update `skills/databricks/databricks-official-skills/SKILL.md` and `INIT.md` to state that GitHub Copilot cannot receive project-scoped official skills, and tell users to use a supported agent or stop before installation.

---

<!-- Migrated from 2026-11-27-1200-us00-tests-outside-venv.md -->

---
date: 2026-11-27 12:00
area: environment-setup
status: open
seen: 1
---

# US00 checks ran outside the project virtual environment

- Symptom: The notebook engineer reported `pytest` and `ruff` were not recognized when run directly, despite the project `.venv` containing both tools.
- Cause: The commands were run without activating `.venv` or using its `Scripts` executables.
- Fix: Re-run through `.venv\Scripts\pytest.exe` and `.venv\Scripts\ruff.exe`.
- Prevention: Update `agents/notebook-engineer.md` to activate or explicitly use the project virtual environment before running checks.

---

<!-- Migrated from 2026-11-27-1202-auth-status-identity-output.md -->

---
date: 2026-11-27 12:02
area: environment-setup
status: open
seen: 1
---

# Authentication verification output included account identity

- Symptom: A filtered `gh auth status` check printed the authenticated account name despite the request not to show login details.
- Cause: The output filter selected the full “Logged in to ... account” line instead of checking only authentication success and scopes.
- Fix: Avoid printing identity-bearing status lines; verify success and required scopes without echoing account details.
- Prevention: Update `skills/core/environment-setup/SKILL.md` to document identity-safe authentication verification.

---

<!-- Migrated from 2026-11-27-1203-databricks-volume-command.md -->

---
date: 2026-11-27 12:03
area: databricks-cli-auth
status: open
seen: 1
---

# Guessed Databricks volume command was unavailable

- Symptom: The notebook engineer reported that `databricks volumes get` failed; the corrected `databricks volumes read` check passed.
- Cause: The CLI command name was guessed instead of checked against its help.
- Fix: Use the supported `databricks volumes read` command, or the project's read-only SQL wrapper for existence checks.
- Prevention: Update `agents/notebook-engineer.md` to inspect `databricks volumes --help` before invoking unfamiliar CLI subcommands.

---

<!-- Migrated from 2026-11-27-1203-github-status-powershell-quoting.md -->

---
date: 2026-11-27 12:03
area: environment-setup
status: open
seen: 1
---

# PowerShell quoting broke an exploratory GitHub status check

- Symptom: The notebook engineer reported a PowerShell quoting error in the first GitHub status check.
- Cause: unknown; exact shell error was not included in the report.
- Fix: A later filtered `gh auth status` and fork-settings check passed.
- Prevention: Update `agents/notebook-engineer.md` to prefer simple PowerShell commands and verify quoting before running composed status checks.

---

<!-- Migrated from 2026-11-27-1203-venv-pip-check-unavailable.md -->

---
date: 2026-11-27 12:03
area: environment-setup
status: open
seen: 1
---

# Optional pip check was unavailable in the uv virtual environment

- Symptom: `.venv\Scripts\python.exe -m pip check` failed because `pip` is not installed in the environment.
- Cause: The project virtual environment is managed with uv and does not include the `pip` module.
- Fix: The declared dev/docs packages were validated by import checks; project tests and Ruff passed.
- Prevention: Update `agents/notebook-engineer.md` to use the project's uv and test commands instead of assuming `python -m pip` exists.

---

<!-- Migrated from 2026-11-27-1345-pr-ci-no-checks-reported.md -->

---
date: 2026-11-27 13:45
area: github-pr-flow
status: open
seen: 1
---

# GitHub reported no CI checks for the US00 pull request

- Symptom: `gh pr checks` returned `no checks reported` for PR #19; the Actions API showed no runs.
- Cause: unknown. Actions are enabled and the CI workflow file exists on the default branch, but no run was visible.
- Fix: none yet; do not claim CI passed or trigger an alternate workflow to bypass the missing checks.
- Prevention: Update `skills/github/github-pr-flow/SKILL.md` with the safe response when a PR has no reported checks.

---

<!-- Migrated from 2026-11-27-1426-powershell-object-enumeration.md -->

---
date: 2026-11-27 14:26
area: exploration
status: open
seen: 1
---

# PowerShell endpoint inspection used an unsupported object method

- Symptom: `Method invocation failed because [System.Management.Automation.PSCustomObject] does not contain a method named 'GetEnumerator'.`
- Cause: The Swagger `paths` value was deserialized as a `PSCustomObject`, not a dictionary.
- Fix: Inspect its properties via `.PSObject.Properties` instead of calling `GetEnumerator()`.
- Prevention: Check the deserialized object type before using dictionary methods in exploratory PowerShell.

---

<!-- Migrated from 2026-11-27-1430-java-runtime-unavailable.md -->

---
date: 2026-11-27 14:30
area: environment-setup
status: open
seen: 1
---

# Local PySpark runtime has no Java executable

- Symptom: `java : The term 'java' is not recognized ...`
- Cause: A Java runtime is not available on the current PATH; PySpark is installed but cannot run a local Spark session here.
- Fix: None; do not install packages or runtimes without approval. Use unit tests that do not start a local Spark session.
- Prevention: `scripts/check_environment.py` or the project setup checklist should report Java before local Spark execution is attempted.

---

<!-- Migrated from 2026-11-27-1430-powershell-invoke-webrequest-noninteractive.md -->

---
date: 2026-11-27 14:30
area: exploration
status: open
seen: 1
---

# PowerShell web request prompted in noninteractive mode

- Symptom: `Invoke-WebRequest` failed with `Windows PowerShell is in NonInteractive mode. Read and Prompt functionality is not available.`
- Cause: The Windows PowerShell web cmdlet attempted an interactive confirmation while checking remote response headers.
- Fix: Use `curl.exe -I` for a non-interactive header-only request.
- Prevention: Prefer non-interactive HTTP clients for command-line metadata checks.

---

<!-- Migrated from 2026-11-27-1435-bronze-initial-lint.md -->

---
date: 2026-11-27 14:35
area: pipelines
status: open
seen: 2
---

# Initial bronze ingestion lint found style issues

- Symptom: Ruff reported an unsorted import block and three lines over the configured 100-character limit.
- Cause: The new module and tests were not formatted to the repository's Ruff rules before the first lint run.
- Fix: Apply Ruff's import ordering and wrap the reported long lines.
- Prevention: Run Ruff on each focused change before the full suite.
- Seen again: 2026-11-27, lint also flagged Databricks notebook globals (`dbutils`, `spark`, `display`), import placement, and one long test line; the notebook needs a lint-aware structure.

---

<!-- Migrated from 2026-11-27-1435-bronze-test-double-signatures.md -->

---
date: 2026-11-27 14:35
area: pipelines
status: open
seen: 1
---

# Bronze ingestion tests used incomplete test doubles

- Symptom: The focused tests reported keyword-argument errors for `timeout` and expected no directories after a failed download.
- Cause: Test doubles did not accept the production function's optional keywords, and the cleanup assertion incorrectly rejected an empty year directory.
- Fix: Update mocks to accept `**kwargs` and assert that no partial/original files remain, while allowing the containing directory.
- Prevention: Keep injected dependency fakes compatible with the complete production call signature and assert file state rather than directory creation.

---

<!-- Migrated from 2026-11-27-1438-lint-missing-ruff.md -->

---
date: 2026-11-27 14:38
area: testing
status: open
seen: 3
---

# Ruff check is blocked by missing Ruff

- Symptom: `python -m ruff check ...` returned `No module named ruff`.
- Cause: The selected Python 3.15 interpreter does not have the development requirements installed.
- Fix: Ran Ruff with the existing `.venv\Scripts\python.exe`, which has Ruff installed; no package installation was needed.
- Prevention: Invoke the project interpreter explicitly rather than the system Python selected on PATH.
- Seen again: 2026-11-27, the Ruff command was invoked from PATH and again was not installed for the system Python environment.
- Seen again: 2026-11-27, `ruff check` was invoked from PATH during the US01 notebook import fix and the command was not recognized.

---

<!-- Migrated from 2026-11-27-1438-test-missing-pyspark.md -->

---
date: 2026-11-27 14:38
area: testing
status: open
seen: 3
---

# Focused pytest collection is blocked by missing PySpark

- Symptom: `ModuleNotFoundError: No module named 'pyspark'` while collecting `tests/test_bronze_contracts.py`.
- Cause: The selected Python 3.15 interpreter does not have the development requirements installed.
- Fix: Ran tests with the existing `.venv\Scripts\python.exe`, which has PySpark installed; no package installation was needed.
- Prevention: Invoke the project interpreter explicitly rather than the system Python selected on PATH.
- Seen again: 2026-11-27, system `python -m pytest` was used for the focused bronze contracts test run and again lacked PySpark.
- Seen again: 2026-11-27, `pytest` was invoked from PATH during the US01 notebook import fix and the command was not recognized.

---

<!-- Migrated from 2026-11-27-1450-databricks-docs-url-404.md -->

---
date: 2026-11-27 14:50
area: databricks-bundles-dev
status: open
seen: 2
---

# Guessed Databricks bundle documentation URLs returned 404

- Symptom: Fetching two guessed Databricks documentation URLs returned HTTP 404.
- Cause: The documentation paths were not verified before fetching.
- Fix: Use the CLI help output for sync and plan options; locate the current official documentation URL before relying on web guidance.
- Prevention: Verify official documentation paths before fetching them.
- Seen again: 2026-11-27, a guessed notebook-context documentation URL returned HTTP 404; the failure was not used as implementation guidance.

---

<!-- Migrated from 2026-11-27-1501-bundle-sync-directory-actions.md -->

---
date: 2026-11-27 15:01
area: databricks-bundles-dev
status: open
seen: 1
---

# Sync verification counted directories as unexpected files

- Symptom: A post-sync assertion reported two unexpected entries, `notebooks` and `src`.
- Cause: The Databricks CLI reports newly created workspace directories alongside uploaded files.
- Fix: The sync itself completed; the dry-run had listed only the three approved files. Verify directory creation separately from uploaded file paths.
- Prevention: Normalize `Uploaded` output into files and parent directories before comparing it with the approved sync list.

---

<!-- Migrated from 2026-11-27-1510-us01-serverless-run-submit.md -->

---
date: 2026-11-27 15:10
area: databricks-bundles-dev
status: open
seen: 1
---

# US01 one-time serverless run submission failed

- Symptom: `databricks jobs submit` returned exit code 1 for the approved one-time notebook run.
- Cause: PowerShell passed the inline JSON argument in a form the Databricks CLI could not decode. A retry with the same idempotency token returned `invalid character 'a' in literal true (expecting 'r')`; no run ID was returned.
- Fix: Retry the same approved request with the same idempotency token, using Python's argument-list handling to preserve the JSON argument, then inspect only the run ID/state.
- Prevention: Avoid inline JSON arguments from PowerShell when invoking the Databricks CLI; pass a correctly quoted argument and capture only sanitized output.

---

<!-- Migrated from 2026-11-27-1512-databricks-get-run-positional-id.md -->

---
date: 2026-11-27 15:12
area: databricks-cli
status: open
seen: 1
---

# Databricks CLI get-run rejected the run-id flag

- Symptom: `databricks jobs get-run --run-id <id>` exited with `unknown flag: --run-id`.
- Cause: This CLI version accepts the run ID as a positional argument.
- Fix: Use `databricks jobs get-run <id>`.
- Prevention: Check the subcommand usage when a CLI flag is rejected; this command's usage is `databricks jobs get-run RUN_ID`.

---

<!-- Migrated from 2026-11-27-1518-databricks-multitask-run-output.md -->

---
date: 2026-11-27 15:18
area: databricks-cli
status: open
seen: 1
---

# Run output requires the individual task run ID

- Symptom: `databricks jobs get-run-output <parent-run-id>` returned `Retrieving the output of runs with multiple tasks is not supported`.
- Cause: The one-time run is represented as a multi-task run, and output retrieval requires an individual task run ID.
- Fix: Read the task run ID from `jobs get-run`, then query output for that task.
- Prevention: For multi-task runs, retrieve output using the task run ID rather than the parent run ID.

---

<!-- Migrated from 2026-11-27-1524-us01-workspace-import-src.md -->

---
date: 2026-11-27 15:24
area: databricks-bundles-dev
status: open
seen: 1
---

# Databricks notebook could not import the synced source package

- Symptom: The US01 notebook run failed with `ModuleNotFoundError: No module named 'src'`.
- Cause: The workspace notebook runtime did not include the synced `src` directory on Python's import path.
- Fix: The notebook now requires an absolute `project_root` parameter, verifies it contains the synced `src` package, and adds it to `sys.path` before importing. A focused local regression test exercises the parameter behavior. The workspace run has not been repeated.
- Prevention: Pass the validated bundle `workspace.file_path` as `project_root`; serverless compute does not guarantee a working directory.

---

<!-- Migrated from 2026-11-27-1535-us01-notebook-dynamic-import-lint.md -->

---
date: 2026-11-27 15:35
area: testing
status: open
seen: 1
---

# Ruff flagged the workspace-rooted notebook import

- Symptom: `ruff check .` reported `E402 Module level import not at top of file` for the import after `sys.path.insert`.
- Cause: The Databricks notebook must add the runtime-supplied absolute workspace root before importing its `src` package.
- Fix: None yet. Mark the intentionally delayed import with a narrowly scoped `# noqa: E402` and rerun lint.
- Prevention: Keep the suppression limited to the required workspace import; do not disable E402 for the notebook or repository.

---

<!-- Migrated from 2026-11-27-1545-sync-dry-run-changed-files-only.md -->

---
date: 2026-11-27 15:45
area: databricks-bundles-dev
status: open
seen: 1
---

# Sync dry-run listed only the changed notebook

- Symptom: A local scope check expected all three US01 files, but `databricks bundle sync --dry-run` listed only `notebooks/bronze_contracts.py` and the check exited with code 2.
- Cause: The source module and package initializer were already synced and unchanged; this dry-run listed only the file needing an update.
- Fix: Treat the output as a change set, not a complete workspace inventory. The dry-run performed no upload.
- Prevention: Compare the dry-run with the expected changed files for the current edit, and use a read-only workspace inventory if a full remote inventory is needed.

---

<!-- Migrated from 2026-11-27-1600-us01-workspace-run-failed-again.md -->

---
date: 2026-11-27 16:00
area: databricks-bundles-dev
status: open
seen: 2
---

# US01 one-time dev run failed after the import-path correction

- Symptom: The approved one-time run (parent run `1028879162673046`) failed, and the required P1 retry also could not resolve `dados.gov.pt` (`Temporary failure in name resolution`).
- Cause: Serverless DNS resolution for `dados.gov.pt` failed on both attempts. The retry's catalog HEAD and GET failed; the ZIP endpoint was not reached.
- Fix: Stopped before deployment or ingestion after the required retry. No network workaround was used.
- Prevention: Verify source reachability from the selected compute before ingestion; when outbound DNS/network access is blocked, record the limitation and stop.
- Seen again: 2026-11-27, required serverless P1 retry; DNS, catalog HEAD, and catalog GET failed, ZIP not reached.

---

<!-- Migrated from 2026-11-27-1610-us01-upstream-branch-deleted.md -->

---
date: 2026-11-27 16:10
area: github-issues-flow
status: open
seen: 1
---

# Upstream US01 head branch was unavailable after merge

- Symptom: `git fetch` for `refs/heads/feat/2-contratos-bronze` returned `couldn't find remote ref`.
- Cause: The PR is merged and its source branch is no longer available as a branch ref.
- Fix: None yet. Fetch the merged PR commit or `refs/pull/26/head` without changing the existing worktree.
- Prevention: When a referenced PR branch has been deleted, use its immutable PR ref or merge commit rather than retrying the deleted branch name.

---

<!-- Migrated from 2026-11-27-1620-workspace-get-status-path-prefix.md -->

---
date: 2026-11-27 16:20
area: databricks-cli
status: open
seen: 1
---

# Workspace get-status rejected the bundle filesystem path

- Symptom: `databricks workspace get-status` returned `doesn't exist` for both the bundle filesystem path and its corresponding API-style path to the prior synced notebook.
- Cause: The expected notebook object is absent at the bundle path; the exact reason the earlier sync output did not correspond to a visible object remains unknown.
- Fix: The active profile's `/Users/<user>` directory is verified; use a new uniquely named temporary probe there, not the unverified bundle path.
- Prevention: Verify remote object existence after sync/deploy, and distinguish bundle filesystem paths from Workspace API object paths.

---

## 2026-11-27 16:19 — Quick validation ran with Python missing PyYAML

- Area: testing
- Status: open
- Seen: 1
- Symptom: `python scripts/quick_validate.py skills/core/self-improvement` failed with `ModuleNotFoundError: No module named 'yaml'`.
- Cause: The selected system Python environment does not have PyYAML installed.
- Fix: None yet; check whether the project virtual environment has the dependency and use it if available.
- Prevention: Run repository validators with the configured project Python environment rather than an unrelated system interpreter.
