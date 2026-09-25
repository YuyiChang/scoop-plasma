# scoop-plasma

[Scoop](https://scoop.sh) bucket for [PLASMA](https://github.com/YuyiChang/PLASMA) —
Platform for LSL-based Acquisition of Sensor Metrics and Analytics.

## Install

```powershell
# one-time, if you don't have Scoop yet (no admin needed)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex

scoop bucket add plasma https://github.com/YuyiChang/scoop-plasma
scoop install plasma
```

This installs the [latest PLASMA release](https://github.com/YuyiChang/PLASMA/releases/latest)
(`PLASMA_Windows_x64.zip`) into `~\scoop\apps\plasma`, adds a **PLASMA**
Start Menu shortcut plus a `plasma` command on `PATH` — no Python
environment or `liblsl` install required. x86-64 only.

No "Windows protected your PC" SmartScreen prompt: that warning is keyed
off the Mark-of-the-Web a *browser* stamps on downloaded files, and Scoop
downloads the release itself, so the binary never carries it. (The binary
is still unsigned — this doesn't help on machines with Smart App Control
turned on, or with an AppLocker/WDAC policy that blocks unsigned apps.)

```powershell
scoop update plasma      # update to the latest release
scoop uninstall plasma   # remove
```

### Nightly

```powershell
scoop install plasma/plasma-nightly
```

Built off `dev` every day at 07:00 UTC — expect it to be less stable.
Installs the same `plasma` command / Start Menu shortcut as `plasma`, so
only have one of the two installed at a time. `scoop update plasma-nightly`
re-downloads the latest nightly (at most once a day).

## Run

Launch **PLASMA** from the Start Menu, or run:

```powershell
plasma
```

PLASMA has no GUI window of its own — it's a local web server, so either
path opens a console window running the app and then a browser tab at
<http://127.0.0.1:7860>. Config, gyro-bias calibration and recordings are
written to `%LOCALAPPDATA%\PLASMA`, not the install folder, so they survive
updates and uninstalls.

## Updating the manifest

`bucket/plasma.json` pins a `version` and `hash` to a specific PLASMA
release. `.github/workflows/bump-plasma.yml` polls PLASMA's latest release
daily (or on demand via **Run workflow**) and opens a PR bumping both;
review and merge it by hand.

By hand instead, on Windows with Scoop installed:

```powershell
& "$(scoop prefix scoop)\bin\checkver.ps1" -App plasma -Dir .\bucket -Update
git diff   # review, then commit + push
```

`bucket/plasma-nightly.json` never needs a bump: it tracks the fixed
`nightly` release tag with `"version": "nightly"`, which tells Scoop to
skip the hash check and treat each day's download as new.
