# SciBowl Vault — Cloudflare shared database

## Package
- `index.html`: updated frontend with cloud loading, server-verified moderator login, shared writes, and 15-second refresh.
- `worker.js`: Cloudflare Worker API.
- `schema.sql`: D1 schema.
- `wrangler.toml`: configuration template.
- `bank.json`: included if it was present in the working session.

## Setup
1. In Cloudflare, create a D1 database named `scibowl-vault`.
2. Open its SQL Console and run all of `schema.sql`.
3. Create a Worker named `scibowl-vault-api`; paste in `worker.js` and deploy.
4. Add a D1 binding to the Worker: variable name `DB`, database `scibowl-vault`.
5. Add these Worker secrets under Settings → Variables and Secrets:
   - `MODERATOR_PASSWORD`: choose a new private password.
   - `TOKEN_SECRET`: a separate long random secret (32+ random characters).
   - `ALLOWED_ORIGIN`: recommended; set to your GitHub Pages origin, e.g. `https://YOUR-USERNAME.github.io`.
6. Copy the deployed Worker URL.
7. In `index.html`, replace `https://YOUR-WORKER-NAME.YOUR-SUBDOMAIN.workers.dev` in `API_BASE` with your Worker URL (no trailing slash).
8. Push the updated `index.html` to GitHub Pages.
9. Open the site, enter the moderator password, and use the moderator bank upload control to upload `bank.json`. This replaces the shared bank.

Do not put `MODERATOR_PASSWORD` or `TOKEN_SECRET` in frontend code or GitHub. Visitors can read the public bank; only a valid Worker-issued moderator token can write. Moderator sessions last 12 hours. The browser refreshes the shared bank about every 15 seconds.

The current frontend syncs the full bank after a moderator save. Avoid simultaneous editing from multiple moderator browsers. Keep an exported backup before replacing the bank.
