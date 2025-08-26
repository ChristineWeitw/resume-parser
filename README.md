# Resume Parser — Gemini‑first with graceful fallback

This project parses resumes (PDF/DOCX/TXT) into:
```json
{ "name": "...", "email": "...", "skills": ["..."] }
```

## Why this design?
- **LLM‑first** (Gemini-1.5-flash) for higher accuracy in varied formats
- **Reliable fallback** (regex + heuristics) if no API key or offline
- **Modular code** that’s easy to extend and explain

## Quickstart
Create virtual env with python>=3.11
   ```bash
   python3.11 -m venv .venv
   source .venv/bin/activate
   ```
1. Set your API key in .env file (you may refer to .env.example) or export api key as environment variable in terminal.
   ```bash
   export GOOGLE_API_KEY="your_key"
   ```
2. Run `requirements.txt` to install packages
   ```bash
   pip install -r requirements.txt
   python -m spacy download en_core_web_sm
   ```
3. Open the notebook `resume_parser_v2_eval.ipynb` and follow cells top to bottom.
4. To extend skills, add `skills.csv`.
5. TO test example pdf or word file, add `sample_resume.pdf` or `sample_resume.docx` 

## Files
- `resume_parser_v2_eval.ipynb` — main notebook
- `requirements.txt` - Python packages and their exact versions within the current environment
- `output` - folder that includes outputs results from the parse_resume pipeline
- `answer.jsonl` - file explicits the correct example resumes' results for evaluation purpose 
- `skills.csv` — 
Here we use the esco_skills.csv dataset from Kaggle to ensure the fallback works out of the box.(https://www.kaggle.com/datasets/thenoob69/esco-skills) 
Optionally, you can extend it by pointing to your own CSV or TXT file.
- `sample_resume.pdf` & `sample_resume.docx` - sample PDF and WORD file to test out. 

## Mini-Evaluation (field-level + relaxed skills)

This section evaluates the parser on a small labeled set. It measures:

Name accuracy (case-insensitive exact match)

Email accuracy (case-insensitive exact match)

Skills (strict): phrase-level precision/recall/F1 (exact normalized phrase overlap)

Skills (relaxed): a gold skill counts as covered if ≥50% of its meaningful tokens appear anywhere in predicted skills (word-by-word). Also reports token-level recall.

1) Prepare ground truth (answer.jsonl)

Create a file at the repo root named answer.jsonl. One JSON per line, with file, name, email, skills.
Angle brackets <...> are allowed; the evaluator strips them automatically.
   ```
   {"file":"sample_resume1.pdf","name":"<Kristen Connely>","email":"<email@email.com>","skills":["Call Sheets & Sides","Adobe Premiere Pro","Camera Boom","Light Boom","Mic Boom","DaVinci Resolve"]}
   ...
   {"file":"sample_resume7.docx","name":"<JACKY WILSON>","email":"<example@resumeviking.com>","skills":["tools manufacturing","CNC machining","OSHA trained","Lathes Machines"]}
   ```


2) Generate predictions

Run the notebook to produce outputs/*.json. Each prediction JSON must include:
   ```
   {
   "file": "",
   "name": "",
   "email": "jane@example.com",
   "skills": [],

   }
   ```
3) Run evaluation (drop-in notebook cell below)

   Case-insensitive comparisons for name/email

   Phrase-level strict metrics and relaxed, word-by-word coverage for skills

   Adjustable threshold RELAXED_OVERLAP (default 0.50)
