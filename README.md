# Glass TV Companion

Create a quiet, personal Home screen for an Android TV or Google TV from your Windows PC.

## Start here

1. Download the **GlassTVCompanion** ZIP and extract the whole folder. Keep its `adb` folder beside the program.
2. On the TV, enable Developer options (usually Settings → About → press Build seven times), then USB/network debugging. Menu names vary by TV.
3. Put the PC and TV on the same local network. Open **GlassTVCompanion.exe**. You do not need Python, Java or Android Studio.
4. Choose your local subnet and click **Find TVs**, or enter the TV's IP address and debugging port. Click **Connect** and accept the debugging prompt on your TV. If required, click Connect again.
5. Customise the heading, colours and six tiles. Choose four installed TV apps and up to two detected input IDs. Labels and captions are editable. Save the design to reuse it.
6. Click **Install & preview**. This opens Glass TV and leaves your current programme, but does not change the Home default.
7. Test the remote navigation, each app tile and each HDMI shortcut on the TV. Tick the confirmation only after they work. Then choose **Make default**.

**Preview is not automatic approval.** The tool checks installation, profile acceptance, the foreground activity and Home resolution. It cannot verify what your TV displays, sound, console power or remote responsiveness for you. HDMI tiles switch inputs; they do not automatically power on a console.

## If the stock Home keeps winning

Some firmware, including the tested TCL, gives its system launcher higher priority. The first attempt keeps the original launcher enabled and reports this limitation. You can explicitly tick **Allow disabling the saved stock launcher** and confirm a second attempt. That disables the original launcher for the connected Android user; it does not uninstall it.

If you choose this workaround, keep the PC's authorised ADB connection available. The TV's Home chooser cannot re-enable a disabled stock app by itself. Activation failures trigger an automatic restore attempt; a disconnected TV can prevent recovery until it reconnects.

## Go back to the original Home screen

Open the companion, connect the same TV and click **Restore stock**. It re-enables the stock launcher if this tool disabled it, selects the saved Home component and checks the Home button. Glass TV remains installed.

Use **Setup & recovery → Open recovery folder** to back up your records. They live in `%LOCALAPPDATA%\GlassTVCompanion\devices`. The identity check is local and prevents accidentally restoring another TV just because an IP address was reused. Do not delete these records while stock Home is disabled. Your design JSON is separate from the recovery record.

If Glass TV was made Home using a different PC/tool and this companion has no record, first restore stock Home using the original tool. Connect with this companion before activating it again. Do not uninstall Glass TV while it is your only enabled Home app.

## Supported customisation

- Heading and subtitle.
- Background, focus highlight, Xbox card and PS5 mark colours.
- Four app packages with editable names/captions; installed app icons load on the TV.
- Two input IDs with editable names/captions. Input IDs are device-specific and are validated against the connected TV.
- JSON import/export and a simple PC layout illustration. The TV preview is the authoritative appearance.

The current template has four app slots and two console slots. It does not yet support arbitrary grids, custom uploaded artwork or wallpaper. Reduced motion can be selected on the TV. The app needs Android 8 or later; only Android/Google TV is supported, not Samsung Tizen or LG webOS. A TV that requires wireless-debugging pairing must be paired separately first. No root, firmware changes or reboot is required. Reboot persistence depends on firmware and is not automatically tested.

The companion does not send telemetry or device IDs to external services. It needs LAN ADB access. Profiles stay local; the launcher itself has no Internet or microphone permission. Its profile import receiver accepts only callers with Android's privileged DUMP permission, including the authorised ADB shell.

## Developers

Source: `glass_companion.py` (Tk UI), `glass_companion_core.py` (validated profiles and device lifecycle), `launcher/` (native Android app). LAN discovery is reused from `run_toolkit.py`; importing it does not launch the package manager.

```powershell
python -m unittest discover -s tests
python launcher/build.py
powershell -ExecutionPolicy Bypass -File build_companion_release.ps1 -Python .local-tools/build-env/Scripts/python.exe
```

The build machine needs the Android SDK/JDK described in `launcher/README.md`, plus `build-requirements.txt`. End users only need the extracted release. The APK is embedded in the EXE. The Android build uses a local development signing key; preserve it across updates and establish a maintained release signing identity before wider distribution. Neither the signing key nor recovery records are packaged.

The EXE is unsigned. Release artifacts are not automatically uploaded to GitHub. `--diagnostics OUTPUT.json` performs an offline packaged-runtime check without connecting to a TV.
