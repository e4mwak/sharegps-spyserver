# RTL TCP Launcher

Small Windows GUI for running `rtl_tcp.exe` without editing batch files.

This project wraps the prebuilt RTL-SDR command-line tools in a portable launcher so you can:

- run `rtl_tcp` on `127.0.0.1`
- bind it to a detected LAN IP
- bind it to any custom IPv4 address
- choose the TCP port from the GUI
- view a live log while the server is running
- ship the app as a portable `.exe` folder

## Why This Exists

The original folder only included the RTL-SDR binaries and one batch file with a hardcoded IP address. This launcher turns that into a simple server UI that is easier to use on any PC.

The server-first workflow is intentional. The Share GPS user guide recommends server mode for TCP setups because it usually avoids extra firewall friction on the PC side. This launcher follows the same idea: start a local or LAN listener, then connect clients to it.

## Features

- `Localhost` mode for `127.0.0.1`
- `Detected LAN IP` mode with refresh
- `Custom IP` mode
- custom port field
- start and stop controls
- live log box
- portable runtime lookup for `rtl_tcp.exe` and required DLLs
- PyInstaller build script for a portable Windows build

## Files

- `rtl_tcp_gui.py`: Tkinter launcher source
- `build_portable.ps1`: build script for the portable Windows bundle
- `dist/rtl_tcp_gui/`: ready-to-copy portable output
- `rtl_tcp.exe`: RTL-SDR TCP server
- `librtlsdr.dll`
- `libusb-1.0.dll`
- `libwinpthread-1.dll`

## Quick Start

1. Open `dist/rtl_tcp_gui`.
2. Run `rtl_tcp_gui.exe`.
3. Choose one of these modes:
   - `Localhost` to listen only on `127.0.0.1`
   - `Detected LAN IP` to use a local network address found on this PC
   - `Custom IP` to manually enter the bind address
4. Enter the port you want to use. Default is `1234`.
5. Click `Start rtl_tcp`.
6. Connect your SDR client to the selected IP and port.

## Portable Use

Copy the whole `dist/rtl_tcp_gui` folder to another Windows PC.

Keep these files in the same folder as `rtl_tcp_gui.exe`:

- `rtl_tcp.exe`
- `librtlsdr.dll`
- `libusb-1.0.dll`
- `libwinpthread-1.dll`

Do not copy only the EXE by itself. The full folder is the portable app.

## Build From Source

Requirements:

- Windows
- Python 3.12 or later

Build command:

```powershell
powershell -ExecutionPolicy Bypass -File .\build_portable.ps1 -PythonExe "C:\Path\To\python.exe"
```

Output:

- `dist/rtl_tcp_gui/rtl_tcp_gui.exe`
- `dist/rtl_tcp_gui/rtl_tcp.exe`
- `dist/rtl_tcp_gui/librtlsdr.dll`
- `dist/rtl_tcp_gui/libusb-1.0.dll`
- `dist/rtl_tcp_gui/libwinpthread-1.dll`

## Notes

- Default `rtl_tcp` port is `1234`.
- `Localhost` mode is best when the client runs on the same PC.
- `Detected LAN IP` mode is best when another device on your network needs to connect.
- If a client cannot connect over LAN, check Windows Firewall and confirm the selected IP is correct for the active adapter.
- This launcher starts `rtl_tcp` with bind address and port only. Advanced tuner parameters are not exposed in the current GUI.

## Troubleshooting

### The app starts but `rtl_tcp` does not run

Make sure the RTL files are present beside the launcher:

- `rtl_tcp.exe`
- `librtlsdr.dll`
- `libusb-1.0.dll`
- `libwinpthread-1.dll`

### No LAN IP is detected

- confirm the PC is connected to a network
- click `Refresh`
- if needed, use `Custom IP`

### The EXE works on one PC but not another

- copy the full `dist/rtl_tcp_gui` folder, not only `rtl_tcp_gui.exe`
- install the correct RTL-SDR USB driver on the target PC

## Project Status

Current GUI scope:

- IP selection
- port selection
- start and stop
- live log
- portable EXE packaging

Possible next improvements:

- gain control
- frequency field
- sample-rate field
- device index selection
- saved profiles

## Acknowledgment

Connection workflow notes were informed by the Share GPS user guide:

- https://www.jillybunch.com/sharegps/user.html

