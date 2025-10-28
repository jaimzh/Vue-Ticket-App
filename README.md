# Vue Ticket App

A simple ticket management app built with Vue. Create, view, edit, and delete tickets. This README is intentionally short and focused — quick setup and usage so you can get started fast.

## Live Demo
https://hng-ticket-management-app-vue.netlify.app/

## Features
- Create new tickets with title and description
- View a list of tickets
- Edit and delete tickets
- Simple client-side state management (Vue reactive state / Vuex / Pinia)
- Clean, minimal UI

## Tech
- Vue (JavaScript — Vue 3 recommended)
- Optional: Vite or Vue CLI for local development
- CSS (or your preferred styling solution)
- Node.js and npm/yarn for local development

## Quick Start

1. Clone the repo
```bash
git clone https://github.com/jaimzh/Vue-Ticket-App.git
cd Vue-Ticket-App
```

2. Install dependencies
```bash
npm install
# or
yarn
```

3. Run the app locally
- If the project uses Vite:
```bash
npm run dev
# or
yarn dev
```
- If the project uses Vue CLI:
```bash
npm run serve
# or
yarn serve
```

If you're unsure which command, check the "scripts" section of package.json for the available dev command.

4. Open the app in your browser (common addresses)
- Vite: http://localhost:5173
- Vue CLI: http://localhost:8080

## Build
To create a production build:
```bash
npm run build
# or
yarn build
```

To serve a production build locally you can use a static server such as `serve`:
```bash
npm install -g serve
serve -s dist
```

## Project Structure (example)
- src/ — Vue source code (components, views, router, store)
- public/ — static files
- package.json — scripts and dependencies
- README.md — this file

(Adjust paths above if your project structure differs.)

## Contributing
- Open an issue to discuss major changes.
- Create a pull request for fixes or small features.
- Keep changes focused and include a short description.

## Notes & Next Steps
- If you'd like a Live Demo link added, tell me the deployment URL and I will update the README.
- If you want, I can add CI (GitHub Actions) for build checks, or add a LICENSE file (MIT by default).

## License
MIT — see the LICENSE file (or add one) if you want an explicit license.