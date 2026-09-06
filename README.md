# Rhea iOS

**Take the question out of the control room.**

Rhea iOS brings the agent conversation, task queue, research views, and operating signals to a SwiftUI mobile interface. It includes main-app sources, keyboard-extension sources, and a local copy of the shared RheaKit package.

A question should be allowed to follow you outside. So should the ability to inspect the answer: which work is still open, which agents are speaking, what the request is costing. The phone is another place to ask and observe.

**Current form:** an extracted client source tree. The Xcode project specifications still contain unresolved paths from the original workspace, so this checkout needs integration repair before it can offer a reliable standalone app build.

## What the phone opens

The current [app shell](Sources/RheaPreview/RheaPreviewApp.swift) has eight destinations:

| Destination | Purpose |
|---|---|
| Ops | Operational overview |
| Tribunal | Questions and model responses |
| Bio | Biological/molecular visualization |
| Radio | Agent communication |
| Tasks | Task queue |
| Governor | Budget and service views |
| Tools | Additional tools and inputs |
| Config | Settings and connection |

Requests go to the configured Rhea services. A mobile view does not acquire execution authority merely by sharing a backend with a desktop client; the receiving service's behavior and permissions still determine the result.

## Find your way through the source

| Path | What lives here |
|---|---|
| [Sources/RheaPreview](Sources/RheaPreview) | App entry and SwiftUI screens |
| [Sources/RheaKeyboard](Sources/RheaKeyboard) | Keyboard controller, UI, and its own API client |
| [packages/RheaKit](packages/RheaKit) | Shared views, state, API, and authentication code |
| [project.yml](project.yml) | Root XcodeGen specification |
| [RheaApp/project.yml](RheaApp/project.yml) | Earlier nested XcodeGen specification |

The directory is lower-case `packages`. The keyboard client is separate from RheaKit; shared authentication requires an explicit containing-app/extension setup.

[RheaKit's AppConfig](packages/RheaKit/Sources/RheaKit/AppConfig.swift) uses localhost for the simulator and the configured cloud address elsewhere. Its startup migration replaces saved local/private-network addresses on non-simulator launches. Successful service access and live data must be established for the selected environment.

## Start with the package; account for the extraction

With a Swift toolchain installed, inspect the bundled package without claiming an app build:

```bash
swift package --package-path packages/RheaKit dump-package
```

The declared application target is `RheaApp`, with iOS 17 as its deployment target. However, both project specifications refer to old locations including `../../packages/RheaKit`, `../RheaPreview.swiftpm/Sources`, and sibling keyboard/tunnel directories. The referenced tunnel source and entitlement files are absent from this clone.

A standalone build therefore needs the source/package paths reconciled, the missing extension inputs supplied or its targets deliberately revised, and appropriate signing/capability configuration. Running `xcodegen generate` against the current files does not resolve those missing inputs.

The ambition remains a portable instrument: a request can leave the desk without leaving its context behind. The next engineering step is to make this extracted application reproducibly buildable, then verify its service and extension paths on the intended devices.

## The surrounding system

Start at [the Rhea family entrance](https://blueshoes.space/rhea/).

- [Rhea / Tribunal](https://github.com/timelabs-npo/rhea-project) contains the coordination and backend work.
- [Rhea Play](https://github.com/timelabs-npo/rhea-play) is the related macOS operations application.
- [RheaKeyboard](https://github.com/timelabs-npo/rhea-keyboard) maintains standalone keyboard-package sources.
- [Rhea Atlas](https://github.com/timelabs-npo/rhea-atlas) develops the web interface to the system.

MIT — see [LICENSE](LICENSE).
