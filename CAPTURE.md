# VolleyInsight manual capture notes

## Verified build

- Repository target: `VolleyInsight`
- Scheme: `VolleyInsight`
- Bundle identifier: `PalStudio.VolleyInsight`
- App version/build: `0.1 (1)`
- Deployment target: iOS/iPadOS 17.0
- Xcode: 26.6 (17F113)
- Simulator: iPad Pro 13-inch (M5), iPadOS 26.5
- Capture date: August 27, 2026
- Appearance: dark
- Data: isolated in-memory synthetic dataset only

The app target, simulator output, and current Swift source were treated as the source of truth. No mockups or generated UI images were used.

## Build and simulator commands

The reproducible build command was:

```sh
xcodebuild \
  -project VolleyInsight.xcodeproj \
  -scheme VolleyInsight \
  -configuration Debug \
  -destination 'platform=iOS Simulator,name=iPad Pro 13-inch (M5),OS=26.5' \
  -derivedDataPath /tmp/VolleyInsightManualDerivedData \
  CODE_SIGNING_ALLOWED=NO \
  COMPILER_INDEX_STORE_ENABLE=NO \
  build
```

The app was installed and launched with:

```sh
xcrun simctl install \
  booted \
  /tmp/VolleyInsightManualDerivedData/Build/Products/Debug-iphonesimulator/VolleyInsight.app

xcrun simctl launch \
  booted \
  PalStudio.VolleyInsight \
  -DocumentationDemoMode
```

To reset the documentation dataset, run `xcrun simctl terminate booted PalStudio.VolleyInsight` and repeat the launch command above. The isolated in-memory session is rebuilt and no persistent user records are touched.

Each PNG was captured with the corresponding filename from the manifest:

```sh
xcrun simctl io \
  booted \
  screenshot docs/VolleyInsightUserManual/screenshots/01-main-menu-ipad.png
```

Landscape framebuffers were losslessly rotated after capture with `sips` when Simulator orientation metadata did not match the stored pixel orientation. The Settings image was cropped to the current production competition, paperwork, and backup controls so no development-only control appears in the public package.

## Documentation-only launch hooks

A `#if DEBUG` launch-argument handler in `VolleyInsight/VolleyInsightApp.swift` creates the app's existing `DemoAppSessionFactory` session and optionally selects deterministic documentation scenes. It is excluded from Release builds and changes no production behavior or user data.

Used arguments:

- `-DocumentationDemoMode`
- `-DocumentationScene practice-live`
- `-DocumentationScene tournament-list`
- `-DocumentationScene tournament-detail`
- `-DocumentationScene referee-scoring`
- `-DocumentationScene learning-center`
- `-DocumentationScene contextual-help`
- `-DocumentationScene settings`

Other screens were reached through the visible production UI using Simulator accessibility actions.

## Published captures

The final manifest contains 20 screenshots:

- Main Menu and isolated documentation dataset
- Manage Teams, roster, player profile, and player development history
- Local Collection, Add Data Card, and Collaborative Data Collection
- Rotation Manager R1, R3 serve receive, and Substitution Pairs
- Practice plan and live-practice controls
- Tournament list and tournament detail
- Referee Tool live state with explicit starting server
- Learn VolleyInsight and a live tutorial spotlight
- Public-safe production Settings controls

## Verification results

- Debug simulator build: succeeded with the capture configuration above.
- Release simulator build: succeeded for `generic/platform=iOS Simulator`; the documentation launch handler remains excluded by `#if DEBUG`.
- Static package audit: 20 HTML figures, 20 manifest records, and 20 PNG files match exactly; every image has non-empty alternative text and lazy loading; all 16 internal anchors resolve; no external or absolute runtime URL is present.
- Browser audit: all 20 lazy images decoded at full dimensions; widths of 390, 820, 1024, and 1440 CSS pixels had no horizontal overflow; the table of contents changed from single-column to sticky two-column layout as intended; internal navigation resolved; no browser warning or error was logged.
- Full Xcode test target: 1,078 tests executed, with 3 failures. An isolated rerun passed `BackupRestoreTests.testBackupExcludesTransportSecretsPeerIDsAndGeneratedArtifacts`. `RefRemoteScoreTests.testPairingQRCodeIsConcreteSquareAndContainsDarkAndLightModules` remained blocked by an iOS Simulator Vision error (`Could not create inference context`). `RotationIntelligenceTests.testDirectLiberoSwitchUsesNoRegularSubAndRequiresAnInterveningRally` consistently failed its assertion at line 229. Neither remaining failure exercises the documentation-only launch handler or the static manual package.

## App-versus-draft discrepancies

- The draft described a public “Explore Demo” entry point. The current production UI does not expose one. First launch offers `Create My Team`, `Quick Tour`, and `Skip`; later guidance opens from `Learn`. The manual now identifies the DEMO banner as documentation-only synthetic data.
- The current Player Manager is player-profile based. Development history, progress reports, Player Rankings, and Competitive Lineup Advisor live under player profiles rather than a generic development dashboard.
- The primary practice label is `Practice & Drills`, and its menu includes Practice Planner, Run Practice, time budget, countdown timer, scoreboard, training plans, history, and libraries.
- The learning page is titled `Learn VolleyInsight` and currently contains seven replayable tutorials plus discovery tips.
- Settings currently contains Learning Center access, Tournament & Competition defaults, Paperwork Standards, and Backup & Restore. Backup is user-initiated; the app does not claim automatic cloud backup.
- “Compatible” paperwork templates are not presented as official publications of a governing organization. App output is not itself the legally official match record.

## Rejected or omitted captures

Four draft IDs were intentionally omitted from the published HTML and manifest image list:

- `rankings`: the feature and its “decision-support score—not an official player rating” language were verified, but a clean deterministic deep-link screenshot did not render reliably.
- `tournament-new`: the adaptive form and fields were verified in source and the running UI, but no clean public capture was retained.
- `referee-start`: a rejected capture showed a stale-rotation blocking alert. The published Referee Tool image already shows the explicit starting-server selector in a clean live state.
- `paperwork`: preview availability depends on installed compatible template packages. No generic document was presented as an official organization form.

Two rejected generated PNGs (`10-player-rankings.png` and `19-referee-start-set.png`) were removed. They contained no user data.

## Observed product issues

- The DEMO banner can overlap or clip navigation-bar actions on iPad. This is visible at the top edge of some synthetic-data captures.
- The seeded Dynamic Stretch block reports `Stretch Sequence Unavailable` even though the live timer and courtside controls remain usable.
- Starting a team-mode set correctly blocks when the selected rotation is stale, but the documentation dataset reached this state immediately and reported a reserve-Libero validation error.
