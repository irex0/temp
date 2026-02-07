# VidFlow Streamer - Installation Guide

This guide explains how to install and run the self-contained VidFlow Streamer on Ubuntu/Debian based systems.

## Prerequisites

The only external dependency required is **FFmpeg**.

### 1. Install FFmpeg
```bash
sudo apt update
sudo apt install -y ffmpeg
```

Verify installation:
```bash
ffmpeg -version
```

## Running the Application

### Option A: Run the Pre-built Binary
If you have the `vidflow-streamer` binary:

1.  Make it executable:
    ```bash
    chmod +x vidflow-streamer
    ```
2.  Run it:
    ```bash
    ./vidflow-streamer
    ```
    The server will start on port 3000 (default).

### Configuration (Optional)
You can configure the server using environment variables:
```bash
PORT=8080 ./vidflow-streamer
```

## Building from Source (Optional)

If you need to rebuild the project:

1.  **Install Go** (1.21+):
    ```bash
    sudo apt install -y golang
    ```
2.  **Run Build Script**:
    ```bash
    chmod +x build.sh
    ./build.sh
    ```
