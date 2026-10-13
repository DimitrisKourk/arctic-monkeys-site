# Arctic Monkeys Fan Site

A single-page fan website for the band **Arctic Monkeys**, with a Node.js/Express backend. It has two REST APIs: one backed by a **JSON file** (discography) and one backed by a **MySQL database** (external links). An admin panel performs full CRUD operations on both.

## Features

**Visitor pages** (single page; sections are shown and hidden with JavaScript, no reloads):

- **Bio:** the band's style, members, history and legacy
- **Photos:** galleries of concerts, studio sessions and music videos
- **Discography:** song tables for all albums or for one album (*Suck It and See*, *AM*, *Tranquility Base Hotel & Casino*), loaded from the music API
- **Links:** music platforms, social media and the official website, loaded from the database by category

**Management panel:**

- **Manage Music:** add, update and delete songs in the JSON store
- **Manage Links:** add, update and delete links in the MySQL database
- Clicking a table row fills the form with that record for quick editing

## Tech stack

| Layer | Technology |
|---|---|
| Front end | HTML, CSS, vanilla JavaScript (`fetch` API) |
| Back end | Node.js, Express |
| Storage | JSON file (`api/music.json`) and MySQL (`mysql` driver) |

## API

### Songs (JSON file)

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/music` | List all songs |
| GET | `/api/music/:id` | Get one song |
| POST | `/api/music` | Add a song: `{ id, album, name, releaseYear }` |
| PUT | `/api/music/:id` | Update a song |
| DELETE | `/api/music/:id` | Delete a song |

### Links (MySQL)

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/links` | List all links |
| GET | `/api/links/:type` | Links of one type: `music`, `social` or `officialwebsite` |
| POST | `/api/links` | Add a link: `{ id, name, url, type }` |
| PUT | `/api/links/:id` | Update a link |
| DELETE | `/api/links/:id` | Delete a link |

All SQL queries use **parameterized placeholders** (`?`) to prevent SQL injection.

## Setup

### 1. Install dependencies

```bash
npm install
```

### 2. Create the database table

```sql
CREATE TABLE Links (
  id   INT PRIMARY KEY,
  Name VARCHAR(100),
  URL  VARCHAR(255),
  Type VARCHAR(50)   -- 'music', 'social' or 'officialwebsite'
);
```

### 3. Configure the database connection

Credentials are read from environment variables:

```bash
export DB_HOST=your_host
export DB_USER=your_user
export DB_PASSWORD=your_password
export DB_NAME=your_database
```

### 4. Run

```bash
node app.js
```

Open [http://localhost:4000](http://localhost:4000). Set `port` in the environment to use a different port.

## Limitations

- The management panel has **no authentication**, so anyone with access to the site can edit the data.
- Song data is stored in a JSON file that's rewritten on every change, which suits a demo but not concurrent use.

## Credits

Band photos and information belong to their respective owners and are used for educational purposes only. This is a non-commercial project, not affiliated with Arctic Monkeys.
