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
## ✅ Option: Use Static FFmpeg Build (Fastest & Cleanest)

Best for **servers, streaming, HLS, RTMP, scripts**, no dependency issues.

### 1️⃣ Download static build

```bash
curl -L https://johnvansickle.com/ffmpeg/releases/ffmpeg-release-amd64-static.tar.xz -o ffmpeg.tar.xz
```

### 2️⃣ Extract & install

```bash
tar -xf ffmpeg.tar.xz
cd ffmpeg-*-static
sudo cp ffmpeg ffprobe /usr/local/bin/
```

### 3️⃣ Verify

```bash
ffmpeg -version
```

✔ Includes **H.264, AAC, MP3, HLS, RTMP, x264, x265**
✔ No repo headaches
✔ Ideal for EC2


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


Below is a **clean, reliable setup** for Amazon Linux 2023 (also works on AL2).

---

## ✅ Step 1: Place the executable properly

Make sure the file is executable and in a stable path:

```bash
sudo chmod +x ~/vidflow-streamer
sudo mv ~/vidflow-streamer /usr/local/bin/vidflow-streamer
```

Test manually:

```bash
vidflow-streamer
```

Stop it if it runs.

---

## ✅ Step 2: Create a systemd service

Create the service file:

```bash
sudo nano /etc/systemd/system/vidflow-streamer.service
```

Paste **exactly this**:

```ini
[Unit]
Description=Vidflow Streamer Service
After=network.target

[Service]
Type=simple
ExecStartPre=/bin/sleep 10
ExecStart=/usr/local/bin/vidflow-streamer
Restart=always
RestartSec=5
User=ec2-user
WorkingDirectory=/usr/local/bin
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

Save and exit.

---

## ✅ Step 3: Enable & start the service

```bash
sudo systemctl daemon-reload
sudo systemctl enable vidflow-streamer
sudo systemctl start vidflow-streamer
```

---

## ✅ Step 4: Verify it’s running in background

```bash
systemctl status vidflow-streamer
```

View logs:

```bash
journalctl -u vidflow-streamer -f
```

---

## ✅ What this setup guarantees

✔ Starts automatically on **EC2 boot**
✔ Waits **10 seconds** before launching
✔ Runs in **background**
✔ Auto-restarts if it crashes
✔ Clean logs via `journalctl`
✔ No `nohup`, no `screen`, no hacks

---

## 🔥 Optional Enhancements (Recommended)

### If your app needs network fully ready:

Change:

```ini
After=network.target
```

to:

```ini
After=network-online.target
Wants=network-online.target
```

---

### If you want to pass arguments:

```ini
ExecStart=/usr/local/bin/vidflow-streamer --config /etc/vidflow/config.yml
```

---

### If you want environment variables:

```ini
Environment=NODE_ENV=production
Environment=STREAM_PORT=8080
```

