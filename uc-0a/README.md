# UC-0A — Complaint Classifier
Core failure modes: Taxonomy drift · Severity blindness · Missing justification · Hallucinated sub-categories · False confidence on ambiguity

## Classification Schema
| Field | Allowed values | Rule |
| :--- | :--- | :--- |
| **category** | Pothole · Flooding · Streetlight · Waste · Noise · Road Damage · Heritage Damage · Heat Hazard · Drain Blockage · Other | Exact strings only — no variations |
| **priority** | Urgent · Standard · Low | Urgent if severity keywords present |
| **reason** | One sentence | Must cite specific words from description |
| **flag** | NEEDS_REVIEW or blank | Set when category is genuinely ambiguous |

**Severity keywords that must trigger Urgent:** `injury`, `child`, `school`, `hospital`, `ambulance`, `fire`, `hazard`, `fell`, `collapse`

## Usage Instructions
Run the following command from the `uc-0a` directory:
```bash
python classifier.py --input ../data/city-test-files/test_NaviMumbai.csv --output results_NaviMumbai.csv
```

## Skills to Define in skills.md
- **classify_complaint** — one complaint row in → category + priority + reason + flag out
- **batch_classify** — reads input CSV, applies classify_complaint per row, writes output CSV

## What Will Fail From the Naive Prompt
Run "Classify this citizen complaint by category and priority." first. Then look for:
1. Category names that vary across rows for the same type of complaint.
2. Injury/child/school complaints classified as Standard instead of Urgent.
3. No reason field or multi-sentence reasoning in the output.
4. Category names that are not in the allowed list above.
5. Confident classification on genuinely ambiguous complaints.
