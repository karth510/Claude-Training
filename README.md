# IT PMO Kanban Board

A demo IT PMO Kanban board for a **fictitious** bank: a single-file, dependency-free web app for tracking IT project tasks.

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/docs/Web/HTML)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)](https://developer.mozilla.org/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/docs/Web/JavaScript)
[![No dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)](#tech-stack)
[![GitHub Pages](https://img.shields.io/badge/hosted%20on-GitHub%20Pages-222?logo=github)](https://karth510.github.io/Claude-Training/)
[![Deploy to GitHub Pages](https://github.com/karth510/Claude-Training/actions/workflows/pages.yml/badge.svg)](https://github.com/karth510/Claude-Training/actions/workflows/pages.yml)
[![Live demo](https://img.shields.io/badge/demo-live-blue)](https://karth510.github.io/Claude-Training/)

## Live demo

**<https://karth510.github.io/Claude-Training/>**

## Features

- **Four columns:** Backlog, In Progress, Blocked and Done. Each has a short description and a live count badge.
- **Drag and drop** cards between columns using the native HTML5 API. Each card also has a keyboard-friendly "Move" menu.
- **Add tasks** with title, description, project, category, assignee, priority, due date and status. Validation errors show inline, next to the field.
- **Delete** with an inline "Delete? Yes / No" confirmation. There are no browser pop-ups.
- **Filters** by project, assignee (free-text) and priority, plus a reset button. Filters only hide cards. Counts always cover every task.
- **Summary strip** in the header with live totals per status.
- **Overdue and upcoming due dates** are highlighted. Seed data is dated relative to today, so the demo always has both.
- **Email notification** for each new task via [FormSubmit](https://formsubmit.co/) (subject `[IT PMO] …`). The card is added immediately. If the email fails, you only get a warning toast and the card stays.
- **No persistence on purpose:** data lives in memory, so a refresh resets the board to the seed data.
- Task IDs look like `ITPM-0001`. Branding is neutral throughout.

## Tech stack

- Vanilla HTML, CSS and JavaScript, all in one `index.html`.
- No frameworks, build step, npm, CDN scripts, web fonts or images. It uses the system font stack and inline SVG/Unicode icons.
- Colours and spacing come from CSS custom properties on `:root`.

## How to install

```bash
git clone https://github.com/karth510/Claude-Training.git
cd Claude-Training
```

There is nothing else to install.

## How to run

- **Quickest:** double-click `index.html`. It works from `file://`.
- **With working email notifications:** FormSubmit may reject `file://` origins, so serve the folder locally:

  ```bash
  python3 -m http.server 8000
  # then open http://localhost:8000
  ```

### Configure the notification email

1. In `index.html`, set the address in `FORMSUBMIT_ENDPOINT` (near the top of the `<script>` block):

   ```js
   const FORMSUBMIT_ENDPOINT = "https://formsubmit.co/ajax/you@example.com";
   ```

2. Add a task once. FormSubmit sends a one-time **activation email** to that address. Click the link in it, and notifications are delivered from then on.

> The address in `FORMSUBMIT_ENDPOINT` is visible to anyone viewing the published page. The repo ships with the placeholder `YOUR_EMAIL@example.com`.

## CI/CD

[`.github/workflows/pages.yml`](.github/workflows/pages.yml) runs on every push to `main`, on pull requests and on demand:

- **`check` job** (pushes and PRs):
  - Fails on forbidden APIs: `localStorage`, `sessionStorage`, IndexedDB, cookies, `alert`/`confirm`, `!important`, or any URL other than the FormSubmit endpoint.
  - Fails on extra network calls: anything besides the single FormSubmit `fetch`, such as external scripts, stylesheets or images.
  - Syntax-checks the inline script with `node --check`.
  - Greps tracked files for common secret patterns.
- **`deploy` job** (push to `main` only, after `check` passes): stages **only** `index.html` and deploys it to GitHub Pages. Repo docs such as `CLAUDE.md` are never published.

## Project structure

```
.
├── index.html                    # The whole app: markup, <style> and <script>
├── README.md
├── CLAUDE.md                     # Constraints and architecture notes for contributors / AI assistants
├── .gitignore
└── .github/workflows/pages.yml   # CI checks + GitHub Pages deploy
```

## Privacy and security

- No `localStorage`, `sessionStorage`, IndexedDB or cookies. Nothing is stored in the browser.
- No analytics, tracking, external scripts or fonts.
- The **only** outbound request is the FormSubmit AJAX call made when you add a task. It sends that task's details to the configured address.
- Every user-supplied string is HTML-escaped before rendering.

## Disclaimer

This is a training/demo project. The bank, projects, people and tasks are all fictitious. It is not affiliated with, endorsed by, or representative of any real bank or financial institution.
