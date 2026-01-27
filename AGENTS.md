# Repository Guidelines

## Project Structure & Module Organization
- `daily_arxiv/`: Scrapy project and spider (`daily_arxiv/spiders/arxiv.py`) plus dedup logic (`daily_arxiv/check_stats.py`).
- `ai/`: LLM enhancement pipeline (`enhance.py`, `structure.py`).
- `to_md/`: Convert JSONL into Markdown pages (`convert.py`, `paper_template.md`).
- `js/`, `css/`, `index.html`, `login.html`, `settings.html`, `statistic.html`: Static site UI and client logic.
- `assets/`: Generated file list and UI assets; `assets/file-list.txt` is updated by workflows.
- `data/`: Generated JSONL/Markdown outputs (created by workflows/scripts; not always present locally).
- `template.md` and `readme_content_template.md`: README generation templates; `update_readme.py` assembles README.

## Build, Test, and Development Commands
- `uv sync`: Install Python deps into `.venv` (used in CI).
- `source .venv/bin/activate`: Activate the environment before running scripts.
- `./run.sh`: End-to-end local workflow (crawl → dedup → AI → Markdown → file list). Requires `OPENAI_API_KEY` for full mode.
- `scrapy crawl arxiv -o ../data/DATE.jsonl`: Run the crawler manually from `daily_arxiv/`.
- `python ai/enhance.py --data data/DATE.jsonl`: AI summarization step.
- `python to_md/convert.py --data data/DATE_AI_enhanced_LANG.jsonl`: Markdown conversion.
- `python update_readme.py`: Rebuild README from templates.

## Coding Style & Naming Conventions
- Python: 4-space indentation, prefer snake_case for files/functions, keep modules focused by stage (crawl/enhance/convert).
- JS/CSS/HTML: follow existing style in `js/` and `css/`; keep config placeholders intact.
- No enforced formatter/linter; keep changes minimal and consistent with surrounding code.

## Testing Guidelines
- No dedicated test framework in this repo. Validate by running `./run.sh` or the specific stage you touched.
- Ensure output files are written to `data/` and `assets/file-list.txt` updates accordingly.

## Commit & Pull Request Guidelines
- Recent history mixes conventional prefixes (`feat:`, `fix:`, `chore:`) and plain messages; prefer Conventional Commits when adding code changes.
- Data branch commits follow `update: YYYY-MM-DD arXiv papers` in CI; avoid manual edits to the `data` branch.
- PRs should describe the change, note any workflow impacts, and include screenshots for UI changes.

## Security & Configuration
- Required secrets/vars for automation: `OPENAI_API_KEY`, `OPENAI_BASE_URL`, `LANGUAGE`, `CATEGORIES`, `MODEL_NAME`, `EMAIL`, `NAME`.
- Optional access control: `ACCESS_PASSWORD` (GitHub Actions) or `.env` + `setup-local-auth.sh` for local password hashing.
