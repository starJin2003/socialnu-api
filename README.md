# SocialNU API

Backend API for the [SocialNU](https://github.com/starJin2003/DISC-socialNU) web app. Built with Node.js and Express, connected to Supabase PostgreSQL. Serves student profile data to the React frontend.

## Endpoints

**`GET /users`** - Returns all users from the `users` table.

**`GET /users/profiles`** - Returns all users with their profile data joined from `user_profiles` (major, bio, shared classes, course tags). This is the main endpoint the frontend consumes.

## Tech Stack

- Node.js + Express 5
- Supabase JS client for database queries
- CORS enabled for frontend communication
- dotenv for environment variables

Originally connected to Render PostgreSQL using the `pg` library with raw SQL, then migrated to the Supabase JS client for cleaner relational queries.

## Database Schema (Supabase PostgreSQL)

**`users`** table:

| Column | Type | Description |
|--------|------|-------------|
| id | int4 | Primary key |
| first_name | text | Student's first name |
| last_name | text | Student's last name |
| email | text | Northwestern email |

**`user_profiles`** table:

| Column | Type | Description |
|--------|------|-------------|
| id | int4 | Foreign key to users.id |
| date_of_birth | date | Birthday |
| bio | text | Short bio (e.g., "Active DISC member") |
| major | text | Academic major |
| shared_classes | int4 | Number of shared classes with current user |
| tags | text[] | Array of course/org tags (e.g., ["CS 211", "MATH 228-1", "DISC"]) |

The `/users/profiles` endpoint joins these two tables using Supabase's relational query syntax:

```javascript
const { data, error } = await supabase
  .from('users')
  .select(`*, user_profiles (*)`);
```

## Getting Started

### Prerequisites

- Node.js 18+
- A Supabase project with the tables above set up

### Installation

```bash
git clone https://github.com/starJin2003/socialnu-api.git
cd socialnu-api
npm install
```

Create a `.env` file:

```
SUPABASE_URL=your_supabase_project_url
SUPABASE_ANON_KEY=your_supabase_anon_key
```

### Running

```bash
node server.js
```

Runs on `http://localhost:5001`.

## Related

- [DISC-socialNU](https://github.com/starJin2003/DISC-socialNU) - React frontend that consumes this API
