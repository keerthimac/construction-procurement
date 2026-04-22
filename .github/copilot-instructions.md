# AI Coding Guidelines for Arch-Procurement Electron App

## Architecture Overview
This is an Electron desktop application built with React, TypeScript, and Vite. The app follows a standard Electron architecture:
- **Main Process** (`electron/main.ts`): Manages the application lifecycle, creates BrowserWindow, handles system integration
- **Renderer Process** (`src/`): React-based UI running in Chromium
- **Preload Script** (`electron/preload.ts`): Securely exposes Electron APIs to renderer via contextBridge

## Development Workflow
- **Start dev server**: `npm run dev` - Launches Vite dev server and Electron app with hot reload
- **Build for production**: `npm run build` - Compiles TypeScript, bundles with Vite, packages with electron-builder
- **Linting**: `npm run lint` - ESLint with TypeScript rules, strict mode enabled

## Key Patterns and Conventions

### IPC Communication
Use the preload-exposed `ipcRenderer` for main-renderer communication:
```typescript
// In renderer (src/App.tsx)
window.ipcRenderer.invoke('some-channel', data)

// Exposed in electron/preload.ts
contextBridge.exposeInMainWorld('ipcRenderer', {
  invoke: ipcRenderer.invoke,
  // ... other methods
})
```

### File Structure
- `src/`: React components and frontend logic
- `electron/`: Main process and preload scripts
- `public/`: Static assets
- `dist/`: Built renderer output
- `dist-electron/`: Built main/preload output

### Build Configuration
- Vite handles bundling with `vite-plugin-electron` for Electron integration
- electron-builder packages for Windows (NSIS), macOS (DMG), Linux (AppImage)
- Output goes to `release/${version}/`

### TypeScript Setup
- Strict mode enabled with unused variable checks
- Separate configs: `tsconfig.json` for app code, `tsconfig.node.json` for build tools
- Includes both `src` and `electron` directories

## Important Notes
- Main process entry is `dist-electron/main.js` (set in package.json "main")
- In dev, renderer loads from Vite dev server; in prod, from built `dist/index.html`
- Preload script compiled to `preload.mjs` and loaded via `path.join(__dirname, 'preload.mjs')`

## References
- [vite.config.ts](vite.config.ts) - Build configuration with Electron plugins
- [electron/main.ts](electron/main.ts) - Window creation and app lifecycle
- [electron/preload.ts](electron/preload.ts) - Secure API exposure</content>
<parameter name="filePath">e:\dev\arch-procurement\.github\copilot-instructions.md