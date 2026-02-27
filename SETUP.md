# Setup Instructions

This guide covers local development and server deployment for **direkt.bahn.guru**.

## Architecture Overview

The project consists of two components:

1. **Frontend** (this repository) — A static single-page application built with Mapbox GL JS that visualises direct train connections on a map.
2. **Backend** ([api.direkt.bahn.guru](https://github.com/juliuste/api.direkt.bahn.guru)) — An API server that computes and serves direct connections for each station. It queries departure data from Deutsche Bahn and derives all reachable destinations.

The frontend also uses the public **[v6.db.transport.rest](https://v6.db.transport.rest/)** API (powered by [db-vendo-client](https://github.com/public-transport/db-vendo-client)) for station search / autocomplete.

## Prerequisites

- [Node.js](https://nodejs.org/) >= 18 (LTS recommended)
- npm (included with Node.js)
- A [Mapbox](https://www.mapbox.com/) access token (a default one is included for development, but you should use your own for production)

## Local Development

### 1. Clone the repository

```bash
git clone https://github.com/juliuste/direkt.bahn.guru.git
cd direkt.bahn.guru
```

### 2. Install dependencies

```bash
npm install
```

### 3. Build the project

```bash
npm run build
```

This bundles the frontend source files into `assets/bundle.js` using Webpack.

### 4. Serve locally

Since this is a static site, you can serve it with any HTTP server. For example:

```bash
npx serve .
```

Then open [http://localhost:3000](http://localhost:3000) in your browser.

### 5. Development workflow

After making changes to files in `src/`, rebuild with:

```bash
npm run build
```

Then refresh the browser to see your changes.

### Linting

```bash
# Check for issues
npm run lint

# Auto-fix issues
npm run fix
```

### Running all checks

```bash
npm test
```

This runs linting, dependency checking, and the production build.

## Server Deployment

### GitHub Pages (default)

The project includes a GitHub Actions workflow (`.github/workflows/ci.yaml`) that automatically deploys to GitHub Pages on every push to `main`:

1. It runs linting and builds the project.
2. It copies `index.html` and `assets/` into a deploy folder.
3. It deploys to the `gh-pages` branch using [JamesIves/github-pages-deploy-action](https://github.com/JamesIves/github-pages-deploy-action).

To use this for your own fork:

1. Fork the repository.
2. Go to **Settings → Pages** and set the source to the `gh-pages` branch.
3. Push changes to `main` — the CI workflow will build and deploy automatically.

### Custom server

To deploy on your own server:

1. Build the project:

   ```bash
   npm install
   npm run build
   ```

2. Copy the following files to your web root:

   - `index.html`
   - `assets/` (contains `bundle.js`, `styles.css`, `filter.css`, and images)

3. Serve the files with any static HTTP server (nginx, Apache, Caddy, etc.).

   **Example nginx config:**

   ```nginx
   server {
       listen 80;
       server_name direkt.example.com;
       root /var/www/direkt.bahn.guru;
       index index.html;

       location / {
           try_files $uri $uri/ /index.html;
       }
   }
   ```

### Mapbox access token

The source code includes a default Mapbox access token for development. For production deployments, replace the token in `src/index.js` with your own:

```js
mapboxGl.accessToken = 'your-mapbox-access-token'
```

You can obtain a token at [mapbox.com/account/access-tokens](https://account.mapbox.com/access-tokens/).

## Backend (api.direkt.bahn.guru)

The direct connections data is served by a separate backend. If you want to run the full stack:

1. Clone the backend repository:

   ```bash
   git clone https://github.com/juliuste/api.direkt.bahn.guru.git
   cd api.direkt.bahn.guru
   ```

2. Install dependencies and start the server:

   ```bash
   npm install
   PORT=3000 npm start
   ```

3. Optionally configure Redis for caching by setting the `REDIS_URI` environment variable.

4. Update the frontend's API URL in `src/index.js` to point to your backend instance (search for `api.direkt.bahn.guru`).

> **Note:** The backend originally used `db-hafas` (based on the discontinued DB HAFAS API). It needs to be updated to use [db-vendo-client](https://github.com/public-transport/db-vendo-client) as the data source. See the [db-vendo-client documentation](https://github.com/public-transport/db-vendo-client) for migration guidance.

## API Migration Notes

In late 2024, Deutsche Bahn discontinued the unofficial HAFAS API that this project originally relied on. The following changes have been made to adapt:

- **Station search**: Migrated from `v5.db.transport.rest` to `v6.db.transport.rest`, which uses [db-vendo-client](https://github.com/public-transport/db-vendo-client) as its backend.
- **Polyfill removal**: Removed references to `polyfill.io` (the domain was compromised in 2024). The `Intl.Locale` and `Intl.GetCanonicalLocales` APIs are now natively supported in all modern browsers.
- **Backend**: The `api.direkt.bahn.guru` backend needs to be separately migrated from `db-hafas` to `db-vendo-client`.
