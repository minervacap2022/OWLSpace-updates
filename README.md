# OWLSpace-updates

Public appcast + DMG hosting for the [OWLSpace](https://github.com/minervacap2022/OWLSpace) macOS app.

Sparkle (the macOS auto-update framework) bundled inside the OWLSpace app reads
`appcast.xml` from this repo's `main` branch over HTTPS:

    https://raw.githubusercontent.com/minervacap2022/OWLSpace-updates/main/appcast.xml

Each release uploads the signed `.dmg` into `releases/` here so Sparkle can
download it from a stable raw-content URL.

The source repo (`minervacap2022/OWLSpace`) is private; this one is public
only so that Sparkle can fetch the appcast without GitHub auth.

## Release flow

`scripts/release.sh` in the main repo runs `make-appcast.sh` after the DMG is
built. That script:

1. Computes the DMG's length + EdDSA signature (via Sparkle's `sign_update`,
   using the private key stored in the developer's macOS Keychain under
   `https://sparkle-project.org`).
2. Inserts/updates an `<item>` in `appcast.xml`.
3. Commits + pushes the DMG to `releases/` and the updated `appcast.xml` to
   `main`.

## Manual inspection

    curl -s https://raw.githubusercontent.com/minervacap2022/OWLSpace-updates/main/appcast.xml
