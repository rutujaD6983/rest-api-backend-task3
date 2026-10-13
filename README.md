# Task 3 — REST API & Backend Development

A complete beginner-friendly RESTful API built using **Node.js, Express, and SQLite**. The API manages items and includes CRUD operations, input validation, consistent JSON error handling, query filtering, persistent database storage, automated tests, and API documentation.

## Features

- REST endpoints for Create, Read, Update, and Delete (CRUD).
- SQLite database integration; the database file is created automatically.
- Input validation for IDs, title, description, status, JSON format, and unsupported fields.
- Consistent JSON success and error response structures.
- Search and status filters for listing items.
- Prepared SQL statements with bound parameters.
- OpenAPI specification and an importable Postman collection.
- Automated API test covering the main CRUD flow and error handling.

## Requirements

- Node.js 18 or newer
- npm (included with Node.js)

## Setup and run

1. Download or clone this repository.
2. Open a terminal inside the project directory.
3. Install packages:

   ```bash
   npm install
   ```

4. Copy `.env.example` to `.env` if you want to customize the port/database path. The default configuration works without an `.env` file.
5. Start the server:

   ```bash
   npm start
   ```

6. Open `http://localhost:3000/api/health` in your browser. You should get a JSON response with `"status": "ok"`.
7. Test the CRUD operations using Postman or the cURL examples in `docs/API_DOCUMENTATION.md`.

The SQLite database is stored at `data/items.sqlite`. It is generated at runtime and excluded from Git so each installation can create its own database.

## API endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/health` | Check server status |
| `GET` | `/api/items` | List items; optional `status` and `search` filters |
| `GET` | `/api/items/:id` | Read one item |
| `POST` | `/api/items` | Create an item |
| `PUT` | `/api/items/:id` | Replace the editable fields |
| `PATCH` | `/api/items/:id` | Update selected fields |
| `DELETE` | `/api/items/:id` | Delete an item |

For request/response examples, error codes, field rules, and the data model, read **[docs/API_DOCUMENTATION.md](docs/API_DOCUMENTATION.md)**. The machine-readable spec is **[docs/openapi.yaml](docs/openapi.yaml)**. The Postman collection is **[docs/postman_collection.json](docs/postman_collection.json)**.

## Data model

Each item has an auto-generated integer `id`, a required `title` (1–120 characters), optional `description` (maximum 1000 characters), a `status` (`pending`, `in_progress`, or `completed`), and generated `created_at` and `updated_at` timestamps.

## Run automated tests

```bash
npm test
```

The test initializes an in-memory SQLite database, checks the health endpoint, creates and reads an item, updates it with PUT and PATCH, deletes it, and verifies validation/404 responses. It does not change your persistent `data/items.sqlite` file.

## Project structure

```text
rest-api-backend-task3/
├── server.js
├── package.json
├── .env.example
├── src/
│   ├── app.js
│   ├── db.js
│   ├── routes/items.js
│   └── middleware/
│       ├── AppError.js
│       ├── errorHandler.js
│       └── notFound.js
├── tests/api.test.js
└── docs/
    ├── API_DOCUMENTATION.md
    ├── openapi.yaml
    └── postman_collection.json
```

## Error handling and input safety

Invalid inputs return `400 Bad Request`; missing items/routes return `404 Not Found`; unexpected errors return a generic `500 Internal Server Error` JSON response. SQL statements use parameters instead of concatenating user values into SQL. The server has a JSON body-size limit and does not reveal internal error details to clients.

## Publish to GitHub

1. Sign in to GitHub and create a new **public** repository named `rest-api-backend-task3` (a private repository is also fine if the evaluator has access).
2. Extract this project ZIP on your computer/phone.
3. Upload the **contents of the extracted folder** to the repository—not just the ZIP file. Make sure `server.js`, `package.json`, `src/`, `tests/`, `docs/`, and this README appear at the repository's root.
4. Open the repository page and copy its URL. That is the GitHub repository link to submit.

Do not upload `node_modules`, `.env`, or your generated SQLite database.

## Optional deployment

The API can be deployed to a Node.js host that supports a persistent writable filesystem. Set `PORT` if the host provides it and set `DB_FILE` to a writable location. For hosting environments with an ephemeral filesystem, use a managed database or persistent disk so data survives restarts.

## License

MIT
