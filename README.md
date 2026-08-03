# The Book Thieves — MEC

Official website for **The Book Thieves**, the literary club of Model Engineering College, Kochi.

## Tech stack

- [React](https://react.dev/)
- [Vite](https://vitejs.dev/)
- React Router
- Framer Motion and AOS for animation
- Docker and Nginx for containerized production hosting

## Prerequisites

- Node.js 18 or later (the current LTS release is recommended)
- npm (installed with Node.js)

## Run locally

Clone your fork and install the locked dependency versions:

```bash
git clone https://github.com/<your-github-username>/tbt-website.git
cd tbt-website
npm ci
```

Start the development server:

```bash
npm run dev
```

`npm start` is an equivalent command. Vite prints the local URL in the terminal, normally `http://localhost:5173`. If that port is already used, open the alternate URL Vite prints instead.

## Available commands

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start the Vite development server with hot reload. |
| `npm start` | Alias for `npm run dev`. |
| `npm run build` | Create an optimized production build in `build/`. |
| `npm run serve` | Preview the production build locally. Run `npm run build` first. |

## Project structure

```text
src/
├── components/       Reusable UI components
├── hooks/            Custom React hooks
├── pages/            Route-level pages (`/` and `/vision`)
├── sections/         Home-page sections and historical team data
├── styles/           Section and global stylesheets
├── eventsData.json   Content for the initiatives carousel
├── App.jsx           Routes, navigation, and global audio control
└── index.jsx         Application entry point
public/
├── assets/           Static site imagery
├── eventImages/      Images referenced by `eventsData.json`
└── teamMembers*/     Historical team-member images
```

## Updating website content

### Major initiatives

Edit [`src/eventsData.json`](src/eventsData.json) to add or update a card in the “Our Major Initiatives” carousel. Put its image in `public/eventImages/` and use that filename as `imagePath`.

### Team members

The currently displayed team is defined in [`src/sections/teamMembers2023.json`](src/sections/teamMembers2023.json) and rendered by [`src/sections/Team.jsx`](src/sections/Team.jsx). Add the corresponding image to `public/teamMembers23Images/`.

The vision page also presents earlier teams using the data files in `src/sections/` and their matching folders in `public/`.

### Pages and visual sections

- `src/pages/Home.jsx` composes the landing page.
- `src/pages/Vision.jsx` contains the Vision page.
- Home-page content sections live in `src/sections/`.
- Keep section-specific styles in the corresponding file under `src/styles/`.

## Build and deployment

Before opening a pull request, make sure the production build succeeds:

```bash
npm run build
```

The project includes a multi-stage Docker build that builds the Vite application and serves the result through Nginx:

```bash
docker build -t tbt-website .
docker run --rm -p 8080:80 tbt-website
```

Then visit `http://localhost:8080`.

## Contributing

1. Fork the club repository and clone **your fork**.
2. Create a descriptive branch, for example `docs/improve-readme` or `fix/mobile-navigation`.
3. Make focused changes and run `npm run build`.
4. Commit and push the branch to your fork.
5. Open a pull request from your fork to `tbtMEC/tbt-website`.

Do not commit `node_modules/` or the generated `build/` directory. Keep image files appropriately compressed, use meaningful alt text, and avoid unrelated formatting changes in the same pull request.

## Troubleshooting

**`npm run dev` or `npm start` shows a page from another project**

Another Vite server may already be using port 5173. Use the different localhost URL printed by Vite, or stop the other development server.

**`RefreshRuntime.getRefreshReg is not a function`**

This usually means the browser is connected to a stale or different Vite server. Stop running Vite processes, start this project from its repository directory, and open the URL printed by that command. Hard-refresh the browser if needed.

**Dependencies are out of sync**

Delete `node_modules/`, then run `npm ci` again. This installs exactly the versions recorded in `package-lock.json`.
