# Stage 1: Project Setup & Basic Window
**Goal:** Establish the basic project structure, dependencies, and get an empty 3D window showing.

**PR Focus:** Foundational setup, proving the core visualization library (`vedo`) is working.
## **Steps:**
### 1. **Initialize Repository:**
- Create a new Git repository.
- Add a `.gitignore` file (e.g., from a standard Python template, including `__pycache__`, virtual environment folders, `error_log.txt`).
### 2. **Project Structure:**
- Create a basic directory structure:
    ```
    robot_visualizer/
    ├── src/
    │   └── robot_visualizer/
    │       └── __init__.py
    │       └── main.py
    ├── tests/
    │   └── __init__.py
    ├── requirements.txt
    └── README.md

    ```
### 3. **Add Dependencies:**
- Create a virtual environment (e.g., `python -m venv .venv`).
- Activate the virtual environment.
- Install core libraries: `pip install vedo numpy`
- Install testing framework: `pip install pytest`
### 4. **Basic Application Entry Point:**
- In `src/robot_visualizer/main.py`, write minimal code to import `vedo`.
- Create a main function or script block that initializes a `vedo.Plotter`.
- Call the `show()` method on the plotter to display an empty window.
- Ensure the script can be run (e.g., `python src/robot_visualizer/main.py`).
### 5. **Setup Unit Testing:**
- Configure `pytest` (if needed, e.g., create a `pytest.ini` or `pyproject.toml` section).
- In `tests/`, create a simple test file (e.g., `test_main.py`).
- Write a basic placeholder test (e.g., `test_placeholder(): assert True`) to ensure the test runner is working correctly. Run `pytest`.
### 6. **README:**
- Create an initial `README.md` with the project title and basic setup instructions (virtual env, install requirements).