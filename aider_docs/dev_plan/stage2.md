# Stage 2: URDF Parsing Logic
**Goal:** Implement the ability to read a URDF file and extract the necessary information (links, revolute joints, visuals).

**PR Focus:** Core data processing – turning a URDF file into usable Python objects.
**Steps:**
### 1. **Choose/Implement URDF Parser:**
- Decide on a parsing strategy:
    - *Option A (Recommended):* Add a URDF parsing library (e.g., `yourdfpy` or `urdfpy`) to `requirements.txt` and install it.
    - *Option B:* Use Python's built-in `xml.etree.ElementTree` for basic parsing (more work, less robust).
### 2. **Define Data Structures:**
- In a new file (e.g., `src/robot_visualizer/urdf_parser.py`), define Python classes or dataclasses to represent the relevant URDF elements:
    - `Link(name, visual=[Visual(...)])`
    - `Joint(name, type, parent_link, child_link, origin_xyz=[0,0,0], origin_rpy=[0,0,0], axis=[0,0,1], limits={'lower':0, 'upper':0})` (Focus on `revolute`)
    - `Visual(geometry, origin_xyz=[0,0,0], origin_rpy=[0,0,0])`
    - `Geometry(type, params)` (e.g., `MeshGeometry(filename, scale)`, `BoxGeometry(size)`, `CylinderGeometry(radius, length)`, `SphereGeometry(radius)`)
### 3. **Implement Parsing Function:**
- In `urdf_parser.py`, create a function `parse_urdf(urdf_path)` that:
    - Takes the path to the URDF file as input.
    - Uses the chosen library/method to parse the XML.
    - Iterates through the URDF elements (`link`, `joint`).
    - Populates instances of your data structures.
    - Focus on extracting data required by the PRD (FR4.1.2, FR4.1.4, FR4.1.5): link names, joint names, types (filter for `revolute`), parent/child links, joint origins (xyz, rpy), joint axes, joint limits, visual origins, and visual geometry (mesh filenames, primitive parameters).
    - Return a representation of the robot structure (e.g., a dictionary of links, a list of joints, or a root link object).
### 4. **Unit Testing (Parsing):**
- Create a test file (e.g., `tests/test_urdf_parser.py`).
- Create sample minimal URDF files (as strings or small files in the `tests` directory) covering:
    - Basic link-joint structure.
    - Revolute joints with and without limits.
    - Visual elements with meshes, boxes, cylinders, spheres.
    - Visual elements with origins (xyz/rpy).
- Write `pytest` tests for the `parse_urdf` function:
    - Test successful parsing of link names.
    - Test successful parsing of joint names, types, parent/child, axis.
    - Test correct extraction of joint limits (or defaults if missing).
    - Test correct extraction of visual geometry types and parameters (mesh filename, box size, etc.).
    - Test correct parsing of visual origins (xyz/rpy).
    - Test handling of non-revolute joints (should likely be ignored gracefully for MVP).