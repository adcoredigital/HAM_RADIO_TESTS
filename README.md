# HAM RADIO TESTS

**Amateur Radio Examination Practice & Mock Tests**

A complete, static (no backend, no database, no login) practice test website for
HAM Radio / Amateur Radio examination preparation. Built with plain HTML5, CSS3
and vanilla JavaScript, reading questions from CSV files. Designed to be hosted
for free on **GitHub Pages**.

- 24 Practice Tests × 25 questions = 600 practice questions
- 16 Mock Tests × 50 questions = 800 mock questions
- 1,400 total questions
- One question shown at a time, instant feedback, explanations, score summary, full review

---

## 1. Folder Structure

```
ham-radio-tests/
├── index.html              Homepage
├── practice.html           Practice Tests listing (24 tests)
├── mock-tests.html         Mock Tests listing (16 tests)
├── test.html                The test-taking engine (used for both practice & mock)
├── instructions.html       How to use the site
├── about.html               About page
├── README.md
├── css/
│   └── style.css            All site styling
├── js/
│   ├── config.js             ⭐ Central list of every test (names, files, question counts)
│   ├── app.js                 Shared nav, homepage stats, listing page rendering
│   ├── csv-loader.js         CSV parser + fetch helper
│   └── test-engine.js        Test-taking logic (state, scoring, review)
└── data/
    ├── practice/
    │   ├── practice-01.csv   ← contains 3 DEMO questions (see section 9)
    │   ├── practice-02.csv   ← empty placeholder (header row only)
    │   └── ... practice-24.csv
    └── mock/
        ├── mock-01.csv        ← contains 3 DEMO questions (see section 9)
        ├── mock-02.csv        ← empty placeholder (header row only)
        └── ... mock-16.csv
```

---

## 2. Where To Place Practice CSV Files

Put your 24 real practice CSV files inside `data/practice/`, using **exactly**
these file names (already referenced in `js/config.js`):

```
data/practice/practice-01.csv   ...   data/practice/practice-24.csv
```

Simply **overwrite** the existing files with your real question data — no
JavaScript changes are needed as long as the file names stay the same.

## 3. Where To Place Mock CSV Files

Put your 16 real mock CSV files inside `data/mock/`, using exactly these names:

```
data/mock/mock-01.csv   ...   data/mock/mock-16.csv
```

Overwrite the existing placeholder files the same way.

---

## 4. CSV Column Requirements

Every CSV file (practice or mock) **must** use this exact header row, in any
column order you like — the loader matches columns by name, not position:

```
Question Number,Question,Option A,Option B,Option C,Option D,Correct Option,Correct Answer,Explanation
```

| Column           | Notes                                                            |
|------------------|-------------------------------------------------------------------|
| Question Number  | Informational; not required to be sequential                     |
| Question         | The question text                                                 |
| Option A–D       | The four answer choices                                           |
| Correct Option   | Must be exactly one letter: `A`, `B`, `C`, or `D`                  |
| Correct Answer   | The full text of the correct answer (shown to the user)           |
| Explanation      | Shown after the user answers — comes directly from your CSV       |

**Tips for CSV editing:**
- If a field contains a comma, wrap the whole field in double quotes: `"Yes, this works"`
- If a field contains a double quote character, double it: `"He said ""hello"""`
- Save the file with UTF-8 encoding.
- Use Excel, Google Sheets ("Download as CSV"), or any plain text editor.
- A row missing a required value is **skipped automatically** with a console warning — it will not crash the test.

---

## 5. How To Test Locally

Opening `index.html` directly by double-clicking it (a `file://` URL) may
**not** load the CSV files correctly, because browsers restrict `fetch()`
requests made from `file://` pages. Always test using a local web server:

**Option A — VS Code Live Server extension**
1. Install the "Live Server" extension in VS Code.
2. Right-click `index.html` → "Open with Live Server".
3. Your browser opens at `http://127.0.0.1:5500/` (or similar) — CSV loading will work correctly.

**Option B — Python's built-in server**
```bash
cd ham-radio-tests
python3 -m http.server 8000
```
Then open `http://localhost:8000/` in your browser.

**Option C — Node's `http-server` (if you have Node.js installed)**
```bash
npx http-server .
```

---

## 6. How To Create a GitHub Repository

