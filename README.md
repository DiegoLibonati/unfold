# Unfold

## Educational Purpose

This project was created primarily for **educational and learning purposes**.  
While it is well-structured and could technically be used in production, it is **not intended for commercialization**.  
The main goal is to explore and demonstrate best practices, patterns, and technologies in software development.

## Description

**Unfold** is a lightweight FAQ (Frequently Asked Questions) accordion component built entirely with vanilla TypeScript — no frameworks, no runtime dependencies. It renders a list of questions where each item can be expanded to reveal its answer. Only one question can be open at a time: clicking an open question collapses it, and clicking a different one automatically closes the current one before opening the new one.

The component is designed as a self-contained, embeddable section meant to be dropped into any web page. It follows a clean component lifecycle pattern where each element exposes an optional `cleanup()` method to safely remove event listeners when the component is unmounted, preventing memory leaks in long-lived applications.

Under the hood, the architecture separates concerns into two layers: the `Question` component handles the rendering and interaction of a single expandable item, while `UnfoldPage` orchestrates the full list, manages the single-open state, and delegates cleanup to each child. Questions data is loaded from a static constants file, making it trivial to swap in dynamic data from an API.

The project includes a full test suite using Jest, Testing Library, and ts-jest, with coverage enforced at 70% across branches, functions, lines, and statements. Code quality is maintained through ESLint, Prettier, and a Husky pre-commit hook that runs lint and format checks on staged files automatically.

## Technologies used

1. Typescript
2. CSS3
3. HTML5
4. Vite

## Libraries used

The project ships with zero runtime dependencies. All tooling lives in `devDependencies`:

#### Dependencies

```
No production dependencies - Pure Vanilla TypeScript
```

#### devDependencies

```
"@eslint/js": "^9.39.2"
"@testing-library/dom": "^10.4.0"
"@testing-library/jest-dom": "^6.6.3"
"@testing-library/user-event": "^14.5.2"
"@types/jest": "^30.0.0"
"@types/node": "^22.0.0"
"eslint": "^9.39.2"
"eslint-config-prettier": "^10.1.8"
"eslint-plugin-prettier": "^5.5.5"
"globals": "^17.3.0"
"husky": "^9.1.7"
"jest": "^30.3.0"
"jest-environment-jsdom": "^30.3.0"
"lint-staged": "^16.2.7"
"prettier": "^3.8.1"
"ts-jest": "^29.4.6"
"typescript": "^5.2.2"
"typescript-eslint": "^8.54.0"
"vite": "^7.1.6"
```

## Getting Started

To run the application locally:

1. Clone the repository
2. Navigate to the project folder
3. Ensure you have **Node.js >= 22** installed (see `.nvmrc`)
4. Execute: `npm install`
5. Execute: `npm run dev`

The application will open automatically at `http://localhost:3000`.

### Pre-Commit for Development

Code quality is enforced automatically via a Husky pre-commit hook that runs `lint-staged` on staged files (ESLint + Prettier). The hook is installed automatically when you run `npm install`, so no extra setup is required.

To run the checks manually:

```bash
npm run lint           # Check code style
npm run lint:fix       # Auto-fix lint issues
npm run format         # Format with Prettier
npm run type-check     # TypeScript type-check only
```

## Testing

Once the project is set up, you can run the test suite (Jest + Testing Library + ts-jest):

1. Navigate to the project folder
2. Execute: `npm test`

For coverage report:

```bash
npm run test:coverage
```

Coverage is enforced at 70% across branches, functions, lines, and statements.

## Continuous Integration

The repository ships with a **GitHub Actions** pipeline defined in [`.github/workflows/ci.yml`](.github/workflows/ci.yml). It runs automatically on every `push` and `pull_request` targeting the `main` branch.

[![CI](https://github.com/DiegoLibonati/Questions-Page/actions/workflows/ci.yml/badge.svg)](https://github.com/DiegoLibonati/Questions-Page/actions/workflows/ci.yml)

### Pipeline overview

```
                      ┌─── PR or push to main ───┐
                      ▼                          ▼
┌──────────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│    lint-and-audit    │─▶│     testing      │─▶│      build       │
│   eslint · tsc       │  │      jest        │  │   tsc + vite     │
└──────────────────────┘  └──────────────────┘  └──────────────────┘
```

### Validation jobs (run on every PR and push)

1. **`lint-and-audit`** — installs Node.js from [`.nvmrc`](.nvmrc), runs `npm ci`, then `npm run lint` (ESLint) and `npm run type-check` (`tsc --noEmit`).
2. **`testing`** — depends on `lint-and-audit`. Installs dependencies and runs the full Jest suite via `npm run test`.
3. **`build`** — depends on `testing`. Installs dependencies and runs `npm run build` (`tsc` + `vite build`) to verify the project compiles to `dist/`.

All jobs run on `ubuntu-latest` and reuse the Node version declared in [`.nvmrc`](.nvmrc), with the npm cache enabled via `actions/setup-node@v4`.

### Where the build outputs live

| Output                                    | Location                     |
| ----------------------------------------- | ---------------------------- |
| Validation logs (lint, type-check, tests) | **Actions** tab on GitHub    |
| Build artifact (`dist/`)                  | Ephemeral, inside the runner |

> **Note:** This pipeline only validates the code — it does not publish releases or deploy artifacts. The `dist/` folder produced by the `build` job is discarded when the runner shuts down.

### Running the same checks locally

```bash
# lint-and-audit
npm run lint
npm run type-check

# testing
npm run test

# build
npm run build
```

## Security Audit

Check for vulnerabilities in dependencies:

```bash
npm audit
```

## Known Issues

None at the moment.

## Portfolio Link

[`https://www.diegolibonati.com.ar/#/project/unfold`](https://www.diegolibonati.com.ar/#/project/unfold)
