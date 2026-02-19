# Power Apps Code Apps + SvelteKit

A starter template for building [Power Apps code apps](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview) with [SvelteKit](https://svelte.dev/docs/kit), [Tailwind CSS](https://tailwindcss.com/), and TypeScript.

## Prerequisites

- [Node.js](https://nodejs.org/) (LTS)
- [pnpm](https://pnpm.io/)
- [Power Platform CLI](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction)
- [A Power Platform environment with code apps enabled](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/overview#enable-code-apps-on-a-power-platform-environment)

## Getting started

Scaffold a new project from this template:

```sh
pnpm dlx degit chumleesockson/power-apps-code-apps-sveltekit my-app
cd my-app
```

Authenticate, select your environment, install dependencies, and initialize your code app:

```sh
pac auth create
pac env select --environment <your-environment-id>
pnpm install
pac code init --displayname "My App"
```

## Developing

Start the dev server:

```sh
pnpm dev
```

Open the **Local Play** URL printed in the terminal to test inside the Power Apps host. Open it in the same browser profile as your Power Platform tenant.

## Building and publishing

Build and push to Power Apps in one step:

```sh
pnpm build | pac code push
```

## Connecting to data

Add a data source using the PAC CLI. For example, to add a Dataverse table:

```sh
pac code add-data-source -a dataverse -t <table-logical-name>
```

For other connectors (e.g. Office 365 Users, SQL), use a connection reference for portability across environments:

```sh
pac code add-data-source -a <apiName> -cr <connectionReferenceLogicalName> -s <solutionId>
```

This auto-generates typed model and service files in `generated/`. Use them in your Svelte components:

```svelte
<script lang="ts">
  import { AccountsService } from './generated/services/AccountsService';
  import type { Accounts } from './generated/models/AccountsModel';

  const result = await AccountsService.getAll();
</script>
```

See the full guide: [Connect to data](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/how-to/connect-to-data) | [Connect to Dataverse](https://learn.microsoft.com/en-us/power-apps/developer/code-apps/how-to/connect-to-dataverse)

## Project structure

```
src/
  app.html          - HTML shell (includes Power Apps URL fix)
  routes/
    +layout.svelte  - Root layout with Tailwind CSS
    +layout.ts      - Disables SSR for static SPA output
    +page.svelte    - Home page
svelte.config.js    - SvelteKit config (adapter-static -> dist/)
vite.config.ts      - Vite config with Power Apps and SvelteKit plugins
power.config.json   - Power Apps metadata (managed by SDK/CLI)
```

## Key configuration

- **`adapter-static`** outputs to `dist/` to match `power.config.json`'s `buildPath`
- **`paths.relative`** ensures asset paths work on the Power Apps CDN
- **`app.html`** includes a `history.replaceState` fix so SvelteKit's router handles the deep URL path Power Apps serves from
- **`@microsoft/power-apps-vite`** provides the Vite plugin for local dev play and CORS

## Other commands

| Command | Description |
| --- | --- |
| `pnpm check` | Run svelte-check for type errors |
| `pnpm lint` | Check formatting and lint rules |
| `pnpm format` | Auto-format with Prettier |
| `pnpm test:unit` | Run unit tests with Vitest |
| `pnpm test:e2e` | Run end-to-end tests with Playwright |
