# Program Manager Tracker — Distribution & Update System Guide

This guide explains how to package the application installer, share it with users, and publish new updates to GitHub so that existing users receive automated update notifications.

---

## 1. How the Installer Works (Step 1)

Whenever you build the application, an installer named **`ProgramManagerTracker_Setup.exe`** is created.

### Key Benefits of the Installer:
- **No Administrator Rights Required**: Installs into `%LOCALAPPDATA%\Programs\Program Manager Tracker\`. Users can install and run it without IT passwords or Windows UAC prompts.
- **Desktop & Start Menu Shortcuts**: Automatically adds a Start Menu shortcut and an optional Desktop shortcut with the custom high-resolution icon.
- **Windows Integration & Uninstaller**: Registers in Windows *Settings > Apps > Installed apps* for clean, one-click uninstallation.
- **Preserves User Data**: Updating or reinstalling will **never** delete user configs, session tokens, or cached data.
- **Single File Distribution**: You only need to send users this single file or download link.

### Building the Installer:
You can build it in two ways:
1. **Automatic (recommended)**: Double-click **`build_exe.bat`**. It compiles the Python executable AND then automatically compiles the Inno Setup installer.
2. **Standalone Installer**: Double-click **`build_installer.bat`**. It compiles the installer from the existing `Program Manager Tracker.exe`.

---

## 2. The Permanent Shareable Link

When you attach `ProgramManagerTracker_Setup.exe` to a GitHub Release, GitHub provides a **permanent direct download link**:

```
https://github.com/SalimLTawk/CRM/releases/latest/download/ProgramManagerTracker_Setup.exe
```

> [!TIP]
> **Share this exact link with anyone!** 
> Whenever someone clicks this link, GitHub automatically redirects them to download the newest installer from your latest release. You never have to send a new link when you push updates.

---

## 3. How to Release an Update (Step 2)

When you make changes to the app and want users to receive the update notification, follow these simple steps:

### Step 1: Bump the Version
Open [`pm_version.py`](file:///c:/Users/Expert-Zone/Lead%20Tracker/pm_version.py) and increment the version string:
```python
APP_VERSION = "1.0.1"  # Change from 1.0.0 to 1.0.1 (or 1.1.0, etc.)
```

### Step 2: Build the New Executable & Installer
Run:
```bat
build_exe.bat
```
This produces the new `ProgramManagerTracker_Setup.exe` (with version `1.0.1` embedded).

### Step 3: Push Code to GitHub
In your terminal / Git:
```bash
git add .
git commit -m "Release v1.0.1: [describe your changes]"
git push origin main
```

### Step 4: Publish the GitHub Release
1. Open your GitHub repository in your browser:
   [https://github.com/SalimLTawk/CRM/releases](https://github.com/SalimLTawk/CRM/releases)
2. Click **"Draft a new release"** (or [click here directly](https://github.com/SalimLTawk/CRM/releases/new)).
3. Fill in the release details:
   - **Choose a tag**: Type `v1.0.1` (matching your `APP_VERSION`) and click *Create new tag*.
   - **Release title**: E.g., `Version 1.0.1 - New Features & Bug Fixes`
   - **Description**: Add bullet points of what's new (these release notes will display directly inside the app's update window!).
4. **Attach the Installer**:
   - Drag and drop **`ProgramManagerTracker_Setup.exe`** into the *"Attach binaries by dropping them here or selecting them"* box.
5. Click **"Publish release"**.

---

## 4. What the Users See

As soon as the release is published on GitHub:
1. **Background Detection**:
   When users launch their desktop app, it silently checks GitHub in the background.
2. **Topbar Notification**:
   A prominent green **"🚀 Update v1.0.1"** badge appears in the top navigation bar next to Settings.
3. **Interactive Update Window**:
   Clicking the badge opens the update window displaying:
   - Current installed version vs. New version available.
   - The formatted **What's New** changelog you typed on GitHub.
   - A **"Download & Update Now"** button with a real-time progress bar.
4. **One-Click Installation**:
   When the download completes, clicking **"Install & Restart"** automatically launches the installer and restarts the application with the new update applied!
5. **Manual Check**:
   Users can also open **Settings** anytime and click **"Check for Updates"** to verify their version.
