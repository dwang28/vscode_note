# Toucan - Custom VS Code Build

This is a customized build of VS Code with custom branding and icon to create a separate note-taking application.

## Build Status

✅ **Build Successful!** The app has been built with:
- Custom icon: `toucan-note.icns` 
- Custom name: "Toucan"
- Bundle ID: `com.dwang28.toucan-note`

## Quick Start

### Run the Built Application

```bash
# Run the built macOS app
open "../VSCode-darwin-arm64/Toucan.app"

# Or run it from the current directory
open ../VSCode-darwin-arm64/Toucan.app
```

### Install as System Application

To install Toucan so it appears in Launchpad and behaves like any other macOS app:

```bash
# Copy the app to your Applications folder
cp -R "../VSCode-darwin-arm64/Toucan.app" "/Applications/"

# Clear icon cache to ensure proper display
sudo rm -rf /Library/Caches/com.apple.iconservices.store
killall Dock
```

After installation:
- ✅ **Launchpad**: App will appear in Launchpad
- ✅ **Spotlight**: You can search "Toucan" in Spotlight
- ✅ **Dock**: You can pin it to your dock
- ✅ **System Integration**: Behaves like any native macOS app

### Development Mode

```bash
# Run in development mode (loads product.overrides.json)
./scripts/code.sh
```

## Building from Source

### Prerequisites

```bash
# Install dependencies
npm install

# Download built-in extensions
npm run download-builtin-extensions
```

### Compile and Build

```bash
# Step 1: Compile TypeScript
npm run compile

# Step 2: Build for macOS ARM64 (Apple Silicon)
npm run gulp vscode-darwin-arm64

# Or for Intel Macs
npm run gulp vscode-darwin-x64
```

Build time: ~5 minutes on Apple Silicon

### Output Location

The built application will be located at:
```
../VSCode-darwin-arm64/Toucan.app
```

## Customization Files

- **Product Overrides**: `build/product.overrides.json` - Contains app name, bundle ID, and other branding
- **Icon**: `resources/darwin/toucan-note.icns` - Custom application icon
- **Build Config**: `build/lib/electron.ts` - Modified to use custom icon

## Features

- ✅ Custom icon prevents macOS from grouping with regular VS Code
- ✅ Separate data folder (`.toucan-note`)
- ✅ Custom protocol handler (`toucan-note://`)
- ✅ Open VSX marketplace integration

## Troubleshooting

### If the app doesn't show custom name in Finder

The Finder name is baked into the build. To fully customize:
1. Right-click the app → "Show Package Contents"
2. Edit `Contents/Info.plist`
3. Change `CFBundleDisplayName` to "Toucan"

### To verify the custom icon was applied

```bash
# Check icon file size (should match)
ls -la "../VSCode-darwin-arm64/Toucan.app/Contents/Resources/Toucan.icns"
ls -la "resources/darwin/toucan-note.icns"
```

Both should show: 372,578 bytes

## Next Steps

To further customize as a note-taking app:
1. Modify the welcome screen in `src/vs/workbench/contrib/welcome/`
2. Add note-specific features in `src/vs/workbench/contrib/`
3. Customize the file explorer for note organization
4. Add markdown-focused features

## Development Commands Reference

```bash
# Watch mode for development
npm run watch

# Run tests
npm test

# Lint code
npm run eslint

# Clean build
rm -rf out-build out-vscode out-vscode-darwin-arm64
```

## License

MIT (inherited from VS Code)