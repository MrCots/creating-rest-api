# Book Club Scheduler

A small full-stack app for keeping track of books and their reading schedules. I use it to plan my lecture reading, but it works for any book club: add a book, set the start and end dates, edit it later, or remove it when you're done.

Built with a React front-end that talks to a Node.js/Express REST API.

## Features

- **Read**: load the list of books when the app starts
- **Create**: add a book with a title and a reading schedule (start and end dates)
- **Update**: change a book's title and/or schedule
- **Delete**: remove a book from the list

## Tech Stack

| Layer      | Technology            |
| ---------- | --------------------- |
| Front-end  | React 18.2.0          |
| Back-end   | Node.js, Express 4.18.2 |
| Dev tools  | nodemon (back-end), react-scripts (front-end) |

Originally developed with Node v19.0.0. See [Version Notes](#version-notes-and-troubleshooting) below.

## Project Structure

```
starting-code/
├── frontend/
│   └── src/
│       ├── api/
│       │   ├── index.js      # API_ENDPOINT (the server address)
│       │   └── books.js      # addNewBook, getBooks, updateBook, deleteBook
│       └── components/
│           ├── Book.js       # update/delete a single book
│           └── Booklist.js   # list books and add a new one
└── backend/
    ├── bin/www               # server entry point
    └── routes/Books.js       # the /books REST endpoints
```

All network calls live in `frontend/src/api/books.js`, so components import those functions instead of writing `fetch()` calls themselves.

## API Endpoints

| Method | Route         | Purpose                         |
| ------ | ------------- | ------------------------------- |
| GET    | `/books`      | Get all books                   |
| POST   | `/books`      | Add a new book                  |
| PUT    | `/books/:id`  | Update a book's title/schedule  |
| DELETE | `/books/:id`  | Delete a book                   |

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (see version notes below)
- npm (comes with Node)

### Install

Run this in **both** the `frontend` and `backend` folders:

```bash
npm install
```

### Run

You need two terminal windows. **Start the back-end first**, so the API is already available when the front-end loads and requests the book list.

```bash
# Terminal 1
cd backend
npm start

# Terminal 2
cd frontend
npm start
```

If the terminal says port 3000 is in use, answer `y` to run the front-end on another port. Make sure `API_ENDPOINT` in `frontend/src/api/index.js` points at the port your back-end is listening on.

Both servers reload automatically when you change code.

## Known Limitations

- Books are stored in memory on the back-end (the `ALL_BOOKS` array in `backend/routes/Books.js`). **Restarting the back-end resets the list to its starting data.**
- There is no authentication, so don't expose this API publicly as-is.

Ideas for next steps: persist data to a JSON file or a database (SQLite, MongoDB), add input validation, and add a "currently reading" view.

## Version Notes and Troubleshooting

This project uses older versions (React 18.2, Express 4.18, Node 19, `react-scripts`). Node 19 is no longer supported, so a current LTS release of Node is the better choice. If something breaks on a fresh install, try the fixes below.

### `Cannot find module 'ajv/dist/compile/codegen'`

**Cause:** `ajv-keywords` (pulled in by `react-scripts`) expects a newer version of `ajv` than the one npm installed.

**Fix**, in the `frontend` folder:

```bash
npm install ajv@8 --legacy-peer-deps
```

### Still broken? Do a clean reinstall

Delete `node_modules` and `package-lock.json`, then reinstall so npm resolves the dependency tree from scratch:

```bash
# macOS/Linux
rm -rf node_modules package-lock.json

# Windows PowerShell
Remove-Item -Recurse -Force node_modules, package-lock.json

npm install
```

### Port already in use

An earlier copy of the app is probably still running. Close the old terminal, or accept the prompt to use a different port.

### `GET / 404` in the back-end logs

This is harmless. The API only has routes under `/books`, so a request to `/` returns 404.

### Updating dependencies safely

1. Run `npm outdated` to see what is behind.
2. Update **one package at a time** (`npm install package@latest`).
3. Start both servers and test add, edit, and delete after each update.

Updating everything at once makes it very hard to tell which package broke the app. Major version jumps (for example React 19 or Express 5) can include breaking changes, so read the release notes first.

## Credits

Starter code and project idea come from the Codecademy tutorial **"Creating REST API Endpoints"**. The API functions in `frontend/src/api/books.js` were written by following that tutorial. If you plan to make this repository public, check that the tutorial's terms allow republishing the starter code.
