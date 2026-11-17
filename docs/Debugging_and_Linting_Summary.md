# Debugging and Linting Summary

## Why This Matters
Broken builds and slow reviews usually come from two habits: submitting code that was never checked, and guessing at the cause of a bug instead of observing it. Linting catches problems before code leaves your working copy. Interactive debugging lets you watch the program run so you diagnose root causes systematically instead of by trial and error.

## Key Terms
- **Linter**: a tool that automatically scans source code for style violations and likely mistakes without running it.
- **Regression**: a bug where something that used to work stops working after a change.
- **Debugger**: a tool, built into the IDE, that lets you pause a running program and examine it.
- **Breakpoint**: a marker you place on a line of code that tells the debugger to pause execution just before that line runs.
- **Call stack**: the ordered list of functions currently being executed, with the most recent call on top and the caller of each function below it.
- **Stack trace**: the printed report of the call stack at the moment an error occurred, showing the sequence of calls that led to it.
- **Stepping**: running the program one line at a time while paused.

## Part 1: Pre-Submission Verification with Linters

### Core Idea
Run the linter on your working copy **before** you stage and commit. A clean lint run is a required step before opening a pull request, because it guards the upstream baseline against regressions and avoids wasted reviewer time.

### Required Practice
1. Save all files in the IDE.
2. Run the linter locally on the files you changed, for example for Python:
   - `flake8 config/metrics_config.py`
3. Run it on the whole project before opening a pull request:
   - `flake8 .`
4. Read each message. It shows the file, line number, error code and a short description.
5. Fix every reported problem in the working copy, then run the linter again.
6. Repeat until the linter prints **no output**.
7. Only then run `git add` and `git commit`.

### Rules
- Do not open a pull request while the linter reports errors.
- Do not silence a warning without a written reason in the pull request.
- Fix only what you are working on; avoid unrelated formatting changes, which make diffs harder to review.

## Part 2: Interactive Debugging

### Core Idea
Instead of adding print statements, pause the program at a chosen line and inspect its real state: variable values and the chain of calls that reached that point.

### Step 1: Insert a Breakpoint in the IDE
1. Open the source file in the IDE (for example VS Code).
2. Find the line where you want execution to pause, usually just before the suspected defect.
3. Click in the **gutter** (the space to the left of the line number), or put the cursor on the line and press **F9**.
4. A **red dot** appears. This is the breakpoint.
5. Start the program in debug mode (**F5**, or Run > Start Debugging).
6. Execution runs normally until it reaches the breakpoint, then **pauses before that line executes**.
7. Click the red dot again (or press F9) to remove the breakpoint.

### Step 2: Control Execution While Paused
| Action | Typical key | Meaning |
|---|---|---|
| Continue | F5 | Run until the next breakpoint |
| Step Over | F10 | Run the current line without entering called functions |
| Step Into | F11 | Enter the function called on the current line |
| Step Out | Shift+F11 | Finish the current function and return to its caller |

### Step 3: Inspect the Call Stack
1. Open the **Call Stack** panel in the debugger view.
2. The **top frame** is the function currently paused. The frames below it are its callers, in order.
3. Click a frame to jump to that line in the code and see its local variables in the **Variables** panel.
4. Move down the stack, asking at each frame: "Are the values passed in what I expected?"
5. The root cause is usually the first frame where a value stops being correct.

### Step 4: Read a Stack Trace
When a program fails, it prints a stack trace. In Python, **the most recent call is at the bottom**.
1. Read the **last line first**: it names the error type and message.
2. Look at the frame just above it: this is where the error was raised.
3. Walk upward through the frames to see who called whom.
4. Find the nearest frame that is **your own project code** (not a library). This is the best place to place a breakpoint.
5. Re-run in debug mode with a breakpoint there, and use the call stack to confirm what led to the failure.

### Systematic Diagnosis Method
1. Reproduce the defect reliably.
2. Read the stack trace to locate where the error is raised.
3. Place a breakpoint just before that location.
4. Inspect variables and the call stack at the pause.
5. Form one hypothesis, test it by stepping, and confirm or reject it.
6. Fix the working copy, run the linter, and re-test before committing.
