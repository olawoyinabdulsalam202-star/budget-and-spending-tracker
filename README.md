# Budget and spending tracker

A budget app for one person, kept one month at a time. The person
types in what they spend, and each spend goes into a category, such as
groceries or transport. Every figure on the screen is the sum of the
entries behind it. At the end of the month, the month closes and its
figures stop changing. What is left over, or what was overspent, then
moves into the next month by a rule the person can read.

## What finished looks like

Anyone can open the app and:

- create an account, and confirm that the email address is theirs
- set a budget for a month that starts on the day they choose, such as
  payday on the 25th
- record what they spend, in categories
- close a month, and carry what is left into the next one
- get a warning while there is still time to change their spending

On the day the new month starts, the old month closes and the new one
opens with nobody pressing anything.

Finished means it is live on the internet, not just on your laptop.
Someone you have never met can use it, and their month still turns
over on the right day when your laptop is shut.

## The road map

Eight sprints. Each sprint delivers one part of the list above. They
must be done in order, because each one builds on the one before.

| # | Sprint | What it delivers |
|---|---|---|
| 1 | Getting in | People create an account, come back to it, and sign out safely on a shared computer |
| 2 | What went out | People record spending in categories. Every total is the sum of the entries |
| 3 | The shape of a month | People set a budget for a month that starts on the day they choose |
| 4 | Closing the books | A month closes, its figures stop changing, and what is left moves into the next month |
| 5 | Told in time | People are warned while there are still days left to act |
| 6 | The day it turns over | The month closes and the warnings go out on their own |
| 7 | An address that is theirs | Email only goes to an address the person has confirmed |
| 8 | Out in the world | The app is live on the internet, and the monthly work runs there |

## The hard parts

**The edge of a month.** Most of the hard questions happen at the end
of a month. A receipt dated the 31st is typed in on the 2nd. A budget
is raised on the 20th when most of it is already spent. Two categories
are merged after six months of history. Each time, the product has to
decide which month the money belongs to.

**What carries over.** Money left over at the end of a month can move
into the next month, or not. Money overspent can be taken off next
month, or forgiven. There is more than one right answer. This road map
does not choose for you. You choose, you write the rule where the
person can read it, and you keep to it in every sprint after that.

**Every figure comes from the entries.** A month's figures are always
worked out from the spending recorded in it. A total stored on its own
and changed by hand cannot answer the question "where did that number
come from?". That is the question people ask a budget most.

## No bank connection needed

Nothing in this product connects to a bank or a payment provider.
People type in what they spend. You do not need an account with any
outside service to build it, except an email service to send messages.
Its password goes in the environment settings, never in this
repository.

## Working on it

The sprints, their briefs and their tickets are in Blacksmith
Experience. Start with sprint 1. Each sprint opens when the one before
it is finished.

Read the documents attached to a sprint before you start its first
ticket. They explain the choices you will make and the options you are
choosing between.

## What this is built with

The whole product: an API and the screens that use it.

- **Express 5** in **TypeScript** for the API.
- **Prisma** for the models and the migrations, against the schema in `prisma/schema.prisma`.
- **Zod** for validating every request, and **zod-to-openapi** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **JWT** for authentication, in `src/modules/auth`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, Prisma client, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the JWT secret, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── prisma/
│   └── schema.prisma    # your models, and the migrations they generate
├── src/
│   ├── config/          # environment, OpenAPI, Zod setup
│   ├── db/              # the Prisma client, made once
│   ├── middleware/      # authentication, validation, error handling
│   ├── modules/
│   │   └── auth/        # register, log in, refresh
│   ├── utils/           # errors, pagination, tokens
│   ├── app.ts           # where routes are mounted
│   └── index.ts
├── package.json
└── tsconfig.json

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a Zod schema or a route. |
| `blacksmith make:resource Post` | Scaffold a Prisma model, Zod schemas, a service, a controller and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run an npm script in the backend, e.g. `blacksmith backend run migrate`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Express and React project. Nothing here is a dependency on us. |
