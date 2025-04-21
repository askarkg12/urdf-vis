# Product Requirements Document: Robot Visualizer MVP

## 1. Introduction
### **1.1. Purpose**
This document outlines the requirements for the Minimum Viable Product (MVP) of a robot visualization tool. The tool will enable users to load a robot model defined in a URDF file, view its 3D representation, and interactively control its joint positions.
### **1.2. Goal**
The primary goal of the MVP is to provide a basic, interactive visualization of a robot's kinematic structure based on its URDF description, focusing on revolute joints.
### **1.3. Target Audience**
Robotics engineers, researchers, or students who need to visualize robot models defined by URDF files.
### **1.4. Scope**
The MVP focuses on core URDF parsing, rendering of visual geometry (meshes and primitives), and interactive control of revolute joints via sliders. Future versions will expand on this to include reachability visualization and potentially other joint types and analysis features.
## **2. Goals (MVP)**
• Load a robot model specified by a user-selected URDF file.
• Correctly locate and load associated visual geometry files (STL, Box, Cylinder, Sphere).
• Render the robot structure accurately in a 3D environment.
• Provide standard interactive camera controls (pan, zoom, rotate) for the 3D view.
• Allow users to control the position of each revolute joint using individual sliders.
• Update the robot's pose in the 3D view in real-time as sliders are adjusted.
• Provide basic error feedback if loading fails.
## **3. Non-Goals (MVP)**
• Support for joint types other than `revolute`.
• Parsing or visualization of `collision` geometry, `inertial` properties, or complex `material` definitions (beyond basic shapes/mesh loading).
• Physics simulation.
• Saving/exporting robot poses or visualizations.
• Advanced visualization features like reachability maps, custom overlays (this is for future versions).
• 3D section analysis (this is for future versions).
• Support for complex URDF features like `transmission` or `gazebo` tags.
• Creating standalone executable packages (focus is on a runnable Python application first).
## **4. Functional Requirements**
### **4.1. URDF Loading**
- **FR4.1.1:** The application must provide a mechanism for the user to select a URDF file from their local filesystem (e.g., using a `tkinter` file dialog).
- **FR4.1.2:** The application must parse the selected URDF file to extract link hierarchy, joint information (specifically `revolute` joints), and `visual` geometry data.
- **FR4.1.3:** For `visual` elements containing `mesh` geometry, the tool must attempt to load the referenced file. It will assume mesh files (primarily STLs) are located in a subdirectory named `meshes` relative to the location of the URDF file.
- **FR4.1.4:** For `visual` elements containing `geometry` primitives (`box`, `cylinder`, `sphere`), the tool must generate and display the corresponding geometric shape based on the parameters specified in the URDF.
- **FR4.1.5:** The tool must extract joint names and joint limits (min/max angle) for `revolute` joints if specified in the URDF.
### **4.2. Robot Visualization**
- **FR4.2.1:** The application must use the `vedo` library to render the robot model in a 3D viewport.
- **FR4.2.2:** The initial view upon loading a robot should be a standard isometric perspective.
- **FR4.2.3:** The 3D viewport must allow interactive camera manipulation using the mouse (e.g., left-click-drag to rotate, right-click-drag to pan, scroll wheel to zoom), using `vedo`'s default interaction style.
- **FR4.2.4:** The visualization must update in real-time to reflect changes in joint positions triggered by the UI sliders.
### **4.3. Joint Control**
- **FR4.3.1:** For each `revolute` joint identified in the URDF, the application must display a dedicated slider control.
- **FR4.3.2:** Each slider must be labeled with the corresponding joint name extracted from the URDF.
- **FR4.3.3:** If joint limits are defined in the URDF for a revolute joint, the corresponding slider's range must be set to these limits. If not defined, a default range (e.g., -π to +π radians) should be used.
- **FR4.3.4:** Adjusting a slider must trigger an update to the corresponding joint angle in the internal robot model and refresh the 3D visualization (as per FR4.2.4).
### **4.4. Error Handling**
-  **FR4.4.1:** If an error occurs during URDF parsing or mesh file loading (e.g., file not found, invalid format), the application must display a user-friendly error message in a dialog box (e.g., using `tkinter`).
-  **FR4.4.2:** Detailed error information (e.g., stack trace, specific file missing) must be logged to a file (e.g., `error_log.txt`) in the application's runtime directory.
## **5. User Interface (UI) / User Experience (UX)** 
### **5.1. Layout**
The main application window should consist primarily of the `vedo` 3D rendering viewport and a separate area (e.g., a side panel or bottom panel) displaying the labeled joint sliders.
### **5.2. Workflow**
1. User launches the application.
2. User is prompted (or uses a menu option) to select a URDF file via a file dialog.
3. Application attempts to load the URDF and associated meshes.
4. If successful, the robot model appears in the 3D view, and joint sliders are populated.
5. User interacts with the camera using the mouse.
6. User interacts with sliders to change joint angles, observing real-time updates in the 3D view.
7. If loading fails, an error dialog is shown, and details are logged.
## **6. Technical Requirements**
### **6.1. Language**
Python (version 3.13 or higher recommended).
### **6.2. Core Libraries**
-  `vedo`: For 3D visualization and interaction.
-  A URDF parsing library (e.g., `urdfpy`, `yourdfpy`, or custom parsing).
-  `tkinter`: For the file selection dialog and error messages.
-  `numpy`: Likely needed for transformations and geometry.
### **6.3. Target Platforms**
-  Primary Target: Windows
-  Secondary Target: Linux (ensure library compatibility)
### **6.4. Test Case**
The application should be tested and confirmed to work with the user's specific 4-DOF all-revolute robot URDF.
## **7. Future Considerations (Post-MVP)**
-  Support for other joint types (`prismatic`, `continuous`, `fixed`).
-  More robust `package://` URI handling for mesh file locations.
-  Visualization of `collision` geometry.
-  Display of coordinate frames (link origins, tool center point).
-  Integration of custom mesh generation/display for reachability analysis (opacity control is key).
-  Implementation of 3D sectioning/clipping planes.
-  Improved UI/UX (e.g., text input for joint angles, saving/loading poses).
-  Packaging for easier distribution (e.g., using PyInstaller).