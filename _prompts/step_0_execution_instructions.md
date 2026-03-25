# Step Execution Rules

* Read all files in this folder. Each file has the prefix `step_{number}`.
* Execute the steps in ascending order (e.g., `step_1`, `step_2`, etc.).

## Execution Tracking

* After executing a step (e.g., `step_1`), add it under the `done` section.
* Do not add `step_0` to the `done` section, as it represents this instruction file.

## Re-run Behavior

* On subsequent runs, check which steps are not listed under `done`.
* Execute only the steps that have not yet been completed.

## Example

```yaml
done:
  - step: step_1
    comment: initial project setup completed

  - step: step_2
    comment: modules structure created
```

---

## Done

```yaml
done: []
```
