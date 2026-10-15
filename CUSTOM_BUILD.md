# Custom build channel

This fork publishes the user's APK channel from `master`.

## What runs

`.github/workflows/custom_build.yml` is the only release workflow. It runs manually or on the daily schedule, checks the `custom-update` state branch, builds `custom_config.toml`, signs every APK with the private GitHub Actions keystore, creates a GitHub Release, and reports the release to Telegram.

The upstream `CI` and `Build Modules` workflows remain in the checkout for upstream compatibility but are disabled in the repository. They must not be used for releases because they build the upstream `config.toml` and use the public template keystore.

## Profiles

- `YouTube-Morphe` — enabled APK profile with in-app update checks.
- `Reddit-Morphe` — enabled APK profile.
- `YouTube` — legacy generic profile, kept disabled.

## State and updates

`custom-update/build.md` records the last successful custom build. The workflow compares that file during `--config-update`; it never reads upstream's `update` branch. A missing state branch intentionally causes the next scheduled run to build.

The final APK certificate must come from the `KEYSTORE`, `KEYSTORE_PASS`, `KEY_ALIAS`, and `KEY_PASS` secrets. The workflow fails before publishing when signing secrets or certificate verification are missing.

`master` is the primary branch. `backup/legacy-custom-build` and the other `backup/*` branches are archival rollback points.
