# Duo Max baseline: issue #1

This fork currently has `versionName = "0.0.36"`, `versionCode = 36`, `targetSdk = 37` in `app/build.gradle.kts`. **It is not yet pinned to upstream 0.0.31.** The owner confirmed that an upstream 0.0.31 APK installs through Yandex's app installation flow, whereas upstream 0.0.36 is rejected. The root cause is unknown.

## CI build
`.github/workflows/duomax-baseline.yml` builds a debug APK on pull requests and via manual workflow dispatch. It does not need repository signing secrets, does not publish releases, and leaves the upstream release workflow unchanged. Download the `duomax-debug-apk` artifact from the run. It includes the APK, SHA256SUMS, source commit, submodule SHAs and build parameters. The debug APK is signed with the CI runner's ephemeral Android debug key, **not** the upstream app signing key; installing over an existing upstream package may fail with a signature mismatch. Do not uninstall the known-working app until data and rollback are accounted for.

## Next steps before claiming 0.0.31 compatibility
1. Identify the exact upstream 0.0.31 source tag/commit and pin all native submodule revisions; do not change only `versionName`.
2. Obtain the original working 0.0.31 APK for comparison. Compare `aapt dump badging`, manifest permissions/features, SDK levels, ABIs, signing certificate, native libraries and APK size.
3. Build from the exact pinned source and validate its output. Record any required changes for YaOS X.
4. Owner tests installation and playback on Duo Max; distinguish rejection by Yandex from Android install errors and from runtime failures.
5. Preserve video/mirroring and document a rollback path.

No installation or runtime test on Duo Max has been performed by this CI setup.
