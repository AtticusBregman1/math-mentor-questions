# MathMentor Question Bank

This folder is a **separate Git repository** that holds the question bank, import pipeline, and authoring guidelines for MathMentor. It is ignored by the main MathMentor application repository.

## Architecture Decisions

### Content Format

- **LaTeX for math** — All mathematical notation is written in LaTeX for portability, precise rendering (via KaTeX/MathJax), and ease of translation. Use `$...$` for inline math and `$$...$$` for block math.

- **Markdown for structure** — Problem setup, hints, and explanatory text use Markdown for headings, lists, emphasis, and readability. Combines cleanly with LaTeX in the same file.

- **Tag-based question bank** — Questions live in a flat bank with metadata tags rather than a rigid hierarchy. Tags enable flexible filtering (e.g. `algebra`, `linear-equations`, `word-problem`, `bloom-apply`) and allow questions to belong to multiple concepts without duplication.

### Pipeline Flow

```
Word doc (legacy) → [Proprietary converter] → Repo files (Markdown + LaTeX)
                                                      ↓
                                        [Import script / CI job]
                                                      ↓
                                            Firestore (MathMentor backend)
```

- **Source of truth:** This repository
- **Runtime store:** Firebase Firestore (MathMentor backend)
- **Import:** Script reads repo files and writes/updates Firestore on merge or manual run

## Contents

- **`questions/`** — Question files (Markdown + LaTeX), organized by topic or tag
- **`pipeline/`** — Conversion and import scripts (Word → repo format, repo → Firestore)
- **`guidelines/`** — Question authoring guidelines, tag taxonomy, and format specs (see `QUESTION-FORMAT.md`)

## Getting Started

```bash
cd questions
git init
git add .
git commit -m "Initial question bank setup"
git remote add origin https://github.com/YOUR_USERNAME/math-mentor-questions.git
git push -u origin main
```

## Importing into Firestore

From the **MathMentor** project root (not this folder):

```bash
npm run import-questions
```

Requires `.env`:

- `GITHUB_TOKEN` — Personal access token with `repo` scope (required for private repos)
- `GITHUB_QUESTIONS_REPO` — `owner/repo` (e.g. `your-username/math-mentor-questions`)
- `GITHUB_QUESTIONS_PATH` — Optional, default `questions`
- `IMPORT_CLEAR=1` — Optional, clears existing questions before import

The script fetches `.md` files from the repo, parses frontmatter and content, and upserts into Firestore by `sourceId` (filename without `.md`).

## Relationship to MathMentor

The MathMentor app reads questions from Firestore. This repo is the authoring layer—questions are edited here, version-controlled, and imported into Firestore for the app to serve. The main MathMentor codebase does not track this folder.
