# Build pipelines

The GitHub Actions workflows that build and ship my apps. Every app is written and run on one Linux server, but iOS, tvOS and Windows builds need a Mac or a Windows machine — so they happen here, on GitHub's runners.

**There is no app source in this repository.** Each workflow checks out a prepared package from that app's private repository, using a read-only deploy key that only works for that one repository.

```
server  ──  yayinla-ios.sh  ──▶  private repo, `derleme` branch  ──▶  this workflow  ──▶  TestFlight / .ipa / .exe
            (tests, assets,       (one commit, overwritten           (checkout with a
             level checks)         on every release)                  per-repo deploy key)
```

## Pipelines

| App | Workflows | Output |
|---|---|---|
| [teamplanOS](https://teamplanos.com) | `derle-teamplan` · `teamplan-testflight` | unsigned check build · TestFlight |
| [ContacTeam](https://contacteam.com) | `derle-contacteam` · `contacteam-testflight` | unsigned check build · TestFlight |
| [Medowl](https://medowl.app) | `derle-medowl` · `medowl-testflight` | unsigned check build · TestFlight |
| [Little Light](https://littlelightgame.com) | `derle-isik` · `isik-testflight` · `isik-video` | check build with simulator screenshots · TestFlight · gameplay video |
| [At Pansiyon](https://atpansiyon.atasoy.dev) | `derle-aheste` · `aheste-testflight` | unsigned check build · TestFlight |
| [Fotoyol](https://chainmedia.co/fotoyol/) | `derle` · `derle-ios` · `fotoyol-testflight` · `fotoyol-vitrin` | Windows .exe + macOS .app · iOS check · TestFlight · store screenshots |
| SyncYourPods | `syncyourpods` · `syncyourpods-windows` | iOS .ipa · Windows .exe (MSVC, Defender-scanned) |
| Seyir | `seyir-engine-check` · `seyir-personal-check` · `seyir-personal-sign` | tvOS engine probe · native build · signed personal build |

## Why it is set up this way

- **Public, so the macOS minutes are free.** In a private repository, macOS runner minutes are billed at many times the Linux rate.
- **Source stays private.** Workflows only ever see the package branch, through a deploy key scoped to one repository. Signing keys and App Store Connect credentials are Actions secrets.
- **Releases are prepared on the server.** The release script runs the tests, bakes assets and checks every level before anything is pushed, so a workflow run is just compile, sign and upload.

<sub>Part of [atasoy.dev](https://atasoy.dev) · [github.com/pexatasoy](https://github.com/pexatasoy)</sub>
