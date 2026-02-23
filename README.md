# File Descriptions
## 1. part4_structured_engine.py

- Loads `sciences.csv` from the web
- Uses pandas to store the schedule
- Returns:
  - `answer_text`
  - `evidence_rows` (subset of DataFrame)

### Supported Query Types:
- Total enrollment (500/600 level classes)
- Classes taught by an instructor
- Build non-overlapping schedule of 3 classes
- Build schedule from list of specific course codes
- Open seats / lab counts

This tool is deterministic and does NOT use an LLM.

---

## Set OPENAI API Key

```powershell
setx OPENAI_API_KEY "your_actual_key_here"
```

Then:
1. Close PowerShell
2. Reopen PowerShell
3. Restart Jupyter

To verify:

```powershell
echo $env:OPENAI_API_KEY
```

---

# How to Add Queries

Inside the notebook, locate the section:

```python
queries = [
    "What is the total enrollment in all 500 and 600-level CS classes?"
]
```

To add more:

```python
queries = [
    "What is the total enrollment in all 500 and 600-level CS classes?",
    "What CS classes does Q Chen teach?",
    "Create a schedule of three 600-level classes.",
    "Create a schedule to take CS 250, MATH 245, PHYS 196."
]
```

Run the cell to execute all queries.

---

# Relevancy Strategy + NOTES
Schedule related questions are answered using `sciences.csv`.
Evidence rows are returned for verifying info.
I decided to use pandas + regex to parse the csv file into useful columns to better extract data.

## Conceptual Questions
If no structured rule matches:
- FAISS retrieves top-k relevant PDF chunks
- Context is passed to the LLM
- LLM generates answer constrained to retrieved content