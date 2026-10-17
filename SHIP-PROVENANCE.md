# Ship Dev acceptance

This repository retains Uptime Kuma 2.5.6, upstream commit
`7ca4a3debd76443d8452da95d4040200a1479af4`, its history and MIT license.
Upstream: https://github.com/louislam/uptime-kuma

The only application manifest adaptation is `scripts.start`: the original
`npm run start-server` is expanded to its existing command `node server/server.js`.
The Dev entrypoint detector otherwise selected the browser's `src/main.js` and
failed to parse `src/App.vue`. No server, frontend, database or dependency code
was changed. The upstream lockfile remains byte-identical.

Local control: `npm ci --no-audit --no-fund`, `npm run build`, then the original
server. Real Chromium checks cover SQLite setup, admin creation, monitor create
and edit, reload, Socket.IO and persistence across SIGTERM plus server restart.
Only a synthetic group monitor is created; no third-party monitoring target or
notification provider is configured. Test credentials and databases stay outside
version control.

The native dependency is `@louislam/sqlite3` 15.1.6: SQLite 3.41.1 C sources,
four C++ binding sources, node-addon-api and N-API 6. Local Linux ELF binaries
are a functional control only. Dev acceptance requires compiling the original
native sources with the platform's own Wasm compiler and executing N-API;
prebuilt Wasm substitutes and JavaScript-only Wasm output do not qualify.
