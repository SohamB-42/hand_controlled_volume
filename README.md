

# Hand Gesture Volume Control 🖐️🔊

This project allows you to control your system's volume using hand gestures captured through your webcam. By adjusting the distance between your thumb and index finger, you can dynamically increase or decrease the volume in real-time. It's built using **Python**, **OpenCV**, and **MediaPipe**.

-----

## Features ✨

  * **Real-Time Hand Tracking**: Utilizes MediaPipe for high-fidelity hand landmark detection.
  * **Gesture-Based Control**: Translates the distance between the thumb and index finger into system volume levels.
  * **Visual Feedback**: Displays the webcam feed with hand landmarks and a volume bar overlay for an intuitive user experience.
  * **Cross-Platform**: Compatible with Windows and macOS (with minor adjustments for volume control libraries).

-----

## Demo 🎬

Here's a quick look at the project in action\!

![handvol](https://github.com/user-attachments/assets/38555ff9-62b2-419c-baf3-4586633513fa)

-----

## Requirements 🛠️

### Hardware

  * A webcam (Yes the one on your laptop works too)

### Software

  * **Python 3.7+**
  * The following Python libraries:
      * `opencv-python`
      * `mediapipe`
      * `math`
      * `numpy`
      * `time `
      * `pycaw` (for Windows)
      * `osascript` (for macOS, usually pre-installed)

-----

## Installation & Usage 🚀

Follow these steps to get the project up and running on your local machine.

1.  **Clone the repository:**

    ```bash
    git clone https://github.com/SohamB-42/hand_controlled_volume.git
    cd hand-gesture-volume-control
    ```

2.  **Create and activate a virtual environment (Recommended):**

    ```bash
    # For Windows
    python -m venv venv
    venv\Scripts\activate

    # For macOS/Linux
    python3 -m venv venv
    source venv/bin/activate
    ```

3.  **Install the required packages:**
    Create a `requirements.txt` file with the following content:

    ```
    opencv-python
    mediapipe
    numpy
    pycaw
    math
    time
    
    ```

    Then, run the following command to install them:

    ```bash
    pip install -r requirements.txt
    ```

    *Note: For macOS, you won't need `pycaw`. The volume control logic will need to be adapted to use `osascript`.*

4.  **Run the script:**

    ```bash
    python HandVolume.py
    ```

5.  **Control the Volume:**

      * Show your hand to the webcam.
      * The application will detect your hand and draw landmarks.
      * Change the distance between the tip of your **thumb (landmark 4)** and **index finger (landmark 8)** to control the volume.
      * Pinching them close together sets the volume to minimum, and moving them far apart sets it to maximum.

-----

## How It Works ⚙️

The project operates in a few key stages within a continuous loop:

### 1\. Hand Tracking

The script captures video feed from the webcam using **OpenCV**. Each frame is processed by the **MediaPipe Hands** solution, which detects the hand and returns the 3D coordinates of its 21 key landmarks.

### 2\. Gesture Recognition

We are interested in two specific landmarks:

  * **Landmark 4**: The tip of the thumb.
  * **Landmark 8**: The tip of the index finger.

The Euclidean distance between the coordinates of these two points ($(x\_1, y\_1)$ and $(x\_2, y\_2)$) is calculated for each frame using the formula:
$$\text{distance} = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$
This distance serves as the raw input for our volume control.

### 3\. Volume Mapping and Control

The calculated distance, which typically ranges from about 20 to 200 pixels depending on the hand's proximity to the camera, needs to be mapped to the system's volume range (e.g., -65 dB to 0 dB for `pycaw`).

  * We use linear interpolation (`numpy.interp`) to convert the finger distance range `[min_dist, max_dist]` into the volume range `[min_vol, max_vol]`.
  * The mapped volume level is then set using the **pycaw** library, which provides an interface to control the Windows Core Audio APIs.
  * Visual feedback, such as a volume bar and the current volume percentage, is drawn back onto the OpenCV frame shown to the user.

-----


## License 📜

This project is licensed under the MIT License. See the `LICENSE` file for details.
