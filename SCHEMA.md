# Question JSON Schema

Every file under `questions/` is a single JSON object representing one Moodle question. The shape mirrors what the [Moodle Question Forge](https://github.com/danielcregg/moodle-question-forge) app stores internally, minus personal/session fields.

## Required top-level fields

| Field | Type | Notes |
|---|---|---|
| `name` | string | Short title (5-10 words). |
| `question_type` | string | One of `multichoice`, `truefalse`, `shortanswer`, `numerical`, `essay`, `matching`, `description`. |
| `question_text` | string (HTML) | The question stem shown to the student. |
| `difficulty` | string | `easy`, `medium`, or `hard`. |
| `topic` | string | One-word tag like `arrays`, `recursion`, `thermodynamics`. Used for the topic filter in the Public Bank. |
| `submitter` | string | `@github-handle` of the contributor (auto-filled by the app). |
| `submitted_at` | string | ISO 8601 timestamp. |
| `validation_score` | integer 1-10 | AI review score at time of submission. Must be ≥ 8. |

## Type-specific fields

### `multichoice`
```json
{
  "single": true,
  "shuffle_answers": true,
  "answer_numbering": "abc",
  "answers": [
    { "text": "Correct option", "fraction": 100, "feedback": "Why it's right." },
    { "text": "Distractor 1", "fraction": 0, "feedback": "Why it's wrong." }
  ]
}
```

### `truefalse`
```json
{ "answer": "true", "true_feedback": "...", "false_feedback": "..." }
```

### `shortanswer`
```json
{
  "use_case": false,
  "answers": [
    { "text": "accepted answer", "fraction": 100, "feedback": "..." }
  ]
}
```

### `numerical`
```json
{
  "answers": [
    { "value": 3.14, "error": 0.01, "fraction": 100, "feedback": "..." }
  ]
}
```

### `essay`
```json
{
  "response_format": "editor",
  "response_field_lines": 15,
  "grader_info": "Rubric for the marker."
}
```

### `matching`
```json
{
  "shuffle_answers": true,
  "subquestions": [
    { "question": "Stem 1", "answer": "Match 1" }
  ],
  "distractors": ["Red herring 1"]
}
```

### `description`
No additional fields.

## Optional fields

- `general_feedback` (HTML) — shown after any attempt.
- `tags` (string array) — extra filterable tags.
- `hints` (array) — Moodle hint objects `{ text, show_num_correct, clear_wrong }`.
- `default_grade` (number, default 1).
- `penalty` (number 0-1, default 0).
- `bloom_level` — one of `remember`, `understand`, `apply`, `analyze`, `evaluate`, `create`.

## Filename convention

`<question_type>-<url-safe-slug>-<8char-hash>.json`

E.g. `multichoice-binary-search-complexity-a3f9c02b.json`.

The hash is the last 8 characters of the question's `idnumber` (or a random 8-char suffix). This keeps filenames unique even if two contributors submit similarly-named questions.
