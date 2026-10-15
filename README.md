# ReVanced Builder v2 — custom APK channel
[![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/rvc_magisk)
[![Custom Build](https://github.com/Neoniq/revanced-builder-v2/actions/workflows/custom_build.yml/badge.svg)](https://github.com/Neoniq/revanced-builder-v2/actions/workflows/custom_build.yml)

Customized fork of the [j-hc/revanced-magisk-module](https://github.com/j-hc/revanced-magisk-module) builder.

The primary branch is `master`. It publishes APKs from [`custom_config.toml`](./custom_config.toml), re-signs them with the repository's private keystore, creates GitHub Releases, and sends Telegram reports. See [`CUSTOM_BUILD.md`](./CUSTOM_BUILD.md) for the release contract.

Get the [latest custom release](../../releases).

Use [**zygisk-detach**](https://github.com/j-hc/zygisk-detach) to detach YouTube and YT Music from Play Store if you are using magisk modules. 

<details><summary><big>Features</big></summary>
<ul>
 <li> Supports all present and future ReVanced apps (including projects implementing the same interface like Morphe)</li>
 <li> Receives in-app updates</li>
 <li> Can build modules and non-root APKs</li>
 <li> Updated daily with the latest versions of apps and patches</li>
 <li> Optimizes APKs and modules for size</li>
 <li> Modules</li>
    <ul>
     <li> handle installation of the correct version of the stock app and all that</li>
     <li> do not trigger root detections</li>
     <li> receive updates from Magisk/KernelSU app</li>
     <li> recompile invalidated odex for faster usage</li>
    </ul>
</ul>
</details>

## Custom build channel

 * Edit [`custom_config.toml`](./custom_config.toml) for enabled APK profiles.
 * Run the [Custom Build Wrapper](../../actions/workflows/custom_build.yml) manually or wait for its daily schedule.
 * Grab signed APKs from [releases](../../releases).

also see here [`CONFIG.md`](./CONFIG.md)

## If you are having trouble with the classic mount method of the modules
such as,
- **"Reflash needed"** error after reboots
- **"Suspicious mount detected"** warnings from root detector apps

You can consider using [rvmm-zygisk-mount](https://github.com/j-hc/rvmm-zygisk-mount)

## Building Locally
### On Termux
```console
bash <(curl -sSf https://raw.githubusercontent.com/Neoniq/revanced-builder-v2/master/build-termux.sh)
```

### On Linux
```console
$ git clone https://github.com/Neoniq/revanced-builder-v2 --depth 1
$ cd revanced-builder-v2
$ ./build.sh custom_config.toml
```
