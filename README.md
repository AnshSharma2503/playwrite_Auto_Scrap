# Playwrite_Auto_Scrap — README (branch: new)

**Drop-in README for the `new` branch** — clear setup, run (with frontend/app.py and headless/main), configuration, troubleshooting, and deployment.

---

## Project summary

This repository automates web scraping / automation using Playwright. The `new` branch contains a Flask-based app frontend plus a `src/` backend for headless automation. The README below explains how to run the project **with the frontend (app.py)** and **without frontend (run the main script in `src/`)**.


## Repo structure (top-level)

- `app.py` — Flask app / frontend (starts the web interface).
- `requirements.txt` — Python dependencies.
- `input.csv` — sample input / targets for scraping.
- `templates/` — HTML templates used by `app.py`.
- `src/` — backend scripts (automation logic, main runner).
- `debug/`, `logs/` — folders for runtime artifacts and logs.
- `.gitignore`


## Prerequisites

- Python 3.8+ (3.10 recommended)
- pip
- Virtual environment (venv)
- Chrome/Chromium (or use Playwright to install browsers)
- If using Playwright, install browsers with `playwright install`


## Setup (one-time)

1. Clone the repo and switch to the `new` branch:

```bash
git clone https://github.com/AnshSharma2503/playwrite_Auto_Scrap.git
cd playwrite_Auto_Scrap
git checkout new
```

2. Create and activate a virtual environment:

```bash
python -m venv venv
# Linux / macOS
source venv/bin/activate
# Windows (PowerShell)
venv\Scripts\Activate.ps1
```

3. Install Python dependencies:

```bash
pip install -r requirements.txt
```

4. (If Playwright is used) Install browsers:

```bash
playwright install
```


## Configuration

- Edit `input.csv` to include target URLs or input arguments for scraping.
- If the project reads config from environment variables, create a `.env` (not present by default) and add keys like `FLASK_ENV`, `PORT`, or provider API keys.
- Logs and debug artifacts are stored in `logs/` and `debug/` respectively.


## Run options

### 1) Run with frontend (web UI) — `app.py`

This starts a Flask web interface where you can upload `input.csv`, trigger scraping runs, and view results.

```bash
# activate venv first
python app.py
```

Default behaviors:
- The app serves templates from `templates/`.
- Web UI endpoints let you upload inputs and start a run.
- Output (scraped data) and temporary files are saved to `debug/` or `logs/`.

**Notes**:
- If `app.py` expects a specific port, pass `--port 5000` or set `PORT` env var.
- Open the UI at `http://127.0.0.1:5000/` (or the configured host/port).


### 2) Run headless / without frontend — `src/` main

To run the core automation without the web UI, run the main script inside `src/`. The exact main filename may be `main.py`, `run.py`, or another entry — run the file with a `if __name__ == "__main__"` guard. Example:

```bash
# Example — replace with actual main filename inside src/
python src/main.py --input input.csv
```

If you are unsure which file to run, open `src/` and look for a file that:
- contains `if __name__ == "__main__":` or
- imports `playwright` or an `asyncio` runner and exposes a `run()` function.


## Typical workflows

1. **Quick test (headless)**
   - Update a small `input.csv` with 1–2 test URLs.
   - Run the headless main script and inspect `debug/` for outputs.

2. **Full run (frontend)**
   - Start `app.py`.
   - Open the UI, upload `input.csv`, and click the run button.
   - Monitor progress from the web UI and check `logs/` for detailed traces.


## Logging & outputs

- `logs/` — execution logs, errors, and runtime traces.
- `debug/` — screenshots, html dumps, and per-run artifacts.
- For reproducibility, keep a copy of the `input.csv` used for each run.


## Troubleshooting

- **Playwright errors**: run `playwright install` and ensure the browser executables are present.
- **Permission issues on Linux serial / files**: use correct file permissions or run with sudo carefully.
- **Flask app not starting**: check if the port is already used; change with `--port`.
- **Missing dependencies**: re-run `pip install -r requirements.txt`.


## Deployment tips

- Use Gunicorn + systemd if exposing the web UI in production:

```
# example systemd ExecStart
ExecStart=/path/to/venv/bin/gunicorn -w 4 -b 127.0.0.1:5000 app:app
```

- For headless runs, schedule scraping using cron or systemd timers, saving outputs to a dated folder.


## Security & privacy

- Do not commit credentials or API keys.
- Respect robots.txt and site terms before scraping.
- Throttle requests and use randomized delays to avoid rate-limiting.


## Next steps / Improvements

- Add a `config.example.env` with expected env vars.
- Add a `README` inside `src/` enumerating each script and CLI options.
- Add a `requirements-dev.txt` for development tools and linters.


---

If you want, I can also:
- create a ready-to-download `README.md` file and `config.example.env` in the repo, or
- inspect `src/` to find the exact main entrypoint and replace the placeholder `src/main.py` with the real filename.

*Generated README — feel free to copy into your repo.*
