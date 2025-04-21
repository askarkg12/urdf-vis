# Stage 3: Loading & Displaying Static Visual Geometry
**Goal:** Load geometry (meshes and primitives) and display them relative to their link origins, without joint transforms.

**PR Focus:** Getting visual shapes into the 3D scene, correctly placed relative to their immediate parent link.
## **Steps:**
### 1. **Geometry Loading Logic:**
- Create a new file (e.g., `src/robot_visualizer/visualizer.py`).
- Implement helper functions to create `vedo` actors from your `Geometry` data structures:
    - `create_mesh_actor(mesh_geometry, urdf_dir)`:
        - Construct the full path to the mesh file (assuming `meshes/` subdirectory relative to `urdf_dir`, as per FR4.1.3). Use `os.path.join`.
        - Load the STL file using `vedo.load()`. Handle potential `FileNotFoundError`.
        - Apply scaling if specified in the URDF.
    - `create_box_actor(box_geometry)`: Create `vedo.Box`.
    - `create_cylinder_actor(cylinder_geometry)`: Create `vedo.Cylinder`.
    - `create_sphere_actor(sphere_geometry)`: Create `vedo.Sphere`.
### 2. **Visual Origin Transformation:**
- Create a utility function (e.g., in `src/robot_visualizer/utils.py`) to convert XYZ and RPY (Roll, Pitch, Yaw in radians) from the URDF `<origin>` tag into a 4x4 transformation matrix (using `numpy`). Ensure correct rotation order (e.g., static ZYX).
- Modify the actor creation functions (or apply after creation) to set the actor's pose using the transformation matrix derived from the `Visual`'s origin (using `actor.apply_transform()` or `actor.pos()`, `actor.orientation()`).
### 3. **Integrate with Main Application:**
- Modify `main.py` (or the `Visualizer` class):
    - After parsing the URDF (from Stage 2), iterate through the links and their associated `Visual` objects.
    - For each `Visual`, call the appropriate actor creation function.
    - Apply the visual origin transformation.
    - Add the created `vedo` actor to the `vedo.Plotter`.
- Run the application with a test URDF. You should see the individual parts of the robot displayed, potentially overlapping, positioned according to their visual origins relative to a common world origin for now.
### 4. **Unit Testing (Geometry & Visuals):**
- Create a test file (e.g., `tests/test_visualizer_geometry.py`).
- **Test Transformation Utility:** Write tests for the XYZ/RPY to 4x4 matrix conversion function with known inputs and expected outputs.
- **Test Actor Creation:**
    - Write tests for `create_box/cylinder/sphere_actor` to ensure they return the correct `vedo` object type.
    - Write tests for `create_mesh_actor`:
        - Mock `vedo.load` and `os.path.exists` to simulate finding/not finding a mesh file.
        - Test that the correct file path is constructed (using the `meshes/` convention).
        - Test error handling if the file is not found.
    - Write tests to verify that the visual origin transformation is correctly applied to the created actors (check `actor.transform` or position/orientation if simpler). Mock the `vedo` actors if necessary to isolate the logic.