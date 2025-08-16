# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a customized fork of Visual Studio Code (VS Code) aimed at building a note-taking application called "Toucan" with custom branding and icons.

### Current Customizations

- **Custom Icon**: `resources/darwin/toucan-note.icns` - custom app icon for macOS
- **Product Overrides**: `build/product.overrides.json` - contains branding customizations:
  - App name: "Toucan"
  - Bundle identifier: `com.dwang28.toucan-note`
  - Custom data folders and protocol handlers
  - Open VSX marketplace configuration

## Build Commands

### Initial Setup
```bash
# Install dependencies
npm install

# Download built-in extensions
npm run download-builtin-extensions
```

### Development
```bash
# Watch mode - compiles TypeScript and watches for changes
npm run watch

# Run the development version
./scripts/code.sh        # macOS/Linux
.\scripts\code.bat       # Windows

# Run web version
./scripts/code-web.sh    # macOS/Linux
.\scripts\code-web.bat   # Windows
```


### Build & Compile
```bash
# Full compile
npm run compile

# Compile specific components
npm run compile-build           # Build system
npm run compile-extensions-build # Extensions
npm run compile-web             # Web version
npm run compile-cli             # CLI

# Production build
npm run gulp vscode-darwin-x64  # macOS Intel
npm run gulp vscode-darwin-arm64 # macOS Apple Silicon
npm run gulp vscode-linux-x64   # Linux
npm run gulp vscode-win32-x64   # Windows
```

### Testing
```bash
# Run tests
npm test                # Shows available test scripts
npm run test-node       # Node unit tests
npm run test-browser    # Browser tests
./scripts/test.sh       # All unit tests

# Code quality checks
npm run eslint          # ESLint checks
npm run stylelint       # Style checks
npm run hygiene         # Code hygiene
npm run monaco-compile-check  # Monaco editor type checking
npm run valid-layers-check    # Architecture layer validation
```

### Linting & Type Checking
```bash
npm run eslint
npm run compile-check-ts-native
npm run vscode-dts-compile-check
```

## Architecture Overview

### Core Structure
- `src/vs/base/` - Foundation utilities and cross-platform abstractions
- `src/vs/platform/` - Platform services and dependency injection
- `src/vs/editor/` - Monaco editor implementation
- `src/vs/workbench/` - Main application UI and features
  - `browser/` - Web components
  - `services/` - Core services
  - `contrib/` - Feature contributions (git, terminal, search, etc.)
  - `api/` - Extension API implementation
- `src/vs/code/` - Electron main process
- `src/vs/server/` - Server implementation

### Build System
- `build/` - Build scripts and configuration
- `build/gulpfile.*.js` - Gulp build tasks
- `build/lib/` - Build utilities
- `scripts/` - Development and build scripts

### Extensions
- `extensions/` - Built-in extensions
- Each extension has standard structure: `package.json`, source code, and contributions

## Customization Guide

### To Change App Name/Icon

1. **Update Product Configuration** (`build/product.overrides.json`):
   - `nameShort`, `nameLong` - Display names
   - `applicationName` - Application identifier
   - `darwinBundleIdentifier` - macOS bundle ID
   - `darwinIcon` - Icon filename (without path)

2. **Add Icon Files**:
   - macOS: Place `.icns` file in `resources/darwin/`
   - Windows: Place `.ico` file in `resources/win32/`
   - Linux: Place `.png` files in `resources/linux/`

3. **Rebuild**: Run the appropriate build command for your platform

### To Customize as Note-Taking App

Key areas to modify:
- `src/vs/workbench/contrib/` - Add note-taking features as contributions
- `src/vs/workbench/browser/parts/` - Modify UI layout for note-taking
- `extensions/markdown-language-features/` - Enhance Markdown support
- `src/vs/editor/` - Customize editor behavior for notes

## Development Tips

### Code Style
- **Indentation**: Use tabs, not spaces
- **Naming**: PascalCase for types/enums, camelCase for functions/variables
- **Strings**: Double quotes for user-visible/localized strings, single quotes otherwise
- **Comments**: Use JSDoc style for functions, interfaces, enums, classes

### Best Practices
- Run `npm run watch` in one terminal, keep VS Code running from `./scripts/code.sh` in another
- Use "Developer: Reload Window" command to apply changes
- Check `src/vs/*/test/` folders for test examples
- Follow the layered architecture: base → platform → editor → workbench
- Use dependency injection for services
- Ensure all user-facing strings are localizable

### Debugging
- Use VS Code's built-in debugging with launch configurations
- Enable Developer Tools: Help → Toggle Developer Tools
- Check console for errors and logs
- Use `--verbose` flag when running scripts for detailed output

## Common Tasks

### Adding a New Feature
1. Create contribution in `src/vs/workbench/contrib/yourfeature/`
2. Register in `src/vs/workbench/contrib/contributions.ts`
3. Add tests in `src/vs/workbench/contrib/yourfeature/test/`
4. Update localization strings if needed

### Modifying Build Process
1. Check relevant `build/gulpfile.*.js` file
2. Update `build/lib/` utilities if needed
3. Test with `npm run gulp <task-name>`

### Creating Custom Distribution
1. Update `build/product.overrides.json` with your branding
2. Place custom resources in `resources/` folders
3. Run platform-specific build command
4. Output will be in `out-vscode/` directory

## Troubleshooting

- **Build fails**: Try `npm ci` for clean install
- **Changes not appearing**: Ensure `npm run watch` is running, reload window
- **Extension issues**: Check `extensions/` folder and `npm run download-builtin-extensions`
- **Type errors**: Run `npm run compile` to see full errors
- **Performance issues**: Check Developer Tools Performance tab

## Important Files for Customization

- `build/product.overrides.json` - Product branding configuration
- `product.json` - Base product configuration (don't edit directly)
- `package.json` - Node dependencies and scripts
- `src/vs/workbench/workbench.desktop.main.ts` - Desktop app entry point
- `src/vs/workbench/workbench.web.main.ts` - Web app entry point
- `src/vs/code/electron-main/main.ts` - Electron main process
