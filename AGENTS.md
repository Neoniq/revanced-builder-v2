# AI Agent Instructions for Custom ReVanced Builder

## Context
This repository is a customized fork of `j-hc/revanced-magisk-module`. The user wishes to maintain custom build configurations (specifically for building requested APKs like YouTube-Morphe and Reddit-Morphe) and custom CI/CD logic (custom Android keystore signing and Telegram uploads).

To minimize maintenance overhead and completely avoid Git merge conflicts when pulling from `upstream/main`, this fork employs a **"Wrapper Architecture"**.

## Wrapper Architecture Principles
**CRITICAL RULE:** Do NOT modify standard upstream files like `build.sh`, `utils.sh`, `config.toml`, or `.github/workflows/build.yml`. These files must remain identical to the upstream repository so `git pull upstream main` resolves without conflicts.

Instead, custom logic is decoupled into two dedicated files:
1. `custom_config.toml`: Contains the user's specific app profiles (e.g., `[YouTube-Morphe]`) set to `build-mode = "apk"`.
2. `.github/workflows/custom_build.yml`: The standalone CI workflow that controls the build process.

### How the Custom Workflow Works
1. Runs the native, untouched builder using the custom config: `./build.sh custom_config.toml`.
2. Intercepts the generated `.apk` files.
3. Decodes the base64 `KEYSTORE` GitHub Secret.
4. Uses `zip -d` to strip the generic test keys from the generated APKs.
5. Uses `zipalign` and `apksigner` to properly re-sign the APKs using the user's secret Keystore.
6. Crafts a Telegram Markdown message and uploads the signed APKs directly to the user's Telegram chat using the Telegram Bot API (`TG_TOKEN` and `TG_CHAT` secrets).

## Instructions for Updating the Implementation
When the user asks you to update this custom implementation based on upstream changes, follow these steps:

1. **Analyze Upstream Changes:** Look at the recent commits to `upstream/main` (specifically changes to their `.github/workflows/build.yml` or `ci.yml`).
2. **Identify Impact:** See if upstream changed Java versions, changed how artifacts are named or placed, or changed how the update checking (`build.sh ... --config-update`) operates.
3. **Adapt the Wrapper:** If upstream changed critical CI structures, apply those underlying architectural changes to `custom_build.yml` while strictly preserving the three custom steps:
   - Using `custom_config.toml` instead of `config.toml`.
   - The "Re-sign APKs" step.
   - The "Report to Telegram" step.
4. **Do Not Touch Core Scripts:** Under no circumstances should you edit `utils.sh` to inject signing logic. That completely defeats the purpose of the wrapper architecture.

By strictly isolating user modifications to `custom_build.yml` and `custom_config.toml`, the primary upstream logic can update indefinitely with zero merge conflicts.
