# OpenROAD GUI - Linux/WSL AppImage Installation Guide

Visit this link to get the build and start with your chip design journey: https://drive.google.com/drive/folders/10rsNmi84GktGHE23wIuOKwfImv1B_dBn?usp=drive_link

## How the AppImage Works

An **AppImage** is a self-contained, portable application format for Linux systems. Unlike traditional software installation, an AppImage bundles the entire application, its dependencies, and runtime libraries into a single executable file. This file can be run directly without requiring installation or root privileges.

### Key Features:
- **Self-contained**: Includes all necessary libraries and dependencies
- **Portable**: Can be run from any location (USB drive, network share, etc.)
- **No installation required**: Just make it executable and run
- **Sandbox-friendly**: Doesn't modify system files
- **Cross-distribution**: Works on most Linux distributions

### How We Packaged GBs of Dependencies into MBs

The OpenROAD GUI AppImage is 441MB but includes over 1.6GB of tools and dependencies. This compression is achieved through:

1. **SquashFS Compression**: AppImage uses the SquashFS filesystem format, which provides excellent compression ratios (typically 2-5x compression for binaries and data)

2. **Selective Bundling**: Only runtime dependencies are included, not development files or unnecessary components

3. **Shared Libraries**: Common system libraries are bundled efficiently

4. **Electron Framework**: The GUI is built with Electron, which bundles Node.js and Chromium efficiently

The build process:
- Copies OpenROAD tools (~1.5GB uncompressed)
- Includes Yosys and utilities (~73MB)
- Bundles Electron runtime and Node.js dependencies
- Compresses everything into a single SquashFS image within the AppImage

## Prerequisites

### For Linux Users:
- A Linux distribution (Ubuntu, Fedora, Debian, etc.)
- Basic system libraries (glibc 2.17+, most modern distros have this)
- No additional software installation required

### For Windows Users:
- Windows 10 version 2004 (Build 19041) or higher, or Windows 11
- Administrator privileges for WSL installation
- At least 4GB RAM recommended
- Virtualization enabled in BIOS (for WSL 2)

## Installation Instructions

### Option 1: Linux Users

#### Step 1: Download the AppImage
Download the `OpenROAD GUI-0.1.0.AppImage` file from the project releases or distribution location.

#### Step 2: Make it Executable
Open a terminal and navigate to the download directory:

```bash
cd ~/Downloads  # or wherever you downloaded the file
chmod +x "OpenROAD GUI-0.1.0.AppImage"
```

#### Step 3: Run the Application
```bash
./"OpenROAD GUI-0.1.0.AppImage"
```

The application will start automatically. You can also double-click the file in your file manager if your desktop environment supports AppImages.

#### Optional: Create a Desktop Shortcut
To make it easier to launch:

```bash
# Create a desktop entry (replace with actual path)
echo "[Desktop Entry]
Name=OpenROAD GUI
Exec=/path/to/OpenROAD GUI-0.1.0.AppImage
Icon=application-x-executable
Type=Application
Categories=Development;" > ~/.local/share/applications/openroad-gui.desktop
```

### Option 2: Windows Users (Using WSL)

#### Step 1: Install WSL (Windows Subsystem for Linux)

**Important**: WSL installation requires administrator privileges and may require a system restart.

1. **Check Prerequisites**:
   - Press `Win + R`, type `winver`, and press Enter
   - Ensure you're running Windows 10 version 2004 (Build 19041) or higher, or Windows 11

2. **Enable Virtualization** (if not already enabled):
   - Restart your computer and enter BIOS/UEFI (usually F2, F10, or Del key)
   - Look for "Virtualization Technology" or "VT-x" and enable it
   - Save changes and exit

3. **Install WSL**:
   - Open PowerShell as Administrator:
     - Press `Win + X` and select "Windows PowerShell (Admin)" or "Terminal (Admin)"
   - Run the installation command:
     ```powershell
     wsl --install
     ```
   - This command will:
     - Enable the necessary Windows features
     - Download and install the Ubuntu Linux distribution
     - Set WSL 2 as the default version
   - **Restart your computer** when prompted