1. Go to [github.com](https://github.com) and sign in (or create a free account).
2. Click the **+** icon (top right) → **New repository**.
3. Repository name: `ham-radio-tests` (or any name you like).
4. Set it to **Public** (required for free GitHub Pages).
5. Click **Create repository**.

---

## 7. How To Upload Files To GitHub

**Option A — Web upload (easiest for beginners)**
1. Open your new repository page on GitHub.
2. Click **Add file → Upload files**.
3. Drag the entire contents of the `ham-radio-tests` folder into the browser window (make sure the folder *structure* — `css/`, `js/`, `data/` — is preserved; most browsers support dragging whole folders).
4. Scroll down and click **Commit changes**.

**Option B — Git command line**
```bash
cd ham-radio-tests
git init
git add .
git commit -m "Initial commit: HAM RADIO TESTS website"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ham-radio-tests.git
git push -u origin main
```

---

## 8. How To Enable GitHub Pages

1. On your repository page, click **Settings**.
2. In the left sidebar, click **Pages**.
3. Under **Build and deployment → Source**, select **Deploy from a branch**.
4. Under **Branch**, select `main` and folder `/ (root)`.
5. Click **Save**.
6. Wait 1–2 minutes. Your site will become available at:

```
https://YOUR-USERNAME.github.io/ham-radio-tests/
```

(The exact URL depends on your GitHub username and the repository name you chose.)

---

## 9. Demo Data — Read Before You Deploy

Because your real question banks will be uploaded separately, this project
ships with:

- `data/practice/practice-01.csv` — **3 sample DEMO questions**, clearly labelled `[DEMO DATA]`, so you can confirm the full practice-test flow works (loading, answering, feedback, scoring, review).
- `data/mock/mock-01.csv` — **3 sample DEMO questions**, similarly labelled, so you can confirm the mock-test flow works.
- All other 38 CSV files (`practice-02` … `practice-24`, `mock-02` … `mock-16`) contain **only the header row** (no questions yet). Opening one of these tests will show a friendly **"No questions were found for this test"** message until you replace it with real data — this is expected and is part of the site's built-in empty-state handling.

**To go live with your real content:** replace each placeholder CSV with your
prepared question file, keeping the exact same file name. No code changes
are required — the app reads whatever is in the CSV automatically.

---

## 10. How To Update CSV Files Later

Just replace the file in `data/practice/` or `data/mock/` with the same file
name and re-upload it to GitHub (or `git push` the change). The website will
automatically reflect the new questions — nothing else needs to change.

---

## 11. How To Change Test Names / Titles / Topics

Open `js/config.js`. Each test is one object in the `practiceTests` or
`mockTests` array, for example:

```js
{ id: "practice-01", number: 1, title: "Part 1A – Basic Electronics",
  topic: "Basic Electronics", group: "PART 1 — BASIC ELECTRONICS",
  questions: 25, file: "./data/practice/practice-01.csv" }
```

Edit the `title`, `topic`, or `group` text as needed. Do **not** change `id`
unless you also rename the matching CSV file and update `file` to match.

---

## 12. How To Change The Pass Percentage

Open `js/test-engine.js` and find this line near the top:

```js
const RANDOMIZE_PRACTICE_QUESTIONS = false;
const RANDOMIZE_MOCK_QUESTIONS = false;
```

and in `js/config.js`:

```js
const PASS_PERCENTAGE = 60;
const SHOW_PASS_FAIL = false;
```

- Change `PASS_PERCENTAGE` to any number (e.g. `75`).
- Set `SHOW_PASS_FAIL = true` to make the result page display a **PASSED** or
  **NEEDS MORE PRACTICE** badge based on that percentage. It stays hidden
  by default until you turn this on.

---

## 13. How To Change Colors

Open `css/style.css` and edit the CSS variables at the very top of the file,
inside `:root { ... }`:

```css
--navy-950: #060c1a;   /* darkest background */
--cyan-400: #35e0e0;   /* primary accent color */
--orange:   #f5943c;   /* alert / mock-test accent */
```

Changing these values updates the color scheme across the entire site.

---

## 14. How To Add Another Test

1. Add your new CSV file to `data/practice/` or `data/mock/`.
2. Open `js/config.js` and add a new object to the `practiceTests` or
   `mockTests` array, following the same pattern as the existing entries
   (give it a unique `id`, a `title`, and the correct `file` path).
3. Save — the new test card will automatically appear on the Practice Tests
   or Mock Tests page, grouped correctly if you set a matching `group`.

No changes to any HTML or other JavaScript file are required.

---

## 15. Future Expansion Ideas (Already Designed For)

The code is intentionally modular so these can be added later without a
rewrite: randomized question order (`RANDOMIZE_PRACTICE_QUESTIONS` /
`RANDOMIZE_MOCK_QUESTIONS` in `test-engine.js`), randomized option order,
timed mock tests, negative marking, score history dashboards, dark/light
theme toggle, search, category filtering, and question bookmarks.

---

## 16. Common Errors & Solutions

| Problem | Likely Cause | Solution |
|---|---|---|
| "Loading Test..." never finishes / CSV doesn't load | Testing via `file://` instead of a local server | Use VS Code Live Server or `python3 -m http.server` (see section 5) |
| "Unable to load this test. Please try again." | CSV file missing, misnamed, or a network/path issue | Check the browser console for the exact error; confirm the file exists at the path listed in `js/config.js` |
| "No questions were found for this test." | CSV only has a header row (placeholder) or all rows failed validation | Add real question rows, or check the console for row-level warnings |
| Explanations look cut off or garbled | A field with a comma wasn't wrapped in quotes | Wrap any field containing a comma in double quotes: `"like, this"` |
| Site works locally but not on GitHub Pages | Repository is private, or Pages folder path was expecting a different branch | Confirm the repo is Public and Pages is set to branch `main`, folder `/ (root)` |
| Layout looks broken on GitHub Pages only | An absolute path (starting with `/`) was introduced somewhere | All paths in this project are relative (`./css/...`, `./data/...`) — keep any new files/links relative too |

---

## 17. Accessibility & Technical Notes

- Semantic HTML, visible focus states, ARIA labels on the progress bar and live regions for feedback.
- Correct/incorrect states are shown with icons and text, not color alone.
- Fully responsive from 375px (mobile) up to large desktop screens.
- No external JavaScript dependencies — the CSV parser is hand-written and dependency-free, so the site has zero third-party network requests and works entirely offline once loaded.
- `localStorage` is used only for optional convenience (last test opened, saved scores) and the site works correctly even if storage is disabled or unavailable.

---

© 2026 HAM RADIO TESTS. All Rights Reserved.
