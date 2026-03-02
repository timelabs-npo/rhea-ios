# Rhea iOS — Claude Context

## What this repo is

iOS SwiftUI application for the Rhea multi-model advisory system. Includes:
- **RheaPreview** (`Sources/RheaPreview/`): main app target, SwiftUI entry point
- **RheaKeyboard** (`Sources/RheaKeyboard/`): custom keyboard extension with AI-assisted input via tribunal API
- **RheaKit** (`Packages/RheaKit/`): shared Swift package — all reusable views, API client, auth manager, data models

## Build system

- `project.yml` — xcodegen spec for the Xcode project
- Run `xcodegen generate` to regenerate `RheaApp.xcodeproj`
- `xcodebuild -scheme RheaApp ...` to build/archive
- iOS 17.0 minimum, Swift 5.9, Xcode 16+
- Bundle ID: `com.rhea.preview`, Team: `398XACWZ7G`

## Key files

| File | Role |
|------|------|
| `Sources/RheaPreview/RheaPreviewApp.swift` | App entry point |
| `Sources/RheaPreview/CommandCentreApp.swift` | Main tab container |
| `Sources/RheaPreview/CommandCentreLayout.swift` | Layout logic |
| `Sources/RheaKeyboard/KeyboardViewController.swift` | Keyboard extension root |
| `Sources/RheaKeyboard/KeyboardView.swift` | Keyboard SwiftUI view |
| `Sources/RheaKeyboard/TribunalClient.swift` | Keyboard-to-API bridge (reads JWT from shared Keychain) |
| `Packages/RheaKit/Sources/RheaKit/AuthView.swift` | Auth UI + AuthManager (Keychain JWT) |
| `Packages/RheaKit/Sources/RheaKit/RheaAPI.swift` | Central API client |
| `Packages/RheaKit/Sources/RheaKit/AppConfig.swift` | Server URL config |
| `Packages/RheaKit/Sources/RheaKit/RheaStore.swift` | Shared state store |

## API backend

- Production: `https://rhea-tribunal.fly.dev` (Fly.io, Amsterdam)
- Auth: POST `/auth/login` or `/auth/signup` → JWT bearer token
- All endpoints require `Authorization: Bearer <token>`
- 54+ endpoints covering tribunal, tasks, agents, proofs, radio, aletheia

## Auth flow

1. `AuthManager.shared` holds JWT in memory + Keychain (`jwt_token` key)
2. `AuthView` handles login/signup/Sign-in-with-Apple
3. Keyboard extension reads the same Keychain group to make authenticated requests
4. `rhea://oauth?token=...&email=...` URL scheme handles OAuth callbacks

## RheaKit package dependencies

- `GRDB.swift` — SQLite for local caching
- `KeychainAccess` — secure token storage
- `Starscream` — WebSocket connections
- `swift-markdown-ui` — rendered markdown in chat views
- `Pow` — animation effects

## What NOT to do

- Never hardcode API keys or secrets — all credentials go through Keychain
- Never commit `.env` files or plist files containing team secrets
- The `Resources/3Dmol-min.js` file is a vendored JS library for 3D molecule rendering — do not minify further
- Do not modify `DEVELOPMENT_TEAM` in project.yml without a corresponding cert/profile update

## TestFlight

https://testflight.apple.com/join/BNya22Jg

Build script (from monorepo): `bash scripts/testflight.sh` — bumps version, runs xcodegen, archives, exports IPA.