4. **Complete WSL Setup**:
   - After restart, WSL will automatically launch Ubuntu
   - Create a Unix username and password when prompted
   - Remember this password - you'll need it for sudo operations

5. **Verify Installation**:
   - Open PowerShell or Command Prompt
   - Run: `wsl --list --verbose`
   - You should see Ubuntu listed with STATE: Running and VERSION: 2

#### Step 2: Download the AppImage in WSL

1. **Access WSL Terminal**:
   - Press `Win + R`, type `wsl`, and press Enter
   - Or open Windows Terminal and select the Ubuntu tab

2. **Navigate to a suitable directory**:
   ```bash
   cd ~
   mkdir openroad && cd openroad
   ```

3. **Download the AppImage**:
   - If you have the file on Windows, copy it to WSL:
     ```bash
     # From Windows Explorer, copy the file to: \\wsl$\Ubuntu\home\yourusername\openroad\
     ```
   - Or download directly in WSL using wget/curl if available

#### Step 3: Install and Run in WSL

1. **Make the AppImage executable**:
   ```bash
   chmod +x "OpenROAD GUI-0.1.0.AppImage"
   ```

2. **Run the application**:
   ```bash
   ./"OpenROAD GUI-0.1.0.AppImage"
   ```

   **Note**: The GUI will display using X11 forwarding. If you see display errors:
   - Install an X server for Windows (like VcXsrv or MobaXterm)
   - Configure DISPLAY variable: `export DISPLAY=:0` (adjust as needed)

#### Alternative: Run WSL Commands from Windows

You can also run WSL commands directly from Windows PowerShell:

```powershell
# Run the AppImage
wsl.exe -- ./OpenROAD\ GUI-0.1.0.AppImage
```

## Troubleshooting

### Common Issues

#### "Permission denied" when running AppImage
- Ensure you've made it executable: `chmod +x "OpenROAD GUI-0.1.0.AppImage"`

#### "FUSE library not found" error
- Install FUSE: `sudo apt-get install libfuse2` (Ubuntu/Debian)
- Or: `sudo dnf install fuse-libs` (Fedora)

#### GUI doesn't display in WSL
- Install an X server for Windows (VcXsrv recommended)
- Set DISPLAY variable: `export DISPLAY=:0.0`
- Ensure X server is running and allowing connections

#### "Cannot execute binary file" error
- Ensure you're on a compatible architecture (x64)
- Check if the file is corrupted during download

#### WSL Installation Issues
- Ensure virtualization is enabled in BIOS
- Run PowerShell as Administrator
- Try: `wsl --update` after installation
- For older Windows versions, see manual installation guide

### Performance Tips
- WSL 2 provides better performance than WSL 1
- Allocate sufficient RAM to WSL (default is 50% of system RAM)
- Store large files on the Windows filesystem for better performance

### Getting Help
- Check the OpenROAD project documentation
- WSL issues: Microsoft WSL documentation
- AppImage issues: AppImage community forums

## What's Included
- OpenROAD flow tools (complete ASIC design flow)
- Yosys synthesis tool
- Yosys utilities
- Electron-based GUI interface
- All necessary runtime dependencies
- Node.js and Chromium runtime

