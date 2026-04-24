# PA-System
Here is a clean, fully formatted README template you can copy and paste directly into your project's `README.md` file. It explains exactly how to deploy the two scripts we just built.

***

```markdown
# 🎤 Mac/Pi Local Intercom System

A purely local, Wi-Fi-based Public Address (PA) and Intercom system. This project turns your Mac into a central command dashboard and a headless Raspberry Pi into a network-attached speaker node. 

The Mac hosts a local web app with 5 functions: Live Voice Recording, Text-to-Speech, MP3/WAV playback, and delayed scheduling for both text and audio.

## 📋 Prerequisites
* **Sender:** A Mac with Python 3 installed.
* **Receiver:** A Raspberry Pi running Raspberry Pi OS (Lite or Desktop).
* **Hardware:** A speaker with a standard 3.5mm AUX cable plugged directly into the Raspberry Pi's headphone jack.
* **Network:** Both devices must be connected to the same local network (Wi-Fi or Ethernet).

---

## 💻 Part 1: Setting up the Mac Sender

The Mac acts as the central brain. It hosts the web dashboard and handles all the file conversions and scheduling.

1. **Install Python Dependencies:**
   Open your Mac's Terminal and run:
   ```bash
   pip3 install flask requests werkzeug
   ```
2. **Run the Server:**
   Navigate to the folder containing `mac_sender.py` and execute it:
   ```bash
   python3 mac_sender.py
   ```
3. **Access the Dashboard:**
   Open Chrome, Safari, or any web browser and go to:
   👉 **`http://localhost:8080`**

---

## 🔊 Part 2: Setting up the Raspberry Pi Receiver

The Raspberry Pi runs completely headless. The provided setup script installs all audio dependencies, forces sound out of the 3.5mm jack, and creates a background service so the Pi is always listening for the Mac.

1. **Transfer the script:** Move the `pi_receiver_setup.sh` file to your Raspberry Pi, or create a new file on the Pi and paste the bash script contents into it:
   ```bash
   nano setup.sh
   ```
   *(Paste the code, press `Ctrl+X`, then `Y`, then `Enter`)*

2. **Make it executable:**
   ```bash
   chmod +x setup.sh
   ```

3. **Run the installer:**
   ```bash
   ./setup.sh
   ```

**That's it for the Pi.** The script automatically creates a `systemd` service (`pi_receiver.service`). Whenever you plug the Pi into power, it will quietly boot up, connect to the Wi-Fi, and wait for commands from the Mac.

---

## 🚀 How to Use

Once both scripts are running, look at the top of your Mac's Web Dashboard. It should say **"🟢 PI CONNECTED & READY"**. 

You can now use the 5 tabs:
1. **Live Talk:** Hold the button, speak into your Mac's microphone, and release to instantly broadcast to the Pi.
2. **Live TTS:** Type a message to have a robotic voice read it on the Pi.
3. **Play MP3/WAV:** Upload an audio file from your Mac to play instantly.
4. **Schedule TTS:** Type a message and set a delay (in seconds). The Mac will queue it and send it when the timer hits zero.
5. **Schedule Audio:** Upload an audio file and set a delay.

*Note: The Pi automatically converts incoming MP3s and browser voice recordings into native WAV formats using `ffmpeg` so they play flawlessly via the hardware jack.*
```
