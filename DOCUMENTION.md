# BigSpoofer Technical Documentation

This document provides technical details about the architecture, core logic, and implementation of BigSpoofer.

## 🏗️ Architecture

BigSpoofer is a WPF (Windows Presentation Foundation) application built with .NET 9.0. It follows a single-window architecture with a modular UI based on `Grid` visibility toggling.

### Core Components
- **MainWindow.xaml**: Defines the UI layout, animations, and themes.
- **MainWindow.xaml.cs**: Contains the core logic for button clicks, batch execution, and UI updates.
- **CustomMessageBox.xaml**: A custom-styled popup window for alerts and confirmations.
- **App.xaml**: Application entry point and global resource management.

## ⚙️ Core Logic

### 1. Batch Execution Engine
The application embeds batch scripts as string constants within the code. When an action is triggered:
1. The script content is written to a temporary `.bat` file.
2. The file is executed using `Process.Start` with `Verb = "runas"` to ensure Administrator privileges.
3. The process runs in a hidden window (`CreateNoWindow = true`).
4. The temporary file is deleted after execution.

### 2. Version Control System
- **Source**: The application checks for updates by fetching a raw text file from Pastebin.
- **URL**: `https://pastebin.com/raw/M1FS7Ct6`
- **Logic**:
  - Compares the remote version string with the local `CurrentVersion` constant.
  - If they differ, a `CustomMessageBox` alerts the user, and the application shuts down to force an update.

### 3. Modules

#### A. Spoofing
Targeted Registry Keys Deleted:
- `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSLicensing`
- `HKEY_CURRENT_USER\Software\WinRAR\ArcHistory`
- `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\bam\State\UserSettings`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FeatureUsage`
- `HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\Shell\MuiCache`

#### B. Deep Cleaning
Files and Directories Removed:
- `%Temp%`, `C:\Windows\Temp`, `C:\Windows\Prefetch`
- FiveM Cache Files: `citizen-shader-cache`, `adhesive.dll`, `profiles.dll`
- Browser Cache (Chrome, Edge, Firefox)

#### C. Unlinking
- **Discord**: Renames `discord_rpc` module to break the link.
- **Rockstar**: Deletes `DigitalEntitlements` folder.
- **CitizenFX**: Deletes `%AppData%\CitizenFX` folder.

#### D. Uninstall
- Kills all FiveM-related processes.
- Removes the main FiveM installation directory (`%LocalAppData%\FiveM`).
- Deletes desktop shortcuts.

## 🎨 UI & Theming

The application uses a dynamic resource dictionary system for theming.
- **Themes**: Defined in `ApplyTheme` method.
- **Resources**: Brushes (`BgDarkBrush`, `AccentPrimaryBrush`, etc.) are updated at runtime.
- **Animations**: Uses `DoubleAnimation` and `ThicknessAnimation` for smooth transitions between views.

## 🔒 Security & Requirements

- **Administrator Privileges**: Enforced via `app.manifest` (`<requestedExecutionLevel level="requireAdministrator" />`).
- **Network Access**: Required for version checking and fetching the latest updates.

## 📦 Dependencies

- **.NET 9.0 Runtime**: Required to run the application.
- **System.Text.Json**: Used for saving/loading user settings.