The AppImage is completely self-contained and doesn't require any additional installations.
     wsl --install
     ```
   - This command will:
     - Enable the necessary Windows features
     - Download and install the Ubuntu Linux distribution
     - Set WSL 2 as the default version
   - **Restart your computer** when prompted

4. **Complete WSL Setup**:
   - After restart, WSL will automatically launch Ubuntu
   - Create a Unix username and password when prompted
   - Remember this password - you'll need it for sudo operations

5. **Verify Installation**:
   - Open PowerShell or Command Prompt
   - Run: `wsl --list --verbose`
   - You should see Ubuntu listed with STATE: Running and VERSION: 2

#### Step 2: Download the AppImage in WSL

1. **Access WSL Terminal**:
   - Press `Win + R`, type `wsl`, and press Enter
   - Or open Windows Terminal and select the Ubuntu tab

2. **Navigate to a suitable directory**:
   ```bash
   cd ~
   mkdir openroad && cd openroad
   ```

3. **Download the AppImage**:
   - If you have the file on Windows, copy it to WSL:
     ```bash
     # From Windows Explorer, copy the file to: \\wsl$\Ubuntu\home\yourusername\openroad\
     ```
   - Or download directly in WSL using wget/curl if available

#### Step 3: Install and Run in WSL

1. **Make the AppImage executable**:
   ```bash
   chmod +x "OpenROAD GUI-0.1.0.AppImage"
   ```

2. **Run the application**:
   ```bash
   ./"OpenROAD GUI-0.1.0.AppImage"
   ```

   **Note**: The GUI will display using X11 forwarding. If you see display errors:
   - Install an X server for Windows (like VcXsrv or MobaXterm)
   - Configure DISPLAY variable: `export DISPLAY=:0` (adjust as needed)

#### Alternative: Run WSL Commands from Windows

You can also run WSL commands directly from Windows PowerShell:

```powershell
# Run the AppImage
wsl.exe -- ./OpenROAD\ GUI-0.1.0.AppImage
```

## Troubleshooting

### Common Issues

#### "Permission denied" when running AppImage
- Ensure you've made it executable: `chmod +x "OpenROAD GUI-0.1.0.AppImage"`

#### "FUSE library not found" error
- Install FUSE: `sudo apt-get install libfuse2` (Ubuntu/Debian)
- Or: `sudo dnf install fuse-libs` (Fedora)

#### GUI doesn't display in WSL
- Install an X server for Windows (VcXsrv recommended)
- Set DISPLAY variable: `export DISPLAY=:0.0`
- Ensure X server is running and allowing connections

#### "Cannot execute binary file" error
- Ensure you're on a compatible architecture (x64)
- Check if the file is corrupted during download

#### WSL Installation Issues
- Ensure virtualization is enabled in BIOS
- Run PowerShell as Administrator
- Try: `wsl --update` after installation
- For older Windows versions, see manual installation guide

### Performance Tips
- WSL 2 provides better performance than WSL 1
- Allocate sufficient RAM to WSL (default is 50% of system RAM)
- Store large files on the Windows filesystem for better performance

### Getting Help
- Check the OpenROAD project documentation
- WSL issues: Microsoft WSL documentation
- AppImage issues: AppImage community forums

## What's Included
- OpenROAD flow tools (complete ASIC design flow)
- Yosys synthesis tool
- Yosys utilities
- Electron-based GUI interface
- All necessary runtime dependencies
- Node.js and Chromium runtime

The AppImage is completely self-contained and doesn't require any additional installations.

```javascript
const LicenseManager = require('./packaging/license-manager.js');
const manager = new LicenseManager();

// Generate hardware fingerprint
const hwId = manager.getHardwareFingerprint();

// Generate test license
const { licenseKey } = manager.generateTestLicense();

// Activate license
const result = manager.activateLicense(licenseKey);

// Check license status
const info = manager.getLicenseInfo();
```

**License Storage**: `~/.openroad-gui/license.json` on all platforms

**Features**:
- Hardware-based locking (prevents license sharing)
- Offline validation (works without internet)
- Test mode for development

### Tool Wrapper (`tool-wrapper.js`)

Handles cross-platform tool execution:

```javascript
const ToolWrapper = require('./packaging/tool-wrapper.js');
const wrapper = new ToolWrapper(appResourcesPath);

// Execute tool automatically handles platform differences
await wrapper.executeTool('openroad', ['-v']);

// Run make (used by OpenROAD flow)
await wrapper.runMake(['DESIGN_CONFIG=config.mk', 'synth']);

