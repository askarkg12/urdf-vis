# Stage 5: UI Elements - File Dialog & Sliders

**Goal:** Add the UI elements for loading files and controlling joints.

**PR Focus:** Building the UI components. Sliders won't yet control the robot.

## **Steps:**

### 1. **Integrate File Dialog:** (FR4.1.1)
- Import `tkinter` and `tkinter.filedialog`.
- In `main.py` or `visualizer.py`, add a mechanism to trigger the file dialog (e.g., a button in the `vedo` window or running it at startup if no file is provided).
- Use `filedialog.askopenfilename(filetypes=[("URDF Files", "*.urdf")])` to get the URDF file path from the user.
- Pass this path to the URDF parsing logic. Hide the `tkinter` root window (`root = tkinter.Tk(); root.withdraw()`).
### 2. **Add Vedo Sliders:** (FR4.3.1)
- In `visualizer.py` (within the `vedo.Plotter` setup):
    - After parsing the URDF and identifying the revolute joints:
    - Iterate through the revolute joints.
    - For each joint, add a slider using `plotter.add_slider()`:
        - Provide a placeholder callback function for now (e.g., `lambda widget, event: None`).
        - Set the slider range (`xmin`, `xmax`) using the parsed joint limits (FR4.3.3). Use defaults (e.g., -pi to +pi) if limits are not defined.
        - Set the slider title using the joint name (FR4.3.2).
        - Position the sliders appropriately in the UI (e.g., bottom left).
### 3. **Refactor UI Logic (Optional but Recommended):**
- Consider encapsulating the visualization and UI logic within a class (e.g., `RobotVisualizer`) to manage the plotter, actors, sliders, and state (current joint angles).
### 4. **Unit Testing (UI Logic - Limited):**
- Create a test file (e.g., `tests/test_ui_logic.py`).
- Unit testing direct GUI interaction is hard. Focus on testing the *preparation* of data for the UI:
    - Write tests for any function that extracts joint names and limits specifically for slider creation. Given mock joint data, verify the correct names and ranges are returned.
    - Testing the `tkinter` dialog itself usually requires integration/manual testing.