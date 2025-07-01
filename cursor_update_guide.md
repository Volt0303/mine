# How to Update Cursor IDE

This guide covers various methods to update Cursor IDE across different platforms, with a focus on Linux systems where updating can be more complex.

## Quick Update Methods

### Method 1: Built-in Update Feature
The simplest way to update Cursor is through its built-in update mechanism:

1. **Automatic Updates**: Cursor typically shows update notifications in the bottom-left corner when new versions are available
2. **Manual Check**: Open Command Palette (`Ctrl + Shift + P` or `Cmd + Shift + P`) and type `Cursor: Attempt Update`
3. **Restart**: Close and reopen Cursor after the update process

**Note**: The built-in updater can be unreliable on Linux systems and may not always work properly.

### Method 2: Download and Reinstall (Windows/Mac)
For Windows and Mac users, you can simply:
1. Visit [cursor.com](https://cursor.com)
2. Click the "Download" button
3. Install the new version over the existing one
4. Your settings and projects will be preserved

## Linux-Specific Update Methods

Linux users face unique challenges with updating Cursor since it's distributed as an AppImage. Here are several reliable methods:

### Method 3: Manual AppImage Replacement
1. Download the latest AppImage from [cursor.com](https://cursor.com)
2. Make it executable: `chmod +x Cursor-*.AppImage`
3. Replace your existing AppImage file
4. Update any desktop shortcuts if necessary

### Method 4: Using Community Scripts

#### Option A: Cursor Linux Installer Script
```bash
# Install/Update using the community installer
bash -c "$(curl -fsSL https://raw.githubusercontent.com/watzon/cursor-linux-installer/main/install.sh)"

# Update with a simple command
cursor --update
```

#### Option B: One-Line Installer Script
```bash
# Download and run installer script
curl https://gist.githubusercontent.com/Kinyugo/9845e18998744ff54b8f0cde3bb37182/raw/install_cursor.sh | bash
```

#### Option C: Updated Script with Latest API
```bash
# Script that uses Cursor's API to get the latest version
curl https://gist.githubusercontent.com/markruler/2820bd05d613c61dac906814a4e282b7/raw/install_cursor.sh | sh
```

### Method 5: Custom Update Script
Create your own update script for automated updates:

```bash
#!/bin/bash
# Custom Cursor update script

APP_DIR="$HOME/Applications"
APP_IMAGE_NAME="cursor.AppImage"
CURRENT_APP_IMAGE="$APP_DIR/$APP_IMAGE_NAME"
DOWNLOAD_URL="https://downloader.cursor.sh/linux/appImage/x64"
TEMP_APP_IMAGE="/tmp/cursor-latest.AppImage"

echo "Checking for Cursor updates..."

# Download latest version
wget -q -O "$TEMP_APP_IMAGE" "$DOWNLOAD_URL"
chmod +x "$TEMP_APP_IMAGE"

# Compare versions using checksums
if [ -f "$CURRENT_APP_IMAGE" ]; then
    CURRENT_CHECKSUM=$(sha256sum "$CURRENT_APP_IMAGE" | awk '{ print $1 }')
    NEW_CHECKSUM=$(sha256sum "$TEMP_APP_IMAGE" | awk '{ print $1 }')
    
    if [ "$CURRENT_CHECKSUM" != "$NEW_CHECKSUM" ]; then
        echo "New version found! Updating..."
        mv "$TEMP_APP_IMAGE" "$CURRENT_APP_IMAGE"
        echo "Cursor updated successfully."
    else
        echo "Cursor is already up to date."
        rm "$TEMP_APP_IMAGE"
    fi
else
    echo "Installing Cursor for the first time..."
    mkdir -p "$APP_DIR"
    mv "$TEMP_APP_IMAGE" "$CURRENT_APP_IMAGE"
fi
```

### Method 6: Extract AppImage (Ubuntu 24.04 Solution)
For Ubuntu 24.04 users who face issues with libfuse2:

1. Download the latest AppImage
2. Extract it: `./Cursor-*.AppImage --appimage-extract`
3. Run the extracted executable: `./squashfs-root/AppRun`
4. Use community scripts that handle extraction automatically

## Troubleshooting Update Issues

### Common Problems and Solutions

#### 1. Update Doesn't Work on Linux
- Try the `--no-sandbox` flag: `./cursor.AppImage --no-sandbox`
- Use community installation scripts instead of built-in updater
- Manually download and replace the AppImage

#### 2. "Attempt Update" Does Nothing
- This is often due to incremental rollouts
- Download manually from the website
- Use community update scripts

#### 3. libfuse2 Errors (Linux)
```bash
# Install libfuse2 (but avoid on Ubuntu 24.04+)
sudo apt install libfuse2

# For Ubuntu 24.04+, use extraction method instead
```

#### 4. Sandbox Errors
- Add `--no-sandbox` flag to your Cursor launch command
- Update desktop entries to include the flag

#### 5. Permission Errors
```bash
# Fix permissions
chmod +x cursor.AppImage
sudo chown $USER:$USER cursor.AppImage
```

## Version Information

### Check Current Version
- **GUI**: Help → About (Linux/Windows) or Cursor → About Cursor (Mac)
- **Command Palette**: `Ctrl+Shift+P` → type "About"

### Latest Release Information
- Visit [changelog.cursor.sh](https://changelog.cursor.sh) for version history
- Check [cursor-ai-downloads repository](https://github.com/oslook/cursor-ai-downloads) for direct download links

## Automated Updates

### Set Up Cron Job (Linux)
```bash
# Edit crontab
crontab -e

# Add line to check for updates daily at 3 AM
0 3 * * * /path/to/your/update-cursor.sh > /tmp/cursor-update.log 2>&1
```

### Alternative Update Tools
- **cursor-setup-wizard**: Comprehensive setup and update tool for Linux
- **Cursor Linux Packages**: Web-based package manager alternative

## Best Practices

1. **Backup Settings**: Always backup your Cursor settings before major updates
2. **Test First**: Try updates on a non-critical system first
3. **Use Scripts**: For Linux, prefer community scripts over built-in updates
4. **Regular Updates**: Check for updates regularly to get latest features and security fixes
5. **Monitor Forums**: Follow [Cursor Community Forum](https://forum.cursor.com) for update announcements and issues

## Platform-Specific Notes

### Windows
- Built-in updater works reliably
- Download from website as backup option

### macOS  
- Built-in updater generally works well
- Download from website for manual updates

### Linux
- Built-in updater is unreliable
- Use community scripts or manual AppImage replacement
- Consider automated update scripts
- Ubuntu 24.04+ users should use extraction method

## Getting Help

If you encounter issues with updating Cursor:

1. Check the [Cursor Community Forum](https://forum.cursor.com)
2. Search for your specific error message
3. Try alternative update methods listed above
4. Report persistent issues to the Cursor team

Remember that Cursor is actively developed, and update mechanisms are constantly being improved. The methods above should cover most scenarios, but always check the official documentation and community forums for the latest information.