# Working rules for this repository

1. **Small, tested slices.** Follow README.md → Roadmap. Each slice ships as one
   reviewed commit with tests; CI must stay green (ruff, mypy, bandit,
   pip-audit, pytest at 100% coverage, gitleaks).
2. **No fake data, no invented numbers.** Results tables only contain figures
   produced by `python -m eval.run` on the real labelled set.
3. **Single human author.** Never add "Generated with Claude", "Co-Authored-By",
   bot authors, signature emojis, or any AI mention in commits, code or docs.
   Verify with `git log -i --grep=claude --grep=co-authored` returning nothing.
4. **Secrets stay in `.env`.** Run `bandit -r docintel` and
   `pip-audit -r requirements.txt` locally before every push.
5. **Windows notes.** Call `.venv/Scripts/python.exe -m pip` (pip.exe is
   blocked); if mypy fails to load, use
   `../study-rag-tutor/backend/.venv/Scripts/mypy --python-executable <abs>/.venv/Scripts/python.exe`.
