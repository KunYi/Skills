## Commit Message Format

### Structure

<type>(<scope>): <summary>

<why>

- key change 1
- key change 2

### Constraints

| Part | Constraint |
|------|------------|
| Title format | `<type>(<scope>): <summary>` |
| Title length | ≤ 72 chars (MUST), ≤ 52 chars (preferred) |
| Title style | imperative mood, no trailing period |
| Body paragraph 1 | `<why>` - explain the reason |
| Body bullet points | key changes, one per line |
| Body line length | ≤ 120 chars |

### Rules (MUST follow)
- No **Co-Authored-By** or attribution lines
- Commit must appear as solely from me

### Example

```text
feat(parser): add JSON schema validation

Need to validate config files before processing.
- Add validate() method to Parser class
- Support draft-07 by default
- Return detailed error messages
```
