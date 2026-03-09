# JSON Schemas for Skill Evaluation

This document defines the JSON structures used throughout the skill evaluation system.

## evals.json

The main evaluation definition file.

```json
{
  "skill_name": "example-skill",
  "evals": [
    {
      "id": 1,
      "prompt": "The user's task prompt",
      "expected_output": "Description of what success looks like",
      "files": [
        {
          "path": "input/sample.csv",
          "description": "Input data file"
        }
      ],
      "assertions": [
        {
          "text": "Output contains a valid CSV file",
          "type": "file_exists"
        },
        {
          "text": "CSV has at least 100 rows",
          "type": "quantitative"
        }
      ]
    }
  ]
}
```

### Fields

- `skill_name`: Identifier for the skill being tested
- `evals`: Array of evaluation cases
  - `id`: Unique identifier (integer)
  - `prompt`: The exact task to give Claude
  - `expected_output`: Human description of success criteria
  - `files`: Optional array of input files
    - `path`: Path relative to eval directory
    - `description`: What this file is for
  - `assertions`: Array of testable claims about output
    - `text`: Human-readable assertion
    - `type`: Optional categorization (file_exists, quantitative, format, etc.)

## eval_metadata.json

Per-eval metadata stored alongside outputs.

```json
{
  "eval_id": 1,
  "eval_name": "basic-csv-processing",
  "prompt": "The user's task prompt",
  "assertions": [
    {
      "text": "Output contains a valid CSV file",
      "type": "file_exists"
    }
  ]
}
```

## grading.json

Results of evaluating assertions against outputs.

```json
{
  "summary": {
    "passed": 4,
    "failed": 1,
    "total": 5,
    "pass_rate": 0.8
  },
  "expectations": [
    {
      "text": "Output contains a valid CSV file",
      "passed": true,
      "evidence": "Found output.csv in outputs directory"
    },
    {
      "text": "CSV has at least 100 rows",
      "passed": false,
      "evidence": "CSV has 87 rows after header"
    }
  ]
}
```

### Fields

- `summary`: Aggregate statistics
  - `passed`: Count of passing assertions
  - `failed`: Count of failing assertions
  - `total`: Total assertions
  - `pass_rate`: Ratio of passed/total
- `expectations`: Per-assertion results
  - `text`: The assertion text (must match eval assertion)
  - `passed`: Boolean result
  - `evidence`: Specific evidence from output

## timing.json

Performance data captured from subagent runs.

```json
{
  "total_tokens": 84852,
  "duration_ms": 23332,
  "total_duration_seconds": 23.3
}
```

### Fields

- `total_tokens`: Tokens used by the subagent
- `duration_ms`: Wall-clock time in milliseconds
- `total_duration_seconds`: Human-readable duration

## benchmark.json

Aggregate comparison across configurations.

```json
{
  "metadata": {
    "skill_name": "example-skill",
    "timestamp": "2024-01-15T10:30:00Z",
    "evals_run": ["basic-csv-processing", "large-dataset", "edge-cases"],
    "runs_per_configuration": 3
  },
  "run_summary": {
    "with_skill": {
      "pass_rate": {"mean": 0.85, "stddev": 0.05},
      "time_seconds": {"mean": 45.2, "stddev": 8.3},
      "total_tokens": {"mean": 75000, "stddev": 12000}
    },
    "without_skill": {
      "pass_rate": {"mean": 0.62, "stddev": 0.12},
      "time_seconds": {"mean": 52.1, "stddev": 15.6},
      "total_tokens": {"mean": 92000, "stddev": 18000}
    },
    "delta": {
      "pass_rate": "+23%",
      "time_seconds": "-6.9s",
      "total_tokens": "-17000"
    }
  },
  "per_eval": [
    {
      "eval_name": "basic-csv-processing",
      "with_skill": {
        "pass_rate": {"mean": 0.90, "stddev": 0.00},
        "time_seconds": {"mean": 32.1, "stddev": 4.2}
      },
      "without_skill": {
        "pass_rate": {"mean": 0.70, "stddev": 0.10},
        "time_seconds": {"mean": 38.5, "stddev": 6.8}
      }
    }
  ],
  "notes": [
    "With-skill version shows consistent performance across evals",
    "Without-skill has higher variance, especially on edge cases"
  ]
}
```

### Fields

- `metadata`: Information about the benchmark run
  - `skill_name`: Name of skill being tested
  - `timestamp`: ISO 8601 timestamp
  - `evals_run`: List of eval names included
  - `runs_per_configuration`: Number of runs per config
- `run_summary`: Aggregate statistics per configuration
  - `with_skill` / `without_skill` / `old_skill` / `new_skill`: Configuration names
    - `pass_rate`: Pass rate statistics
    - `time_seconds`: Duration statistics
    - `total_tokens`: Token usage statistics
  - `delta`: Difference between configurations (as string with +/-)
- `per_eval`: Breakdown by individual eval
- `notes`: Analyst observations

## feedback.json

User feedback from the eval viewer.

```json
{
  "reviews": [
    {
      "run_id": "basic-csv-processing-with_skill",
      "feedback": "The output looks good but missing column headers",
      "timestamp": "2024-01-15T10:45:00Z"
    }
  ],
  "status": "complete"
}
```

### Fields

- `reviews`: Array of feedback entries
  - `run_id`: Identifier for the run (format: `<eval-name>-<config>`)
  - `feedback`: User's written feedback (empty string if no issues)
  - `timestamp`: When feedback was given
- `status`: "in_progress" or "complete"

## Directory Structure

```
skill-name-workspace/
├── iteration-1/
│   ├── eval-0/
│   │   ├── eval_metadata.json
│   │   ├── with_skill/
│   │   │   ├── outputs/
│   │   │   ├── grading.json
│   │   │   └── timing.json
│   │   └── without_skill/
│   │       ├── outputs/
│   │       ├── grading.json
│   │       └── timing.json
│   ├── benchmark.json
│   └── feedback.json
├── iteration-2/
│   └── ...
└── skill-snapshot/  # For existing skill improvement
    └── SKILL.md
```