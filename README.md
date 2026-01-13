# 🖐️ Wireless Sound Control (macOS)

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![OpenCV](https://img.shields.io/badge/opencv-%23white.svg?style=for-the-badge&logo=opencv&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-007fbf?style=for-the-badge&logo=google&logoColor=white)
![macOS](https://img.shields.io/badge/mac%20os-000000?style=for-the-badge&logo=apple&logoColor=white)

This project enables macOS users to control system volume and mute/unmute audio via webcam-detected hand gestures. It uses MediaPipe for hand tracking and OpenCV for visuals. Volume is adjusted by thumb-index finger distance. An open palm mutes, a peace sign unmutes. 

## 📺 Demo

## 🚀 Features

* **🔊 Gesture-Based Volume Control:** Adjust system volume by varying the distance between your thumb and index finger.
* **🔇 Mute/Unmute Gestures:**
    * **Mute:** Perform an "open palm" gesture (all five fingers extended) to mute the audio.
    * **Unmute:** Make a "peace sign" gesture (index and middle fingers extended, ring and pinky fingers curled) to unmute the audio.
* **📊 Dynamic Visuals:**
    * Real-time display of hand landmarks and connections.
    * On-screen volume percentage and a dynamic volume bar.
    * System mute status indicator.
    * Brief visual confirmation (red circle for mute, green for unmute) upon successful gesture recognition.
* **⚙️ macOS System Integration:** Seamlessly controls system audio settings using `osascript`.

## 📖 Gesture Guide

| Gesture | Action | Detection Logic |
| :---: | :--- | :--- |
| 🤏 | **Pinch / Stretch** | Measures distance between **Thumb Tip** and **Index Tip**. |
| 🖐️ | **Open Palm** | Triggered when **all 5 fingers** are detected as extended. |
| ✌️ | **Peace Sign** | **Index & Middle** extended; **Ring & Pinky** curled. |
| 🔴 | **Red Circle** | Visual confirmation that system is now **Muted**. |
| 🟢 | **Green Circle** | Visual confirmation that system is now **Unmuted**. |

## ⚙️ How It Works

The application captures video from your webcam and processes each frame using MediaPipe's hand tracking model. Key hand landmarks, specifically the tips of the thumb and index finger, are used to calculate the distance for volume control. Other finger tip and PIP (Proximal Interphalangeal) joint positions are analyzed to determine open palm and peace sign gestures for mute/unmute functionality.

Volume changes are smoothed for a better user experience. Mute/unmute actions are debounced with a cooldown period to prevent accidental rapid toggling.

## 🛠️ Setup and Installation

### Prerequisites

* Python 3.7+
* macOS operating system (required for `osascript` audio control)
* Webcam

### Installation Steps

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/WirelessSoundControl.git](https://github.com/YOUR_USERNAME/WirelessSoundControl.git)
    cd WirelessSoundControl
    ```
    (Replace `YOUR_USERNAME` with your actual GitHub username)

2.  **Install dependencies:**
    It's highly recommended to use a virtual environment.
    ```bash
    python3 -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    pip install opencv-python mediapipe numpy
    ```

3.  **Grant Camera Permissions (macOS):**
    When you run the script for the first time, your macOS system will likely prompt you to grant camera access to the terminal application (or IDE like VS Code/Jupyter) that is running the script. You *must* grant these permissions in `System Settings > Privacy & Security > Camera` for the application to work.

## 💻 Usage

1.  **Run the Jupyter Notebook:**
    ```bash
    jupyter notebook WirelessSoundControl.ipynb
    ```
    Open the `WirelessSoundControl.ipynb` file in your browser, then run all cells.

2.  **Webcam Index:** The code attempts to open the webcam at index `1` (`cap = cv2.VideoCapture(1)`). If you have multiple cameras or your default camera is at a different index, you might need to change this line. You can uncomment and run the `list_cameras()` function in the notebook to find available camera indices.


## 📝 Important Notes

* This project is specifically designed for **macOS** due to its reliance on `osascript` for system audio control. It will not work out-of-the-box on Windows or Linux for audio control, though the gesture detection part could be adapted.
* Ensure adequate lighting for optimal hand detection by MediaPipe.
* The `mute_cooldown_duration` variable (default 2 seconds) prevents rapid mute/unmute toggling.

## 🛠️ Troubleshooting (macOS Specific)

### 1. 📷 Camera Access Denied
If the application crashes with an `Error: Could not open video stream` message:

* **System Permissions:** Navigate to `System Settings > Privacy & Security > Camera`.
* **Enable Apps:** Ensure your Terminal app or IDE (VS Code, PyCharm, etc.) is toggled **ON**.
* **Reset Permissions:** If issues persist, run this command in your terminal to reset camera access:  
  `tccutil reset Camera`

### 2. 🔍 Finding the Correct Camera Index
If you have a MacBook with a built-in camera and a Continuity Camera (iPhone) or external webcam, index `1` might not be correct.

* **The Error:** You see `WARNING: AVCaptureDeviceTypeExternal is deprecated` or a black screen.
* **The Fix:** In the notebook, change `cap = cv2.VideoCapture(1)` to `cap = cv2.VideoCapture(0)`. 
* **Detection Tool:** Use the provided `list_cameras()` utility function in the notebook to identify the correct ID.

### 3. 📝 AppleScript (osascript) Failures
If the volume bar moves in the OpenCV window but your system volume remains unchanged:

* **Accessibility:** Occasionally, macOS requires you to grant **Accessibility** permissions to the app running the script (Terminal or IDE).
* **Check Audio Output:** Ensure your active output device is a standard hardware device (like Built-in Speakers or Headphones). Third-party software bridges (e.g., Background Music, EqMac) may block standard `osascript` volume commands.
