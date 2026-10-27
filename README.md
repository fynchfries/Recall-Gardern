# Recall Garden

Recall Garden is a private, local-first flashcard app for learning things you want to remember. Make decks, add question-and-answer cards, and review them with a spaced-repetition schedule that brings difficult material back sooner. It runs in a browser but stores your study data in a local SQLite database—no account, cloud service, or third-party Python packages required.

## Run it

Requires Python 3.10 or newer.

```bash
python3 app.py
```

The app opens at **http://127.0.0.1:8000**. If it doesn't open automatically, visit that address in your browser. Stop the server with `Ctrl+C`.

The database is created as `recall.db` beside `app.py`. To use another location, set `RECALL_DB=/path/to/recall.db`. To change the port, set `PORT=8080` (for example, `PORT=8080 python3 app.py`). By default the server binds only to `127.0.0.1` for local use.

### Run in GitHub Codespaces

In the Codespaces terminal, run:

```bash
HOST=0.0.0.0 python3 app.py
```

Then open the **Ports** tab in VS Code, find port **8000**, and choose **Open in Browser**. Keep the forwarded port private; the app does not have user accounts or authentication. If port 8000 is already in use, run `HOST=0.0.0.0 PORT=8001 python3 app.py` and open port 8001 instead.

## What you can do

- Create and organize decks of flashcards.
- Add, edit, delete, search, and filter cards.
- Study all due cards or focus on a single deck.
- Rate each answer **Again**, **Hard**, **Good**, or **Easy** to schedule its next review.
- See cards due, total cards, and reviews completed today.
- Export and import a JSON backup. Importing merges cards into matching decks and adds cards; it doesn't delete existing data.
- Use the responsive interface on desktop or mobile.

## Spaced repetition

New cards are immediately due. A correct answer advances the card's interval: the first **Good** review is due in one day, the second in three days, then the interval grows with the card's ease factor. **Easy** advances faster and raises ease; **Hard** advances more slowly and lowers ease. **Again** resets the streak and schedules another try in ten minutes. The ease factor never drops below 1.3.

This is a deliberately understandable, lightweight scheduler rather than a claim to implement a particular formal memory model. The card's review history and current schedule are stored in SQLite.

## Project structure

- `app.py` — local HTTP server, JSON API, SQLite data layer, and scheduling logic.
- `static/index.html` — app shell and accessible page structure.
- `static/styles.css` — responsive visual design.
- `static/app.js` — browser interface and interactions.
- `tests/test_app.py` — tests for persistence, validation, scheduling, and backup behavior.

The data layer (`StudyStore`) is separate from the HTTP handler, which makes the core functionality testable without running a browser or server.

## Tests

```bash
python3 -m unittest discover -s tests -v
```

## Privacy

The server only listens on your own computer. Study content is stored in `recall.db`, and backup files are created only when you request them. The app has no analytics, tracking, sign-in, or external service dependencies. Keep your database and backups private if the material is sensitive.
