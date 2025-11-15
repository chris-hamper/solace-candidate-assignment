# Solace Candidate Assignment

This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

1. Install dependencies

    ```bash
    npm i
    ```

2. Create a local copy of the `.env` file:

    ```bash
    cp .env .env.local
    ```

    and uncomment the `DATABASE_URL` line in `.env.local`, so that services will connect to the local DB container created in the "Database set up" section.

3. Run the development server:

    ```bash
    npm run dev
    ```

## Database set up

The app is configured to return a default list of advocates. This will allow you to get the app up and running without needing to configure a database. If you’d like to configure a database, you’re encouraged to do so. You can uncomment the url in `.env` and the line in `src/app/api/advocates/route.ts` to test retrieving advocates from the database.

1. Feel free to use whatever configuration of postgres you like. The project is set up to use docker-compose.yml to set up postgres. The url is in .env.

    ```bash
    docker compose up -d
    ```

    If possible, use firewall rules to ensure that the DB container created can only be accessed from localhost. This will avoid any possibility of malicious modificiation of the DB. This is OS specific:

    - On Ubuntu, the default configuration of `ufw` will prevent outside access to all ports:

      `sudo ufw enable`

2. Create a `solaceassignment` database.

3. Push migration to the database

    ```bash
    DATABASE_URL=postgresql://postgres:password@127.0.0.1:5432/ solaceassignment npx drizzle-kit push
    ```

4. Seed the database

    ```bash
    curl -X POST http://localhost:3000/api/seed
    ```
