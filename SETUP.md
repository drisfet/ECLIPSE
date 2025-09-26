# Eclipse Theia IDE Setup Guide

This guide documents the setup process for running the Eclipse Theia IDE project locally. The project is a fork of [eclipse-theia/theia-ide](https://github.com/eclipse-theia/theia-ide) configured for mobile-first browser-based development.

## Prerequisites

### System Dependencies (Ubuntu/Debian-based systems)
The project requires build tools and libraries for native Node modules (e.g., node-pty, native-keymap). Install them via apt:

```bash
sudo apt-get update
sudo apt-get install -y build-essential libxkbfile-dev libx11-xkb-utils libxss1 libasound2 libgconf-2-4 libgtk-3-0 libnss3 libgdk-pixbuf2.0-0 libxss1 libxtst6 libxrandr2 libasound2 libpangocairo-1.0-0 libatk1.0-0 libcairo-gobject2 libgtk-3-0 libgconf-2-4 libgdk-pixbuf2.0-0 libxss1 libxtst6 libxrandr2 libasound2 libpangocairo-1.0-0 libatk-bridge2.0-0 libgtk-3-dev
```

### Node.js and Yarn
- Node.js: Version 18+ (aligned with Theia 1.64.1). Use nvm or download from [nodejs.org](https://nodejs.org).
- Yarn: Version 1.x (classic; Lerna monorepo compatible). Install via:

```bash
npm install -g yarn@1.22.22
```

Verify:
```bash
node --version  # Should be >=18
yarn --version  # Should be 1.x
```

## Cloning the Repository
Clone from your GitHub repo (this fork):

```bash
git clone https://github.com/drisfet/ECLIPSE.git
cd ECLIPSE
```

**Note**: The repo contains the full Theia IDE monorepo structure (Lerna workspaces in `applications/`, `theia-extensions/`).

## Installation
1. Install Node dependencies:

```bash
yarn install
```

This resolves the Lerna monorepo, applies patches (via theia-patch), and handles ~500+ packages (may take 5-10 minutes; warnings about unmet peers/deprecations are normal).

2. Download plugins (VS Code-compatible extensions):

```bash
yarn download:plugins
```

## Building and Running

### Development Mode (Recommended for Initial Testing)
Faster builds, unminified for debugging:

```bash
yarn build:dev
```

### Production Mode
Optimized/minified build:

```bash
yarn build
```

### Start Browser Version (Mobile-First Testing)
Launches Theia at http://localhost:3000 (access via iOS Safari for mobile testing):

```bash
yarn browser start
```

- Open http://localhost:3000 in browser.
- For iOS Safari: Ensure local network access (e.g., via ngrok if remote) or run on local machine.
- Default workspace: Empty; create/open folders via File menu.

### Start Electron Desktop Version
For desktop testing:

```bash
yarn electron start
```

### Packaging (Electron App)
Create distributable (Linux/Mac/Windows; note: cross-platform builds limited):

```bash
yarn electron package
```

Output in `applications/electron/dist/`.

## Project Structure
- **Root**: Lerna monorepo config (`lerna.json`, `package.json`).
- **applications/browser/**: Browser app (webpack config, resources).
- **applications/electron/**: Desktop app (electron-builder config, icons).
- **theia-extensions/**: Custom extensions (product branding, updater, launcher).
- **configs/**: ESLint/TS configs.
- **scripts/**: Build utilities.

## Common Commands
- **Clean/Rebuild**: `yarn clean && yarn install && yarn build:dev`
- **Test**: `yarn test` (unit tests).
- **E2E Tests (Electron)**: `yarn electron package:preview && yarn electron test`
- **Update Theia Version**: Edit `package.json` (theia dep), then `yarn install`.
- **Docker Build (Browser)**: `docker build -t theia-ide -f browser.Dockerfile . && docker run -p=3000:3000 --rm theia-ide`

## Troubleshooting
- **Native Build Errors**: Ensure system deps installed; retry `yarn install`.
- **Yarn Warnings**: Ignore unmet peers (e.g., TypeScript versions); non-blocking.
- **Port Conflicts**: Change port in `applications/browser/package.json` scripts.
- **Mobile Testing**: Use Chrome DevTools device emulation or real iOS Safari. Note: Touch support limited (see GitHub issue #3557); enhancements needed for mobile-first.
- **VS Code Extensions**: Install via Extensions view (Open VSX registry); compatibility via VS Code protocol.
- **Performance**: Startup ~10-20s; optimizations in recent releases (1.63+).

## Mobile-First Enhancements (Project Vision)
This fork targets iOS Safari without native restrictions:
- **Current State**: Basic responsive via CSS; Monaco editor WebKit-compatible.
- **Next Steps**: Add PWA manifest, touch gestures (Lumino widgets), VS Code-like sidebar for extensions (e.g., Kilo Code).
- **Feasibility**: High; leverage Browser-Only Mode (#12852) for lightweight deploys.

For issues, refer to [Theia Docs](https://theia-ide.org/docs/) or GitHub issues.

Last Updated: 2025-09-26