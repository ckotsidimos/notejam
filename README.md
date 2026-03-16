# NotejJam

A web-based note-taking application that lets users organize notes into collections called **pads**. Built with Node.js/Express and deployed to Azure via Terraform and GitHub Actions.

## Features

- **User accounts** — Register, sign in, change password, password recovery
- **Pads** — Organize notes into named collections
- **Notes** — Create, edit, delete, and browse notes with timestamps

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Node.js, Express 4 |
| Templates | Jade |
| Auth | Passport.js (local strategy), bcrypt |
| Database | SQLite3 |
| Email | Nodemailer (stub transport in dev) |
| Testing | Mocha, Should.js, Supertest |
| Infrastructure | Terraform, Azure App Service |
| CI/CD | GitHub Actions |

## Getting Started

### Prerequisites

- Node.js
- npm

### Install & Run

```bash
npm install
npm start
```

The app runs on `http://localhost:3000` by default.

### Run Tests

```bash
npm test
```

## Project Structure

```
├── app.js              # Express app setup
├── db.js               # Database initialization
├── models.js           # ORM model definitions
├── helpers.js          # Utility functions
├── settings.js         # Environment config
├── routes/
│   ├── notes.js        # Note CRUD routes
│   ├── pads.js         # Pad CRUD routes
│   └── users.js        # Auth routes
├── views/              # Jade templates
├── public/             # Static assets
├── tests/              # Mocha test suite
└── Terraform/          # Infrastructure as Code
```

## Routes

| Method | Path | Description |
|---|---|---|
| GET/POST | `/signup` | Register |
| GET/POST | `/signin` | Login |
| GET | `/signout` | Logout |
| GET/POST | `/settings` | Change password |
| GET/POST | `/forgot-password` | Password recovery |
| GET | `/` | All notes (home) |
| GET/POST | `/notes/create` | Create note |
| GET/POST | `/notes/:id/edit` | Edit note |
| GET/POST | `/notes/:id/delete` | Delete note |
| GET/POST | `/pads/create` | Create pad |
| GET | `/pads/:id` | View pad |
| GET/POST | `/pads/:id/edit` | Edit pad |
| GET/POST | `/pads/:id/delete` | Delete pad |

## Database Schema

```
users   — id, email, password
pads    — id, name, user_id
notes   — id, name, text, pad_id, user_id, created_at, updated_at
```

## Infrastructure & Deployment

Infrastructure is defined in [Terraform/](Terraform/) and targets **Azure** (`northcentralus` region):

- **Resource Group** — `Kotsisflow-resources`
- **App Service Plan** — Linux, Basic B1 tier
- **App Service** — Runs Docker image `ckotsidimos/testapp1:latest`

### CI/CD (GitHub Actions)

| Trigger | Workflow | Action |
|---|---|---|
| PR to `master` | `terraform-plan.yml` | Runs `terraform plan` |
| Push to `master` | `terraform-apply.yml` | Runs `terraform apply --auto-approve` |

Required GitHub secrets: `ARM_CLIENT_ID`, `ARM_CLIENT_SECRET`, `ARM_SUBSCRIPTION_ID`, `ARM_TENANT_ID`
