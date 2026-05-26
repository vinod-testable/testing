---
Description: Enforce prompt template for all user messages.
alwaysApply: true
---

## Prompt Template Enforcement - testing

Before responding to ANY user request, validate that the message contains the required structure.

### Required Template:
The user MUST include at minimum:

**Objective:** (required)
A clear, specific task description. Placeholders like "[Clearly state the exact task]" are NOT acceptable.

**Scope:** (required)
What should and should not be modified.

**Constraints:** (required)
At least one constraint listed.

**Output:** (required)
What the expected output format is.

**Context:** (optional)
Only needed if extra background is required.

---

### Validation Rules:

1. If the user message is missing ANY required section (Objective, Scope, Constraints, Output) → DO NOT answer. Respond only with the rejection message below.

2. If the user has left placeholder text like `[Clearly state the exact task]` unfilled → DO NOT answer. Respond only with the rejection message below.

3. If the user asks a casual/vague question without the template → DO NOT answer. Respond only with the rejection message below.

---

### Rejection Response (use this exactly):

> ⚠️ **Your prompt does not follow the required template.**
> Please restructure your request using the format below before I can assist:
>
> ```
> Objective:
> [Clearly state the exact task]
>
> Scope:
> Only modify the selected code block.
> Do not modify unrelated code, files, or logic.
>
> Context: (optional)
> [Only if required – provide minimal necessary context]
>
> Constraints:
> - Must compile successfully
> - Must follow existing code style and patterns
> - Must not introduce breaking changes
> - Must not add unnecessary abstractions
> - Must keep changes minimal and focused
>
> Output:
> Provide only the required code changes.
> Do not regenerate entire file unless explicitly requested.
> ```

---

### If template is followed correctly:
Proceed with the response strictly within the defined Scope and Constraints.
```

---

## How it behaves

| User message | Agent response |
|---|---|
| `"fix my login function"` | ❌ Rejects, shows template |
| `"Objective: [Clearly state...]"` | ❌ Rejects, placeholder not filled |
| Correct template filled out | ✅ Answers normally |

---

## Tip — also add this to your `.cursorrules`

So even the base rules file reinforces it:
```
All responses must validate the prompt template before answering.
If the template is not followed, reject and show the template. Do not answer the question.
