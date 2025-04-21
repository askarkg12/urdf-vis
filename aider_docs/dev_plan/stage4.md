# Stage 4: Kinematic Chain Assembly (Static Pose)
**Goal:** Assemble the robot parts correctly based on the URDF joint hierarchy, displayed in a static (e.g., zero-angle) pose.

**PR Focus:** Correctly applying transformations to assemble the robot visually in its default configuration.
## **Steps:**
### 1. **Forward Kinematics Logic:**
- Create a new file (e.g., `src/robot_visualizer/kinematics.py`).
- Implement a function `calculate_link_world_transforms(links, joints, root_link_name, joint_angles={})`:
    - Input: Parsed links, joints, the name of the root link, and a dictionary mapping joint names to angles (use 0 for all joints initially).
    - Output: A dictionary mapping link names to their 4x4 world transformation matrices.
    - Algorithm:
        - Start from the root link (identity transform).
        - Perform a traversal (e.g., Breadth-First Search or Depth-First Search) through the kinematic tree using the parent/child relationships defined in the `joints`.
        - For each link, calculate its world transform by:
            - Getting the parent link's world transform.
            - Calculating the transform from the parent link origin to the joint origin (using the joint's `<origin>` tag).
            - Calculating the joint rotation transform (for revolute joints, this is a rotation around the joint's `axis` by the current `joint_angle`).
            - Calculating the transform from the joint origin to the child link origin (this is implicitly handled by the parent->joint->child sequence).
            - Combine these transforms: `T_world_child = T_world_parent * T_parent_joint * T_joint_rotation`.
        - Store the calculated `T_world_link` for each link.
### 2. **Apply Transforms in Visualizer:**
- Modify `visualizer.py` (or `main.py`):
    - Call `calculate_link_world_transforms` with zero angles to get the initial world transforms for all links.
    - When creating/adding actors for each `Visual` element belonging to a specific link:
        - Get the calculated world transform for that link.
        - Get the local transform for the visual element relative to its link origin (from Stage 3).
        - Combine these: `T_world_visual = T_world_link * T_link_visual`.
        - Apply this final world transform to the `vedo` actor using `actor.apply_transform()`.
- Run the application. The robot should now appear assembled correctly in its zero pose.
### 3. **Unit Testing (Kinematics):**
- Create a test file (e.g., `tests/test_kinematics.py`).
- Create simple kinematic chain definitions (links, joints) programmatically or using minimal URDFs for testing.
- Write tests for `calculate_link_world_transforms`:
    - Test a single link (should be identity or root transform).
    - Test a simple 2-link chain with one revolute joint at zero angle. Verify the child link's world transform against manual calculation.
    - Test the same 2-link chain with a non-zero joint angle (e.g., 90 degrees). Verify the child link's transform.
    - Test a slightly more complex chain (e.g., 3 links, 2 joints) with various joint origins and axes.
    - Use `numpy.testing.assert_allclose` to compare transformation matrices.