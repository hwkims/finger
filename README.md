# Real-time Hand Pose Detection with Finger-Specific Sounds

[![GitHub](https://img.shields.io/badge/GitHub-hwkims-blue?logo=github)](https://github.com/hwkims)
![image](https://github.com/user-attachments/assets/af7d7539-c109-438f-b5f0-ac7ef4a9ac3f)


This project demonstrates real-time hand pose estimation using a webcam feed, specifically identifying when individual fingers are raised. It utilizes **TensorFlow.js** with the **MediaPipe Hands** model to detect detailed hand landmarks.

When a specific finger (thumb, index, middle, ring, or pinky) is detected as being raised, the application triggers a unique sine wave sound using the **Web Audio API**. Each finger corresponds to a different musical note (C4 to G4).

The entire application is self-contained within a single HTML file.

## ✨ Features

*   **Real-time Hand Landmark Detection:** Uses TensorFlow.js and the MediaPipe Hands model to track 21 keypoints on the hand via webcam.
*   **Hand Skeleton Visualization:** Draws the detected keypoints and connecting bones onto an HTML Canvas overlay.
*   **Individual Finger Raised Detection:** Determines if each finger (thumb, index, middle, ring, pinky) is extended upwards.
*   **Finger-Specific Audio Feedback:** Plays a distinct sine wave sound (C4, D4, E4, F4, G4) for each raised finger using the Web Audio API. No external audio files are needed.
*   **Sound Cooldown:** Prevents excessive sound repetition by implementing a short cooldown period for each finger's sound.
*   **Client-Side Implementation:** Runs entirely within the user's web browser.
*   **Single File:** All code (HTML, CSS, JavaScript) is included in one `.html` file for simplicity.

## 🛠️ Technologies Used

*   **HTML5:** Canvas API, WebRTC (`getUserMedia` for webcam access)
*   **CSS3:** Basic styling and layout.
*   **JavaScript (ES6+):** Core application logic.
*   **TensorFlow.js (`@tensorflow/tfjs`):** Core machine learning library.
    *   `@tensorflow/tfjs-backend-webgl`: WebGL backend for GPU acceleration.
*   **TensorFlow.js Models (`@tensorflow-models/hand-pose-detection`):** Pre-trained MediaPipe Hands model for hand tracking.
*   **Web Audio API:** For generating and playing synthesized sounds (sine waves) directly in the browser.

## 🚀 How to Run

1.  **Clone or Download:**
    *   Clone this repository:
        ```bash
        git clone https://github.com/hwkims/finger.git
        cd finger
        ```
    *   Or, download the main HTML file (e.g., `hand_motion_sound.html` or `index.html`) directly.

2.  **Run a Local Web Server:**
    *   **Crucial:** You *must* run this HTML file from a local web server due to browser security policies regarding webcam access (`getUserMedia`) and potentially model loading. Opening the file directly via `file://` protocol will likely **not** work.
    *   **Option 1: Using Python 3** (Usually built-in on Linux/macOS, installable on Windows)
        ```bash
        # Navigate to the directory containing the HTML file in your terminal
        python -m http.server
        ```
        Then open your browser to `http://localhost:8000`.
    *   **Option 2: Using Node.js and `http-server`**
        ```bash
        # Install http-server globally if you haven't already
        npm install -g http-server
        # Navigate to the directory containing the HTML file
        http-server .
        ```
        Then open your browser to the address shown (e.g., `http://127.0.0.1:8080`).
    *   **Option 3: Using VS Code's Live Server Extension**
        *   Install the "Live Server" extension in Visual Studio Code.
        *   Open the project folder in VS Code.
        *   Right-click the HTML file and select "Open with Live Server".

3.  **Open in Browser:**
    *   Navigate to the local server address provided by your chosen method (e.g., `http://localhost:8000`).

4.  **Grant Camera Permission:**
    *   Your browser will likely prompt you for permission to access your webcam. Click "Allow".

5.  **Interact:**
    *   Position your hand in front of the webcam. You should see a skeletal overlay drawn on your hand.
    *   Raise individual fingers clearly. You should hear a distinct musical note corresponding to each finger you raise (Thumb: C4, Index: D4, Middle: E4, Ring: F4, Pinky: G4).

## 📝 How Finger Detection Works (Simplified)

The application continuously analyzes the 21 detected hand landmarks. To determine if a finger is "raised" or "extended", it compares the vertical position (Y-coordinate) of the **fingertip landmark** with the Y-coordinate of a **lower joint landmark** on the same finger (typically the PIP joint, or IP/MCP for the thumb).

Since the Y-coordinate decreases as you move up the screen, if the fingertip's Y is significantly *smaller* than the lower joint's Y, the finger is considered raised, and the corresponding sound is triggered (subject to the cooldown).

## 💡 Notes & Considerations

*   **Performance:** Detection speed and smoothness depend heavily on your computer's hardware (CPU/GPU). The 'lite' version of the MediaPipe Hands model is used by default for better performance, but 'full' can be configured for potentially higher accuracy.
*   **Lighting:** Good and consistent lighting improves detection accuracy.
*   **Audio Context:** Some browsers require user interaction (like a click anywhere on the page) before allowing audio playback. The code includes a basic handler to attempt resuming the `AudioContext` on the first click.
*   **Detection Thresholds:** The sensitivity for detecting a "raised" finger (the Y-coordinate difference threshold) and the minimum confidence score for hand detection (`minConfidence`) can be adjusted in the JavaScript code for fine-tuning.
*   **Mirroring:** The video and canvas are flipped horizontally (`transform: scaleX(-1)`) to provide a more natural "mirror" view. The detection logic accounts for this.

## 📄 License

This project is open source. Please add a `LICENSE` file (e.g., MIT License) if you intend to distribute it more formally. (LICENSE file TBD)

---

Feel free to fork, modify, and experiment!
