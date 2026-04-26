# Face Mask Detector

A real-time face mask detection system that uses a custom-trained Haar cascade classifier with OpenCV, displays a live camera feed in a Qt desktop application, and communicates the detection result to an Arduino over serial to control indicator LEDs.



## Motivation

This project connects three distinct systems into a single pipeline: a webcam feed processed by OpenCV for mask detection, a Qt-based GUI for visual feedback, and an Arduino microcontroller that drives physical LEDs based on the detection result. The Haar cascade classifier (`myhaar_maskdetection.xml`) is custom-trained rather than relying on a pre-built model, allowing it to target face mask presence specifically.

## Features

- Detects face masks in real-time using a custom-trained Haar cascade classifier
- Displays the live camera feed with bounding boxes around detected regions
- Shows color-coded status text (green for mask detected, red for no mask)
- Sends detection state to an Arduino via serial to control red/green indicator LEDs
- Starts and stops the camera with a single button
- Auto-detects the connected Arduino by vendor and product ID

## Tech Stack

| Category | Technology |
|----------|-----------|
| Language | C++17 |
| GUI Framework | Qt 6.3.1 (Widgets, SerialPort) |
| Computer Vision | OpenCV 4.6.0 |
| Microcontroller | Arduino Uno |
| Build System | qmake |
| Compiler | MSVC 2019 (64-bit) |

## Architecture

The application follows a sensor-to-actuator pipeline: the webcam captures frames that are converted to grayscale and histogram-equalized before being passed to the Haar cascade classifier. Detection results drive two output channels in parallel — the Qt GUI updates a color-coded label, and a single-character command (`g` or `r`) is sent over serial to the Arduino. The Arduino firmware reads the serial input and toggles the corresponding LED.

The Haar cascade XML is embedded as a Qt resource and extracted to a temporary file at runtime, keeping the application self-contained as a single deployable binary. Screen-to-world coordinate transformation is handled via a flipped Y-axis graphics transform on the `QGraphicsView`.

## Getting Started

### Prerequisites

- Windows 10 or later
- Arduino Uno (optional, for LED feedback — vendor ID `9025`, product ID `67`)

### Installation

1. Run the installer at `Installer/Maskenerkennunginstaller.exe`.
2. Launch the application.

> [!NOTE]
> The Arduino is optional. The application runs without it and will log a debug message if no board is detected.

### Building from Source

If you want to build from source instead of using the installer, you need Qt 6.3.x (with the `serialport` module), OpenCV 4.6.0, and MSVC 2019 or a compatible C++17 compiler.

1. Place the OpenCV build directory at `./opencv/build/`.
2. Open `V4.pro` in Qt Creator or build via command line:
   ```bash
   qmake V4.pro
   make
   ```
3. Flash the Arduino with `arduinoMaskenErkennung.ino` via the Arduino IDE.

## Usage

1. Connect the Arduino with LEDs wired to pins 6 (red) and 7 (green).
2. Launch the application.
3. Click **Start** to begin the camera feed and mask detection.
4. The status label shows "Maske vorhanden" (green) or "Keine Maske vorhanden" (red).
5. The corresponding LED on the Arduino lights up simultaneously.
6. Click **Stop** to end the camera feed.


## License

This project is licensed under the Apache License 2.0. See [LICENSE.txt](LICENSE.txt) for details.
