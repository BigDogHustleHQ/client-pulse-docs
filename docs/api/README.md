# ClientPulse API

GraphQL API layer for the ClientPulse platform, built with **NestJS 11** and
**Apollo Server 5** using a code-first schema.

- **Repo:** [client-pulse-api](https://github.com/BigDogHustleHQ/client-pulse-api)
- **Protocol:** GraphQL over HTTP
- **Endpoint:** `POST <baseUrl>/graphql`
- **Schema (source of truth):** [`docs/api-spec.graphql`](https://github.com/BigDogHustleHQ/client-pulse-api/blob/main/docs/api-spec.graphql) in the api repo — auto-generated from the resolvers, do not edit by hand.

## Environments

The API runs in four environments. Each serves the same GraphQL endpoint at
`<baseUrl>/graphql`.

| Environment   | Base URL                 | Purpose                                                          |
| ------------- | ------------------------ | ---------------------------------------------------------------- |
| `local`       | `http://localhost:4000`  | Local dev server (`npm run start:dev`)                           |
| `development` | _TBD (Railway)_          | Shared integration target for the frontend, WebSocket, and Integration Hub |
| `staging`     | _TBD (Railway)_          | Production mirror / release gate                                 |
| `production`  | _TBD (Railway)_          | Live                                                             |

URLs for the deployed environments are filled in once the Railway services are
provisioned. The matching Bruno environment files live in
[`bruno/environments/`](https://github.com/BigDogHustleHQ/client-pulse-api/tree/main/bruno/environments).

## Schema

The schema is **code-first**: TypeScript resolvers drive the SDL, which NestJS
writes to `src/schema.gql` at startup and publishes to
[`docs/api-spec.graphql`](https://github.com/BigDogHustleHQ/client-pulse-api/blob/main/docs/api-spec.graphql).
To change the schema, edit the resolver — never the `.graphql` file.

Current operations (see the canonical spec for the always-current list):

| Type  | Field    | Returns   | Description                       |
| ----- | -------- | --------- | --------------------------------- |
| Query | `health` | `String!` | Liveness check — returns `"ok"`.  |

## Testing the API

The api repo ships a [Bruno](https://www.usebruno.com/) collection under
[`bruno/`](https://github.com/BigDogHustleHQ/client-pulse-api/tree/main/bruno)
with one request per operation. Each request carries an `assert` block
(status / type / no-errors checks) and a `tests` block (value assertions).

```bash
# from the client-pulse-api repo
npm install
npm run start:dev      # start the API — generates src/schema.gql
npm run test:api       # run the full Bruno suite against the local env
```

| Command                                   | What it does                                              |
| ----------------------------------------- | --------------------------------------------------------- |
| `npm run test:api`                        | Runs the collection against `local`.                      |
| `npm run test:api:ci`                     | Same, writes a JUnit report to `test-results/` for CI.    |
| `bru run bruno -r --env <name>`           | Runs against any named environment (`development`, etc.). |
| `bru run bruno -r --env-var baseUrl=<url>`| Runs against an ad-hoc URL (e.g. a Railway PR preview).   |

New resolvers are scaffolded into the collection automatically by
`scripts/sync-api-docs.mjs` in the api repo, which also keeps
`docs/api-spec.graphql` in sync.
