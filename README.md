# GL-3702 Workflow QA Repository

This minimal repository is designed to independently validate the fix for GL-3702, which prevents a `SyntaxError: Unexpected end of JSON input` crash when the code formatting workflow skips.

## How to Test the Scenarios

### Success Scenario
1. Modify `hello.py`, `sample.yml`, or `test.sh` while preserving correct formatting.
2. Commit and push the changes, or trigger the workflow via the **Actions** tab (`workflow_dispatch`).
3. The formatters will run and detect no necessary changes. The workflow will report **Success**.

### Failure Scenario
1. Introduce a deliberate formatting or linting violation (e.g., a ShellCheck warning in `test.sh` or unformatted Python code).
2. Commit and push the changes.
3. The formatters will detect issues or fail the lint check. The workflow will report **Failure**, and if attached to a PR, will receive a comment detailing the error. The JSON parse crash will not occur.

### Skipped Scenario (The original bug)
1. Edit `.github/workflows/test-trigger.yml`.
2. Change all formatting inputs to `false`:
   ```yaml
      python_formatting: false
      shell_formatting: false
      yaml_formatting: false
   ```
3. Commit and push the changes.
4. The formatting job will evaluate `languages == "[]"` and naturally skip execution. The `check-status` job will catch the skipped status and properly exit. The JSON parse crash will not occur.
<img width="1024" height="500" alt="Frame 1" src="https://github.com/user-attachments/assets/94737a2a-6302-4a1d-b6e0-430f587b0de2" />
<img width="512" height="512" alt="icon" src="https://github.com/user-attachments/assets/2d8b05e5-480a-490d-921f-551d21a37aff" />
<img width="1024" height="500" alt="First" src="https://github.com/user-attachments/assets/000711b0-f016-45bc-8b99-da0739f137ea" />
<img width="1024" height="500" alt="Frame 1 (1)" src="https://github.com/user-attachments/assets/93b5460f-84e9-4a2d-a5c7-8cdd26d7b554" />
<img width="1024" height="500" alt="Frame 1 (2)" src="https://github.com/user-attachments/assets/e115389e-caf4-4a64-9372-efac88b2b7e6" />
<img width="1024" height="500" alt="Frame 1 (3)" src="https://github.com/user-attachments/assets/c7ef5344-e7b5-45c8-9142-e8f0b246d878" />
<img width="1024" height="500" alt="Frame 1 (4)" src="https://github.com/user-attachments/assets/30ab53b2-eeb8-4bc8-9334-22eee80f4f49" />
<img width="1024" height="500" alt="Frame 1 (5)" src="https://github.com/user-attachments/assets/f70bb65e-98ce-4a9b-bc80-ae278f0960fc" />