// Verify tools are available
const tools = await wrapper.verifyTools();
```

**Behavior**:
- **Windows**: Executes tools via WSL (requires WSL installation)
- **Linux**: Executes native binaries directly

### Electron Builder Config (`electron-builder.config.js`)

Defines packaging parameters:

- NSIS installer for Windows with WSL check
- AppImage + .deb for Linux
- ASAR encryption for code protection
- App metadata and signing options

## Building Process

### Step 1: Prepare Tools
```bash
packaging/build-scripts/prepare-tools.sh <project_root> <gui_dir>
```
Copies OpenROAD, Yosys, and other tools to `dist/resources/tools`

### Step 2: Install Dependencies
```bash
npm install -D electron-builder
```

### Step 3: Build Installers
```bash
npm run build
```
Runs electron-builder for each platform

## Configuration

### Environment Variables

For signing and publishing:

```bash
# Code signing (Windows)
export WIN_CERTIFICATE_FILE=/path/to/cert.pfx
export WIN_CERTIFICATE_PASSWORD=password

# Publishing
export PUBLISH_URL=https://releases.example.com/openroad-gui/

# Build version
export BUILD_VERSION=1.0.0
```

## License Activation Flow

### For Users

1. **Download installer** from distribution website
2. **Install** the application
3. **Launch** the application
4. **Activation dialog** appears on first launch
5. **Enter license key** received via email
6. **Activate** - verifies key matches hardware
7. **Use application** - license stored locally

### For Developers

Generate test licenses for testing:

```javascript
const LicenseManager = require('./packaging/license-manager.js');
const manager = new LicenseManager();
const test = manager.generateTestLicense();
console.log(`Hardware ID: ${test.machineId}`);
console.log(`License Key: ${test.licenseKey}`);
```

## Distribution Setup

### Website Download Portal

Host installers on your website:

```
https://openroad-downloads.example.com/
  ├── windows/
  │   ├── OpenROAD-GUI-1.0.0-win.exe
  │   └── OpenROAD-GUI-1.0.0-win.msi
  └── linux/
      ├── OpenROAD-GUI-1.0.0-linux.AppImage
      └── OpenROAD-GUI-1.0.0-linux.deb
```

### License Key Generation (Server-Side)

Upon payment, generate and email license keys:

```javascript
const LicenseManager = require('./packaging/license-manager.js');
const manager = new LicenseManager();

// User provides their hardware ID during checkout
const userHardwareId = req.body.hardwareId;
const licenseKey = manager.generateLicenseKey(userHardwareId);

// Email licenseKey to user
sendEmail(user.email, {
  subject: 'Your OpenROAD GUI License',
  body: `Your license key: ${licenseKey}`,
  instructions: '1. Download installer\n2. Install application\n3. Enter license key on first launch'
});
```

## Troubleshooting

### Build Fails: "electron-builder not found"
```bash
npm install -D electron-builder
```

### Tools not copied
Check that `tools/OpenROAD`, `tools/yosys` directories exist in project root

### Windows build fails with WSL error
Ensure WSL2 is installed:
```powershell
wsl --list --verbose
wsl --set-version Ubuntu-20.04 2
```

### Linux .deb installation fails
```bash
sudo dpkg -i OpenROAD-GUI-*.deb
# If dependencies missing:
sudo apt-get install -f
```

## File Sizes (Approximate)

- **Windows .exe**: 1.5-2 GB (includes bundled tools)
- **Windows .msi**: 1.5-2 GB
- **Linux .AppImage**: 1.5-2 GB
- **Linux .deb**: 1.5-2 GB

*Note: Exact sizes depend on tool versions and compilation options*

## Next Steps

1. **Add installer icons** to `assets/icons/`
2. **Configure payment provider** integration
3. **Set up license server** for key generation
4. **Create installer customization** (branding, etc.)
5. **Implement auto-updater** for future releases
6. **Set up CI/CD** pipeline for automated builds

## Maintenance

### Updating Tool Versions

1. Update tools in `tools/` directory (OpenROAD, Yosys, etc.)
2. Rebuild: `npm run build`
3. Test on Windows (WSL) and Linux
4. Upload new installers to distribution servers

### Version Updates

Bump version in `package.json` and rebuild:

```json
{
  "version": "1.0.1"
}
```

## Support

For issues with:
- **Licensing**: Check `license-manager.js`
- **Tool execution**: Check `tool-wrapper.js`
- **Building**: Check build scripts and electron-builder config
- **Distribution**: See distribution infrastructure section
