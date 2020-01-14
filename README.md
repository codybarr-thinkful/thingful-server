# Thingful Server

## Setting Up

-   Install dependencies: `npm install`
-   Create development and test databases: `createdb thingful`, `createdb thingful-test`
-   Create database user: `createuser thingful`
-   Grant privileges to new user in `psql`:
    -   `GRANT ALL PRIVILEGES ON DATABASE thingful TO thingful`
    -   `GRANT ALL PRIVILEGES ON DATABASE "thingful-test" TO thingful`
-   Prepare environment file: `cp example.env .env`
-   Replace values in `.env` with your custom values.
-   Bootstrap development database: `npm run migrate`
-   Bootstrap test database: `npm run migrate:test`

### Configuring Postgres

For tests involving time to run properly, your Postgres database must be configured to run in the UTC timezone.

1. Locate the `postgresql.conf` file for your Postgres installation.
    - OS X, Homebrew: `/usr/local/var/postgres/postgresql.conf`
2. Uncomment the `timezone` line and set it to `UTC` as follows:

```
# - Locale and Formatting -

datestyle = 'iso, mdy'
#intervalstyle = 'postgres'
timezone = 'UTC'
#timezone_abbreviations = 'Default'     # Select the set of available time zone
```

## Sample Data

-   To seed the database for development: `psql -U thingful -d thingful -a -f seeds/seed.thingful_tables.sql`
-   To clear seed data: `psql -U thingful -d thingful -a -f seeds/trunc.thingful_tables.sql`

## Scripts

-   Start application for development: `npm run dev`
-   Run tests: `npm test`

## Updates

**JWT / Checkpoint 3**

1. Implement a basic authentication middleware to use for protecting endpoints.
1. The GET /api/things endpoint should remain public.
1. The GET /api/things/:thing_id endpoint should be protected by basic auth.
1. The GET /api/things/:thing_id/reviews endpoint should be protected by basic auth.
1. The POST /api/reviews endpoint should be protected by basic auth and automatically assign a user_id.
1. The thingful-client should store the base64 encoded credentials when the login form is submitted.
1. The base64 encoded credentials should be sent in requests to protected endpoints.

**JWT / Checkpoint 4**

1. You should update your database seeding data to use hashed passwords. Generate the hashed passwords using bcrypt.
1. You'll also need to update your basic-auth middleware to use bcryptjs to compare the password in the basic token with the hash stored in the database.

**JWT / Checkpoint 5**

1. You should create a POST /login endpoint that responds with a JWT.
1. You'll need to change your middleware for protected endpoints to verify the JWT instead of verifying the base64 encoded basic auth header.
