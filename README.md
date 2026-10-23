# Real-Time Face Detection with OpenCV and Tkinter

A desktop app that detects faces from your webcam in real time using **OpenCV Haar cascades**. A simple **Tkinter** window provides Start and Stop buttons, and the video feed shows a bounding box around every detected face along with a live face count.

## Features

- Real-time face detection from the default webcam
- Green bounding box and numbered label (`Face 1`, `Face 2`, ...) for each face
- Live total face count shown on the video frame
- Start / Stop control through a Tkinter GUI
- Press `q` in the video window to stop detection
- Detection runs in a background thread, so the GUI stays responsive
- Friendly error message if the webcam cannot be opened

## Tech Stack

| Component | Purpose |
|---|---|
| Python 3.8+ | Language |
| OpenCV (`opencv-python`) | Webcam capture and Haar cascade face detection |
| Tkinter | GUI with Start/Stop buttons |
| `threading` | Runs the detection loop without freezing the GUI |

## Project Structure

```
.
├── face_detection_app.py   # Main application
├── requirements.txt        # Python dependencies
└── README.md
```

## How It Works

1. Clicking **Start** opens the webcam and launches the detection loop in a separate thread.
2. Each frame is converted to grayscale and passed to OpenCV's pre-trained `haarcascade_frontalface_default.xml` classifier through `detectMultiScale(gray, 1.1, 5)`.
3. A rectangle and label are drawn for every detected face, and the total count is drawn in the top-left corner.
4. Clicking **Stop** (or pressing `q` in the video window) ends the loop, releases the camera, and closes the video window.

The cascade file ships with `opencv-python`, so there is nothing extra to download.

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/milad-zarei/realtime-face-detection-opencv-tkinter.git
cd realtime-face-detection-opencv-tkinter
```

### 2. Create a virtual environment and install dependencies

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate
# Linux / macOS
source .venv/bin/activate

pip install -r requirements.txt
```

> **Linux note:** Tkinter may need to be installed separately: `sudo apt install python3-tk`

### 3. Run

```bash
python face_detection_app.py
```

A small window with two buttons will appear. Click **Start** to begin detection.

## Usage

| Action | Result |
|---|---|
| Click **Start** (شروع) | Opens the webcam and starts detecting faces |
| Click **Stop** (توقف) | Stops detection and closes the video window |
| Press `q` in the video window | Same as Stop |

## Configuration

You can tune detection in `face_detection_app.py`:

```python
faces = face_cascade.detectMultiScale(gray, 1.1, 5)
#                                          |    |
#                                  scaleFactor  minNeighbors
```

- **`scaleFactor`** (default `1.1`): smaller values detect more faces but run slower.
- **`minNeighbors`** (default `5`): higher values reduce false positives but may miss some faces.
- **Camera index:** `cv2.VideoCapture(0)` uses the default webcam. Change `0` to `1`, `2`, ... for other cameras.

## Troubleshooting

- **"Webcam could not be opened":** Close other apps that use the camera, check your OS camera permissions, or try a different camera index.
- **Faces not detected:** Make sure the face is well lit and facing the camera. The Haar cascade used here detects **frontal faces** only.

## Possible Improvements

- Update Tkinter widgets from the main thread (for example with `window.after`) instead of from the worker thread
- Detect profile faces or switch to a DNN-based detector for better accuracy
- Show the video inside the Tkinter window instead of a separate OpenCV window
- Save snapshots or record video
- Add an English/Persian language option for the UI

## License

Add a license of your choice (for example MIT) before publishing.
