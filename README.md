# Rhea iOS

Multi-model advisory system — iOS client with keyboard extension and shared Swift package.

**TestFlight:** https://testflight.apple.com/join/BNya22Jg

## Structure

```
Sources/
  RheaPreview/       — main app entry point and SwiftUI screens
  RheaKeyboard/      — custom keyboard extension (AI-assisted input)
Packages/
  RheaKit/           — shared Swift package (views, API client, auth)
project.yml          — xcodegen project definition
```

## Tabs

| Tab | Purpose |
|-----|---------|
| Tribunal | Submit claims, watch 3-5 models debate |
| Radio | Live agent communication feed |
| Governor | Token budgets, cost tracking |
| Tasks | Persistent task queue |
| Atlas | 3D knowledge graph |
| Pulse | System health |
| Aletheia | Immutable proof browser |
| Settings | Server URL, auth |

## Build Instructions

### Prerequisites

- Xcode 16+ (deployment target: iOS 17.0)
- [xcodegen](https://github.com/yonaskolb/XcodeGen): `brew install xcodegen`
- Apple Developer account with team ID `398XACWZ7G`

### Generate and build

```bash
# Generate the Xcode project
xcodegen generate

# Build for simulator
xcodebuild -scheme RheaApp -destination 'platform=iOS Simulator,name=iPhone 15' build

# Build for device / archive for TestFlight
xcodebuild -scheme RheaApp -configuration Release archive \
  -archivePath build/RheaApp.xcarchive
```

### RheaKit package (standalone)

```bash
cd Packages/RheaKit
swift build
```

## Key Dependencies

| Package | Purpose |
|---------|---------|
| GRDB.swift | Local SQLite persistence |
| KeychainAccess | JWT token storage |
| Starscream | WebSocket for live agent feeds |
| swift-markdown-ui | Rendered markdown in chat |
| AnimatedTabBar | Tab bar animations |

## Architecture

- **Auth**: email/password + Sign in with Apple, JWT stored in Keychain
- **API**: connects to `rhea-tribunal.fly.dev` (gateway, 54+ endpoints)
- **Keyboard**: shares Keychain group with main app, reads JWT, calls tribunal API
- **RheaKit**: shared library used by iOS app and macOS Play app

Part of [TimeLabs NPO](https://github.com/timelabs-npo) open infrastructure.
