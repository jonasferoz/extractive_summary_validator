# Overview

| Developed by | Aryn |
| Date of development | Feb 15, 2024 |
| Validator type | Summarization |
| Blog |  |
| License | Apache 2 |
| Input/Output | Output |

## Description

### Intended use
This validator checks if a summary generated by an LLM is an extractive summary of the original document. An extractive summary contains words and phrases selected from an original document to create a summary.

This validator works by performing a fuzzy match between the sentences in the summary and the sentences in the document. Each sentence in the summary must be similar to at least one sentence in the document. After the validation, the summary is updated to include the sentences from the document that were matched, and the citations for those sentences are added to the end of the summary.

This validator is only useful when performing extractive summarization. If the summary is correct but is an abstractive summary, this validator will give false negatives.

### Requirements

* Dependencies:
	- guardrails-ai>=0.4.0
    - thefuzz

## Installation

```bash
$ guardrails hub install hub://aryn-ai/extractive_summary
```

## Usage Examples

### Validating string output via Python

In this example, we apply the validator to a string output generated by an LLM.

```python
# Import Guard and Validator
from guardrails.hub import ExtractiveSummary
from guardrails import Guard

val = ExtractiveSummary(
    threshold=90,
    filepaths="/path/to/original/documents"
)

# Create Guard with Validator
guard = Guard.from_string(validators=[val, ...])

guard.parse("Summarized text")  # Validator passes
guard.parse("Incorrect summary")  # Validator fails
```

### Validating JSON output via Python

In this example, we apply the validator to a string field of a JSON output generated by an LLM.

```python
# Import Guard and Validator
from pydantic import BaseModel
from guardrails.hub import ExtractiveSummary
from guardrails import Guard

val = ExtractiveSummary(
    threshold=90,
    filepaths="/path/to/original/documents"
)

# Create Pydantic BaseModel
class ArticleSummary(BaseModel):
    title: str
    summary: str = Field(
        description="Summary of text", validators=[val]
    )

# Create a Guard to check for valid Pydantic output
guard = Guard.from_pydantic(output_class=ArticleSummary)

# Run LLM output generating JSON through guard
guard.parse("""
{
    "pet_name": "Using Guardrails Hub",
    "pet_type": "To use Guardrails Hub, download individual validators using the CLI and compose them together into guards."
}
""")
```

# API Reference

**`__init__(self, on_fail="noop")`**
<ul>
Initializes a new instance of the Validator class.

**Parameters**
- **`threshold`** *(int)*: The minimum fuzz ratio to be considered summarized.  Defaults to 85.
- **`filepaths`** *(str)*: The filepaths of the original document and the extracted summary sentences. NOTE - using the playground, only one file can be uploaded at a time.
- **`on_fail`** *(str, Callable)*: The policy to enact when a validator fails. If `str`, must be one of `reask`, `fix`, `filter`, `refrain`, `noop`, `exception` or `fix_reask`. Otherwise, must be a function that is called when the validator fails.
</ul>
<br/>

**`__call__(self, value, metadata={}) -> ValidationOutcome`**
<ul>
Validates the given `value` using the rules defined in this validator, relying on the `metadata` provided to customize the validation process. This method is automatically invoked by `guard.parse(...)`, ensuring the validation logic is applied to the input data.

Note:

1. This method should not be called directly by the user. Instead, invoke `guard.parse(...)` where this method will be called internally for each associated Validator.
2. When invoking `guard.parse(...)`, ensure to pass the appropriate `metadata` dictionary that includes keys and values required by this validator. If `guard` is associated with multiple validators, combine all necessary metadata into a single dictionary.

**Parameters**
- **`value`** *(Any)*: The input value to validate.
- **`metadata`** *(dict)*: A dictionary containing metadata required for validation. Keys and values must match the expectations of this validator.

</ul>
