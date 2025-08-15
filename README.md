# Neurai ESP32 Flash Tool

A modern, browser-based ESP32 flashing tool that allows you to upload firmware binaries directly to ESP32 devices without any additional software installation.

## 🌟 Features

- **No Software Installation Required**: Works entirely in the browser using Web Serial API
- **Modern UI**: Clean, responsive interface with visual feedback
- **Real-time Progress**: Live progress bar and console output during flashing
- **Multiple Firmware Support**: Dropdown menu to select from available firmware files
- **Configurable Parameters**: Adjustable baud rate and flash address
- **Auto-Detection**: Automatically detects ESP32 chip type and MAC address
- **Error Handling**: Clear error messages and status indicators

## 🔧 Requirements

### Browser Compatibility
- Google Chrome 89+ 
- Microsoft Edge 89+
- Opera 75+
- Any Chromium-based browser with Web Serial API support

> ⚠️ **Note**: Firefox and Safari are currently NOT supported as they don't implement the Web Serial API.

### Hardware Requirements
- ESP32 development board
- USB cable (data transfer capable)
- Computer with USB port

## 📦 Installation

1. Clone this repository:
```bash
git clone https://github.com/NeuraiProject/neurai-esp32-flash-tool.git
cd neurai-esp32-flash-tool
```

2. Create the binary directory structure:
```bash
mkdir bin
```

3. Place your ESP32 firmware files (.bin) in the `/bin` directory:
```
project-root/
├── index.html
├── README.md
└── bin/
    ├── ESP32-S3-Lily-Display.v.1.0.bin
    ├── ESP32-S3-Lily-Display.v.1.1.bin
    └── ESP32-S3-Lily-Display.v.1.2.bin
```

4. Serve the files using any web server. Examples:

**Using Python 3:**
```bash
python -m http.server 8000
```

**Using Node.js (http-server):**
```bash
npm install -g http-server
http-server -p 8000
```

**Using PHP:**
```bash
php -S localhost:8000
```

5. Open your browser and navigate to:
```
http://localhost:8000
```

## 🚀 Usage

### Basic Flashing Process

1. **Connect Your ESP32**
   - Plug your ESP32 board into your computer via USB
   - Open the tool in a compatible browser

2. **Configure Settings** (Optional)
   - Adjust baud rate if needed (default: 115200)
   - Set flash address (default: 0x0)

3. **Connect to Device**
   - Click the "CONNECT" button
   - Select your ESP32 device from the browser's serial port dialog
   - Wait for connection confirmation

4. **Select Firmware**
   - Choose the desired firmware from the dropdown menu
   - The list is populated from files in the `/bin` directory

5. **Flash Firmware**
   - Click the "FLASH" button
   - Monitor progress in the progress bar and console
   - Device will automatically reset after successful flashing

6. **Disconnect** (Optional)
   - Click "DISCONNECT" when finished

### Flash Address Reference

Common ESP32 flash addresses:

| Component | Address | Description |
|-----------|---------|-------------|
| Bootloader | 0x1000 | Second stage bootloader |
| Partition Table | 0x8000 | Partition information |
| NVS | 0x9000 | Non-volatile storage |
| OTA Data | 0xd000 | OTA selection data |
| Factory App | 0x10000 | Main application |
| OTA_0 | 0x110000 | First OTA partition |
| OTA_1 | 0x210000 | Second OTA partition |

## 🔌 API Configuration

To dynamically load firmware files from your server, modify the `loadFirmwareList()` function:

```javascript
async function loadFirmwareList() {
    try {
        // Replace with your API endpoint
        const response = await fetch('/api/firmware/list');
        const files = await response.json();
        
        const firmwareSelect = document.getElementById('firmwareSelect');
        files.forEach(file => {
            const option = document.createElement('option');
            option.value = `/bin/${file.name}`;
            option.textContent = file.name;
            firmwareSelect.appendChild(option);
        });
    } catch (error) {
        console.error('Error loading firmware list:', error);
    }
}
```

## 📁 Project Structure

```
neurai-esp32-flash-tool/
├── index.html          # Main application file
├── README.md          # Documentation
├── LICENSE            # License file
└── bin/               # Firmware binaries directory
    └── *.bin          # ESP32 firmware files
```

## 🛠️ Advanced Configuration

### Custom Baud Rates

The tool supports various baud rates. Common values:
- 115200 (default)
- 230400
- 460800
- 921600

Higher baud rates may provide faster flashing but could be less stable depending on your USB connection and cable quality.

### Firmware File Naming Convention

Recommended naming convention for firmware files:
```
firmware_v[VERSION]_[DATE].bin
firmware_v1.2.3_20240115.bin
