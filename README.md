# Air-Canvas

This project implements an "Air Canvas" system that allows users to draw in real-time on a virtual canvas using hand gestures, specifically by detecting a green-colored object. It leverages OpenCV for video processing, color detection, and drawing functionalities.


## Key Highlights:

### Real-time Drawing: 
Enables interactive drawing directly on the live video feed.

### Color Detection: 
Utilizes HSV color space for robust detection of a specific color (green by default) as the "pen."

### Intuitive Control: 
Allows users to draw by moving the detected object and select colors or clear the canvas by positioning the object over on-screen buttons.

### OpenCV Application: 
Demonstrates practical use of OpenCV for tasks like video capture, color segmentation, contour detection, and drawing primitives.

### User Interface: 
Integrates simple UI elements (color selection rectangles and text labels) directly into the video stream for enhanced usability.

## Technical Breakdown:
The "Air Canvas" project follows a clear sequence of operations:

### 1. Initialization:

- Imports cv2 and numpy.

- Defines a set of drawing colors (blue, magenta, green, red, yellow) and sets a default color.

- Sets a min_area threshold for contours to filter out small, noisy detections.

- Initializes cv2.VideoCapture(0) to access the default webcam and retrieves frame width and height.

- Creates a blank canvas (np.zeros) of the same dimensions as the video frame, which will serve as the drawing surface.

- Defines lower_bound and upper_bound arrays for green color detection in the HSV color space.

- Creates a kernel for morphological operations (dilation).

- Initializes previous_center_point to 0 to track the "pen" movement.

### 2. Main Loop (while True):

- Frame Capture: Reads each frame from the webcam.

- Frame Flipping: Flips the frame horizontally (cv2.flip) for a more natural mirror-like view.

- Color Segmentation:

   - Converts the frame from BGR to HSV color space.

   - Creates a binary mask (cv2.inRange) by segmenting out the green color based on the defined HSV bounds.

   - Applies dilation (cv2.dilate) to the mask to enlarge the segmented area and connect broken regions.

- Contour Detection: Finds all contours in the binary mask.

- Drawing Logic:

  - If contours are detected:

     - Identifies the biggest contour (cmax) based on area.

     - Calculates the area of cmax.

     - If area exceeds min_area threshold:

        - Calculates the center point (cX, cY) of the cmax using image moments.

        - Draws a small circle at the center point on the live frame to indicate the detected "pen" position.

        - Color/Clear Selection: If previous_center_point is 0 (meaning drawing hasn't started or was reset) and the cY is within the top region (where buttons are):

            - Checks cX to determine if the "pen" is over the "CLEAR ALL" button or one of the color selection buttons, and updates the canvas or color accordingly.

       - Line Drawing: If previous_center_point is not 0 (meaning drawing is in progress), it draws a line (cv2.line) on the canvas from the previous_center_point to the current center point using the selected color.

       - Updates previous_center_point to the current center point for the next frame.

   - If area is not greater than min_area, previous_center_point is reset to 0, effectively stopping drawing.

- Canvas Overlay:

   - Converts the canvas to grayscale and then to a binary inverse mask (canvas_binary).

   - Uses cv2.bitwise_and and cv2.bitwise_or to overlay the drawn content from the canvas onto the live frame, making the drawing visible.

- UI Elements: Draws rectangles and adds text labels on the live frame to represent the "CLEAR ALL" button and the various color selection buttons.

- Display: Shows the live frame ("Frame") and the canvas ("Canvas") in separate OpenCV windows.

- Termination: Breaks the loop if the 'q' key is pressed (cv2.waitKey(1) == ord('q')).

### 3. Cleanup: 
Releases the video capture object and destroys all OpenCV windows.

## Learnings & Feedback:

I'd love to hear your thoughts and suggestions on this project! Feel free to provide any feedback on its functionality, code structure, potential improvements, or any other observations you might have.
