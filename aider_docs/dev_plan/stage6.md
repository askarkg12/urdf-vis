# Stage 6: Interactive Control - Linking Sliders to Kinematics
**Goal:** Connect UI sliders to the robot's pose, enabling real-time interactive control.

**PR Focus:** Bringing the visualization to life – making it interactive.
## **Steps:**
### 1. **Implement Slider Callback Function:**
- Define the actual callback function that will be passed to `plotter.add_slider()`. Let's call it `update_joint(widget, event)`.
- The `widget` object passed by `vedo` usually has methods like `GetRepresentation().GetValue()` to get the slider's current value and potentially an identifier/title to know *which* joint's slider was moved.
### 2. **Maintain Robot State:**
- Ensure your `Visualizer` class (or main script) maintains the current state of all joint angles (e.g., in a dictionary `self.joint_angles = {'joint1': 0.0, 'joint2': 0.0, ...}`).
### 3. **Update State and Kinematics in Callback:**
- Inside `update_joint()`:
    - Identify which joint corresponds to the slider that triggered the event.
    - Get the new angle value from the slider widget.
    - Update the corresponding angle in your internal `self.joint_angles` state.
    - Recalculate the world transforms for all links by calling `calculate_link_world_transforms` (from Stage 4) using the *updated* `self.joint_angles`. (FR4.3.4)
### 4. **Update Vedo Actor Poses:**
- Inside `update_joint()` (after recalculating transforms):
    - Iterate through your stored `vedo` actors (you'll need a way to map actors back to their corresponding links).
    - For each actor, calculate its new world pose (`T_world_visual = T_world_link * T_link_visual`) using the newly calculated link world transforms.
    - Apply the updated transform to the actor using `actor.apply_transform()`. (FR4.2.4)
### 5. **Connect Callback:**
- Modify the `plotter.add_slider()` calls from Stage 5 to use your actual `update_joint` callback function.
### 6. **Unit Testing (Interaction Logic):**
- Create a test file (e.g., `tests/test_interaction.py`).
- Test the `update_joint` callback logic (or the class method handling it):
    - Mock the `vedo` slider widget and event data.
    - Simulate a slider event for a specific joint with a specific value.
    - Verify that the internal `joint_angles` state is updated correctly for that joint.
    - Verify that the kinematics recalculation function (`calculate_link_world_transforms`) is called with the updated angles (using `unittest.mock.patch` or similar).
    - Verify (by checking mock actor calls or internal state) that the transforms applied to the actors reflect the results of the kinematics recalculation.