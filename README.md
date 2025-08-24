# Resume Parser — Gemini‑first with graceful fallback

This project parses resumes (PDF/DOCX/TXT) into:
```json
{ "name": "...", "email": "...", "skills": ["..."] }
```

## Why this design?
- **LLM‑first** (Gemini-1.25-flash) for higher accuracy in varied formats
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
3. Open the notebook `resume_parser_v1.ipynb` and follow cells top to bottom.
4. To extend skills, add `skills.csv`.
5. TO test example pdf or word file, add `sample_resume.pdf` or `sample_resume.docx` 

## Files
- `resume_parser_v1.ipynb` — main notebook
- `requirements.txt` - Python packages and their exact versions within the current environment 
- `skills.csv` — 
Here we use the esco_skills.csv dataset from Kaggle to ensure the fallback works out of the box.(https://www.kaggle.com/datasets/thenoob69/esco-skills) 
Optionally, you can extend it by pointing to your own CSV or TXT file.
- `sample_resume.pdf` & `sample_resume.docx` - sample PDF and WORD file to test out. 


