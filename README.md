# Overview

## Details

| Developed by | Aryn |
| --- | --- |
| Date of development | Feb 15, 2024 |
| Validator type | Summarization |
| Blog |  |
| License | Apache 2 |
| Input/Output | Output |

## Description

This validator checks if a summary generated by an LLM is an extractive summary of the original document. An extractive summary contains words and phrases selected from an original document to create a summary.

This validator works by performing a fuzzy match between the sentences in the summary and the sentences in the document. Each sentence in the summary must be similar to at least one sentence in the document. After the validation, the summary is updated to include the sentences from the document that were matched, and the citations for those sentences are added to the end of the summary.

## Example Usage Guide

### Installation

```bash
$ guardrails hub install hub://aryn-ai/extractive_summary
```

### Initialization

```python
extractive_summary_validator = ExtractiveSummary(
	threshold=90,
	filepaths="/path/to/original/documents"
)

# Create Guard with Validator
guard = Guard.from_string(
    validators=[extractive_summary_validator, ...],
    num_reasks=2,
)
```

### Invocation

```python
guard(
    "Summarized text",
)
```

## Intended use

- Primary intended uses: This validator is only useful when performing summarization.
- Out-of-scope use cases: If the summary is correct but is an abstractive summary, this validator will give false negatives.

## Expected deployment metrics

|  | CPU | GPU |
| --- | --- | --- |
| Latency |  | - |
| Memory |  | - |
| Cost |  | - |
| Expected quality |  | - |

## Resources required

- Dependencies: `thefuzz`
- Foundation model access keys: N/A
- Compute: N/A

## Validator Performance

### Evaluation Dataset

-

### Model Performance Measures

| Accuracy | - |
| --- | --- |
| F1 Score | - |

### Decision thresholds

-
