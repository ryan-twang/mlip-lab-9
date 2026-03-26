# Lab 9 — TA check-in script (commands + what to show)

Use this during your live demo. Run commands from the repo root with your venv activated:

```bash
cd /path/to/mlip-lab-9
source venv/bin/activate
```

---

## 1. Opening (15–30 seconds)

**Say:** “I ran the Breast Cancer Wisconsin pipeline twice: with **DVC** (declarative `dvc.yaml`) and **Roar** (implicit lineage from `roar run`). I versioned data with DVC, ran experiments with both tools, registered my model on **GLaaS**, and filled in the README reflection table.”

---

## 2. Deliverable 1 — DAGs + GLaaS

### Commands (terminal)

```bash
dvc dag
```

**Show:** ASCII graph. **Say:** “Stages come from **`dvc.yaml`**: preprocess → train → evaluate; deps and outputs are explicit.”

```bash
roar dag
```

**Show:** Current inferred graph. **Say:** “Roar inferred this from **file reads/writes** during `roar run` — no pipeline YAML.”

Optional context:

```bash
cat dvc.yaml
```

**Say:** “Here’s the declarative definition DVC uses.”

### Browser (GLaaS)

**Show:** Logged into [https://glaas.ai](https://glaas.ai):

1. **DAG / session** page — pipeline graph tied to your repo/commit.
2. **Artifact** page for `classifier.pkl` — BLAKE3 hash, “Outputted from jobs” (`train.py`), **Reproduce with roar** if shown.

**Say:** “I ran `roar auth register`, added my public key on GLaaS, `roar auth test`, then `roar register models/classifier.pkl`. This is the registered lineage the lab asks for.”

---

## 3. Deliverable 2 — Experiments (both tools)

### DVC

```bash
dvc exp show
```

**Show:** Table of experiments with params and metrics. **Say:** “I ran multiple **`dvc exp run -S train.n_estimators=... -S train.max_depth=...`** runs and compared them here. **`dvc exp apply <name>`** promoted the best run.”

Traceability:

```bash
grep -A 20 '^  train:' dvc.lock
```

**Say:** “**`dvc.lock`** pins param hashes and artifact MD5s for reproducibility.”

### Roar

```bash
roar show @latest
```

(Or `roar show @1`, `@2`, … if the TA asks for a specific job.)

**Show:** Command, git commit, input/output paths and hashes. **Say:** “Each **`roar run`** is a job; I compared runs by inspecting **`roar show`** and **`metrics/scores.json`** with different **`params.yaml`** settings.”

```bash
cat metrics/scores.json
```

**Say:** “Metrics for the latest evaluation.”

---

## 4. Deliverable 3 — Reflection (README)

**Show:** `README.md` on your machine or GitHub — **Summary Table** and **Reflection Answers** (Q1–Q6).

**Say:** “I compared declarative vs observational tracking: config, DAG definition, caching vs manual re-run, versioning, collaboration, and platform support — details are in the README.”

---

## 5. Answers to reflection questions (short scripts)

Use your README bullets; expand only if the TA asks.

| Question | What to say (essence) |
|----------|------------------------|
| **Q1** | DVC needed **`dvc.yaml`**, remote, **`dvc add`**; Roar needed only **`roar init`**. DVC adds **caching**, **`dvc exp`**, and a clear pipeline contract. |
| **Q2** | Both show **preprocess → train → (evaluate in DVC)**. DVC adds **data `.dvc`** node; Roar may show **exact commands** and extra steps (e.g. **augment**) if you ran them under Roar. |
| **Q3** | **`dvc repro`** re-runs only stages whose **hashes** changed. Roar **always runs** what you execute; **you** choose the steps. Data change: **`dvc add`** vs Roar **observing writes**. |
| **Q4** | **`dvc exp show`** compares experiments in one table. Roar: **per-job** inspection via **`roar show`**. Trace: **`dvc.lock`** + Git vs **job record + hashes** on GLaaS/local DB. |
| **Q5** | **`dvc.yaml`** in Git documents the pipeline for teammates; **GLaaS** gives **global artifact lookup**. Using **both**: DVC for definition/repro, Roar for **runtime lineage** / registration. |
| **Q6** | For a course project: **DVC** for Git + experiments + cross-platform; **Roar** for exploration and **auditing**; **combined** when useful. |

---

## 6. Optional: DVC data versioning (if INSTRUCTIONS Part 1 done)

```bash
cat data/raw/data.csv.dvc
```

**Say:** “Raw data is tracked by DVC; the `.dvc` file stores the hash, not the blob.”

```bash
dvc remote list
```

**Say:** “Remote is configured for `dvc push` / `dvc pull`.”

---

## 7. Pushing your work (your own GitHub)

`origin` may point at the **course upstream**. To save **your** commits:

1. Create a **fork** on GitHub (your account).
2. Add it and push:

```bash
git remote add myfork https://github.com/YOUR_USERNAME/mlip-lab-9.git
git push -u myfork main
```

Replace `YOUR_USERNAME` with your GitHub username. If you already cloned **your** fork, `git push origin main` is enough.

---

## Quick command list (copy-paste block)

```bash
source venv/bin/activate
dvc dag
roar dag
dvc exp show
roar show @latest
cat metrics/scores.json
```

Then show **glaas.ai** (DAG + artifact) and **README.md** reflection section.
