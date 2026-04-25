Title:\*\* `[FEAT] Add input validation to task creation and completion`

**Summary:**  
Currently the task manager accepts any input without checking whether it is valid, meaningful, or safe. This Issue adds validation gates to the three write operations: creating a task, completing a task, and logging an event.

**Motivation:**  
Without validation, the system can silently corrupt files with blank tasks, duplicate entries, or malformed log lines. This has already caused one incident where a blank task was created by an accidental key press and then moved to `done.txt`, making the completion log meaningless. Validation prevents this class of error at the source.

**Acceptance Criteria:**

- [ ] Creating a blank task raises a clear error and does not write to `tasks.txt`
- [ ] Task text longer than 200 characters is truncated with a warning, not silently accepted
- [ ] Completing a task that does not exist in `tasks.txt` raises a `TaskNotFoundError`
- [ ] Log entries without a timestamp are rejected by the logger
- [ ] All validation errors produce human-readable messages with guidance on correct usage
- [ ] Validation is covered by at least eight unit tests

**Tasks:**

- [ ] Audit current write operations to identify all unvalidated inputs
- [ ] Write `ValidationError` base class and specific subclasses
- [ ] Implement `validate_task_text()` — checks blank, length, character encoding
- [ ] Implement `validate_task_exists()` — checks `tasks.txt` before completion
- [ ] Implement `validate_log_entry()` — checks timestamp format, required fields
- [ ] Wire validation into the three write operation functions
- [ ] Write eight unit tests covering happy path and error cases
- [ ] Update README with validation rules and error message reference

**Notes / Decisions:**  
Considered raising exceptions vs. returning error codes. Decided on exceptions because the calling code should not be able to silently ignore validation failures. The error messages will include the invalid value and a description of what was expected, following the pattern used by Python's `argparse` library.
