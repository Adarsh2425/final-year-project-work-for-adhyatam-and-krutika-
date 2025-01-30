# final-year-project-work-for-adhyatam-and-krutika-

**Refactoring Plan: Time-Based Lathe Simulator - Code Management and Structure**



## 1. Introduction

This document outlines a plan to refactor the existing codebase of the Time-Based Lathe Simulator. The primary goals of this refactoring effort are:

*   **Improve Code Structure:** Reorganize the code into more logical modules and classes to enhance clarity and maintainability.
*   **Enhance Readability:** Apply consistent formatting, naming conventions, and add more comprehensive comments to make the code easier to understand.
*   **Increase Modularity:** Break down large functions into smaller, more manageable units with well-defined responsibilities.
*   **Reduce Code Duplication:** Identify and eliminate redundant code sections.
*   **Prepare for Multi-Tool Functionality:** Restructure the code in a way that facilitates the implementation of multi-tool simulation and toolpath optimization.
*   **Improve Error Handling:** Implement more robust error handling and reporting.
*   **Optimize Performance:** Address any identified performance bottlenecks.

## 2. Current Code Assessment

The current codebase provides a good foundation for a single-tool lathe simulator. However, several areas can be improved through refactoring:

*   **Large `LatheSimulator` Class:** The `LatheSimulator` class is currently responsible for a wide range of tasks, including G-code parsing, simulation, rendering, and UI management. This can make the class difficult to manage and extend.
*   **Limited Modularity:** Some functions, particularly within the `LatheSimulator` class, are quite lengthy and could benefit from being broken down into smaller, more focused functions.
*   **G-code Parser in `LatheSimulator`:** The G-code parsing logic is tightly coupled with the `LatheSimulator` class. It would be beneficial to separate it into its own class for better organization and potential reuse.
*   **Preparation for Multi-Tool:** The current code is designed for a single tool. Significant changes will be needed to accommodate multiple tools and their interactions.
*   **Error Handling:** Error handling is present but could be more comprehensive and informative.
*   **OpenGL State Management:** The code could benefit from a more organized way of managing OpenGL state changes.

## 3. Refactoring Steps

The refactoring process will be divided into several key steps:

### Phase 1: Structural Improvements and G-code Parser Extraction

*   **Step 1: Create a `GCodeParser` Class:**
    *   Extract all G-code parsing logic from the `LatheSimulator` class into a new `GCodeParser` class.
    *   The `GCodeParser` class will be responsible for:
        *   Reading G-code from a file or string.
        *   Tokenizing the G-code into individual commands and parameters.
        *   Parsing the commands into a suitable data structure (e.g., a list of `GCommand` objects or a custom Abstract Syntax Tree).
        *   Performing basic syntax and semantic error checking.
        *   Handling different comment styles and whitespace variations.
        *   Potentially, loading and managing tool definitions (if they are defined in the G-code file or a separate file).
    *   The `GCodeParser` should have a well-defined interface for interacting with the `LatheSimulator` (e.g., a `parse()` function that returns a structured representation of the G-code program).

*   **Step 2: Create a `Machine` or `Lathe` Class:**
    *   Extract the machine state and simulation logic from the `LatheSimulator` class into a new `Machine` or `Lathe` class.
    *   This class will encapsulate:
        *   `MachineState` struct (or an equivalent).
        *   Functions for executing individual G-code commands (e.g., `executeG00`, `executeG01`, `executeG02`, etc.).
        *   Functions for managing the simulation loop, including time-based movement calculations and acceleration/deceleration.
        *   Potentially, functions for material removal simulation.
    *   The `LatheSimulator` will then hold an instance of this `Machine` or `Lathe` class and delegate simulation tasks to it.

*   **Step 3: Create a `Renderer` Class (Optional):**
    *   Consider extracting the OpenGL rendering code into a separate `Renderer` class.
    *   This class would be responsible for:
        *   Initializing and managing OpenGL objects (VAOs, VBOs, shaders).
        *   Rendering the workpiece, toolpath, tool, work origin, and any other visual elements.
        *   Managing the view and projection matrices.
    *   This step can improve the separation of concerns and make the rendering code more reusable.

*   **Step 4: Refactor `LatheSimulator`:**
    *   With the G-code parsing, machine state, and rendering logic extracted, the `LatheSimulator` class will primarily act as a coordinator or manager.
    *   Its responsibilities will include:
        *   Initializing the `GCodeParser`, `Machine` (or `Lathe`), and `Renderer` (if applicable).
        *   Managing the main simulation loop (start, pause, stop, step).
        *   Handling user input and updating the view parameters.
        *   Creating and managing the ImGui user interface.
        *   Passing data between the different components (e.g., passing parsed G-code to the `Machine`, passing machine state to the `Renderer`).

*   **Step 5: Review and Improve Error Handling:**
    *   Review the error handling in each of the new classes.
    *   Implement more informative error messages that include line numbers and context.
    *   Consider using exceptions or a dedicated error reporting mechanism.

*   **Step 6: Unit Testing (Optional):**
    *   If time permits, start writing unit tests for the `GCodeParser` and the `Machine` (or `Lathe`) classes to ensure their correctness and prevent regressions during future development.

### Phase 2: Preparing for Multi-Tool Simulation

*   **Step 7: Refactor `MachineState` for Multiple Tools:**
    *   Modify the `MachineState` struct (or equivalent) to store the state of multiple tools.
    *   Use a `std::vector` or `std::map` to hold tool-specific data (position, feed rate, status, etc.).
    *   Add a mechanism to track the currently active tool.

*   **Step 8: Update `Machine` (or `Lathe`) Class for Multi-Tool:**
    *   Modify the `Machine` class functions to handle multiple tools:
        *   `executeG00`, `executeG01`, etc., will need to take a tool index or identifier as a parameter.
        *   The simulation loop will need to manage the movement and state updates of all tools.
    *   Implement initial logic for switching between active tools based on tool change commands (`M06`). This might not involve true parallelism yet but will prepare the foundation for it.

*   **Step 9: Modify `GCodeParser` for Tool Definitions:**
    *   If not already done, enhance the `GCodeParser` to handle tool definitions, either from comments within the G-code (e.g., `;T1: 5.0mm Roughing Tool`) or from a separate tool definition file.
    *   Store tool information in a suitable data structure within the `GCodeParser` or a separate `ToolLibrary` class.

### Phase 3: Code Cleanup and Optimization

*   **Step 10: Code Formatting and Style:**
    *   Apply a consistent code formatting style throughout the project (e.g., using a tool like clang-format).
    *   Ensure consistent naming conventions for variables, functions, and classes (e.g., use camelCase or snake_case consistently).

*   **Step 11: Commenting and Documentation:**
    *   Add comprehensive comments to all classes, functions, and complex code sections.
    *   Generate documentation using a tool like Doxygen.

*   **Step 12: Code Duplication Removal:**
    *   Identify and eliminate any redundant or duplicated code sections.

*   **Step 13: Performance Profiling and Optimization:**
    *   Use profiling tools (e.g., gprof, Valgrind) to identify performance bottlenecks.
    *   Optimize critical sections of the code, such as the simulation loop, material removal calculations, and rendering code.

## 4. Tools and Technologies

*   **Programming Language:** C++
*   **Build System:** CMake (or similar)
*   **Version Control:** Git
*   **Graphics Libraries:** OpenGL, GLAD, GLFW
*   **UI Library:** ImGui
*   **Math Library:** GLM
*   **Code Formatting:** clang-format (optional)
*   **Profiling Tools:** gprof, Valgrind (optional)
*   **Documentation Generator:** Doxygen (optional)
*   **Unit Testing Framework:** Google Test, Catch2 (optional)

## 5. Deliverables

*   Refactored codebase with improved structure, readability, and maintainability.
*   Comprehensive code comments and documentation.
*   (Optional) Unit tests for critical components.
*   (Optional) Performance analysis report.

## 6. Timeline

This refactoring project is estimated to take approximately 4-6 weeks, depending on the available time and resources. The timeline can be broken down as follows:

*   **Phase 1:** 2-3 weeks
*   **Phase 2:** 1-2 weeks
*   **Phase 3:** 1 week

## 7. Risks and Mitigation Strategies

*   **Risk:** Refactoring might introduce new bugs or regressions.
    *   **Mitigation:** Thorough testing after each refactoring step, use of version control, and potentially writing unit tests.
*   **Risk:** The refactoring process might take longer than expected.
    *   **Mitigation:** Break down the refactoring into smaller, manageable tasks, prioritize the most important refactorings, and regularly assess progress.
*   **Risk:** The introduction of new classes and abstractions might increase code complexity if not done carefully.
    *   **Mitigation:** Keep the design simple and focused, avoid over-engineering, and document the design decisions clearly.

## 8. Conclusion

This refactoring plan provides a roadmap for improving the quality and maintainability of the Time-Based Lathe Simulator codebase. By carefully restructuring the code, extracting key components into separate classes, and addressing areas for improvement, this plan will pave the way for implementing the more advanced features of multi-tool simulation and toolpath optimization. Regular testing, code reviews, and documentation will be essential to ensure the success of this refactoring effort.

## 9. Proposed Code Structure

The refactored code will be organized into the following modules:

*   **`GCodeParser`:** Responsible for parsing G-code programs.
*   **`Machine` (or `Lathe`):** Represents the state and behavior of the lathe machine.
*   **`Renderer`:** Handles OpenGL rendering.
*   **`LatheSimulator`:** Coordinates the simulation and manages the UI.
*   **`Tool`:** Represents a cutting tool.
*   **`Toolpath`:** Represents a sequence of tool movements.
*   **`Workpiece`:** Represents the material being machined.
*   **`Utility`:** (Optional) Contains helper functions and constants.

## 10. File Structure

The following file structure is proposed:

```
TimeBasedLatheSimulator/
├── CMakeLists.txt
├── include/
│   ├── GCodeParser.h
│   ├── Machine.h
│   ├── Renderer.h
│   ├── LatheSimulator.h
│   ├── Tool.h
│   ├── Toolpath.h
│   ├── Workpiece.h
│   └── Utility.h (optional)
└── src/
    ├── GCodeParser.cpp
    ├── Machine.cpp
    ├── Renderer.cpp
    ├── LatheSimulator.cpp
    ├── Tool.cpp
    ├── Toolpath.cpp
    ├── Workpiece.cpp
    ├── Utility.cpp (optional)
    └── main.cpp
```

## 11. Class Details

### 11.1. `GCodeParser`

*   **File:** `GCodeParser.h`, `GCodeParser.cpp`
*   **Purpose:** Parses G-code programs and produces a structured representation.
*   **Members:**
    *   `std::vector<std::string> gCodeProgram;` // Stores the original lines of the G-code program (or you can remove it after parsing if not needed).
    *   `std::queue<GCommand> commandQueue;` // Stores parsed G-code commands.
    *   `std::map<int, Tool> tools;` // Stores tool definitions (if parsed from G-code or a separate file).
    *   `std::string currentFilename;` // Stores the filename of the currently loaded G-code file.
*   **Functions:**
    *   `GCodeParser();` // Constructor.
    *   `~GCodeParser();` // Destructor.
    *   `bool loadGCode(const std::string& filename);` // Loads and parses a G-code file. Returns true on success, false on failure.
    *   `bool parseGCode(const std::string& gcodeString);` // Parses a G-code program from a string. Returns true on success, false on failure.
    *   `GCommand getNextCommand();` // Returns the next parsed command from the queue. (Or change your simulation loop to process all commands at once.)
    *   `void clearCommands();` // Clears the command queue.
    *   `void addToolDefinition(const Tool& tool);` // Adds a tool definition (if you have a separate way to define tools).
    *   `Tool getToolDefinition(int toolNumber);` // Retrieves a tool definition by tool number.
    *   `std::vector<Toolpath> segmentToolpaths();` // Segments the parsed G-code into a vector of `Toolpath` objects (see `Toolpath` class below). This is important for multi-tool analysis.
    *   **(Private) `GCommand parseCommand(const std::string& line, size_t lineNumber);`** // Parses a single line of G-code into a `GCommand` object.
    *   **(Private) Error handling functions as needed.**

### 11.2. `Machine` (or `Lathe`)

*   **File:** `Machine.h`, `Machine.cpp`
*   **Purpose:** Represents the state and behavior of the lathe machine.
*   **Members:**
    *   `MachineState state;` // Stores the current machine state (position, feed rate, mode, etc.). See the modified `MachineState` below.
    *   `std::vector<Tool> activeTools;` // A vector to store the currently active tools.
*   **Functions:**
    *   `Machine();` // Constructor.
    *   `~Machine();` // Destructor.
    *   `void executeCommand(const GCommand& command);` // Executes a single G-code command and updates the machine state.
    *   `void update(float deltaTime);` // Updates the machine state based on elapsed time (for time-based simulation). Handles motion, acceleration, deceleration.
    *   `void setFeedRate(float feedRate);`
    *   `void setRapidRate(float rapidRate);`
    *   `void setAbsoluteMode(bool absolute);`
    *   `void setSpindleSpeed(float rpm);`
    *   `void setWorkOffset(const glm::vec2& offset);`
    *   `void setCurrentTool(int toolNumber);` // Function to change the current tool.
    *   `void startMove(const glm::vec2& target, float feedRate);` // Starts a linear move.
    *   `void startArcMove(const glm::vec2& target, const glm::vec2& center, float feedRate, bool clockwise);` // Starts an arc move.
    *   `void stopMove();`
    *   `glm::vec2 getCurrentPosition();`
    *   `float getFeedRate();`
    *   `bool isMoving();`
    *   **(Private) Helper functions for motion calculations, acceleration/deceleration, etc.**

**Modified `MachineState`:**

```cpp
struct MachineState {
    std::vector<glm::vec2> toolPositions; // Current positions of all tools (X, Z).
    std::vector<glm::vec2> toolTargetPositions; // Target positions for all tools.
    std::vector<bool> toolIsMoving; // Flags indicating if each tool is currently moving.
	bool isSimulationRunning = false;
	float feedRate = 100.0f;
	float rapidRate = 5000.0f;
	bool absolutePositioning = true;
	std::chrono::steady_clock::time_point moveStartTime;
	std::vector<float> toolMoveDurations; // Move durations for each tool.
	size_t currentCommandLine = 0;
	std::chrono::steady_clock::time_point simulationStartTime;
	int lastGCode = -1;
	float spindleSpeedRPM = 0.0f;
	glm::vec2 workOffset = { 200.0f, 0.0f };
	float acceleration = 1000.0f; 
	float deceleration = 1000.0f; 
	int currentToolNumber = 1; 
    int numTools = 1; // Number of tools (you'll need to set this appropriately).
    // Add other necessary state variables as needed.
};
```

### 11.3. `Renderer` (Optional)

*   **File:** `Renderer.h`, `Renderer.cpp`
*   **Purpose:** Handles OpenGL rendering.
*   **Members:**
    *   `GLuint shaderProgram;`
    *   `GLuint modelLoc, viewLoc, projectionLoc, colorLoc;`
    *   `GLuint workpieceVAO, workpieceVBO;`
    *   `GLuint toolPathVAO, toolPathVBO;`
    *   `ViewState viewState;` // Might be better to pass this as a parameter to render functions.
*   **Functions:**
    *   `Renderer();` // Constructor.
    *   `~Renderer();` // Destructor.
    *   `void setupShaders();`
    *   `void initialize();` // Initializes OpenGL objects (VAOs, VBOs, shaders). Consider creating a separate function to initialize the workpiece geometry.
    *   `void render(const Machine& machine, const Workpiece& workpiece, const std::vector<Toolpath>& toolpaths);` // Renders the scene.
    *   `void renderWorkpiece(const Workpiece& workpiece);`
    *   `void renderToolpath(const Toolpath& toolpath, const Tool& tool);` // Render a single toolpath with its associated tool's color.
    *   `void renderTool(const glm::vec2& position, const Tool& tool);`
    *   `void renderWorkOrigin(const glm::vec2& origin);`
    *   `void setViewportSize(int width, int height);` // Updates the viewport size.
    *   **(Private) Helper functions for setting shader uniforms, managing OpenGL state, etc.**

### 11.4. `LatheSimulator`

*   **File:** `LatheSimulator.h`, `LatheSimulator.cpp`
*   **Purpose:** Coordinates the simulation and manages the UI.
*   **Members:**
    *   `GCodeParser parser;`
    *   `Machine machine;`
    *   `Renderer renderer;` (if you create a `Renderer` class)
    *   `Workpiece workpiece;`
    *   `std::vector<Toolpath> toolpaths;` // Stores the segmented toolpaths.
    *   `bool isPaused;`
    *   `bool skipLine;`
    *   // ... other UI-related variables (you can move them to a separate UI class if you want further decoupling).
*   **Functions:**
    *   `LatheSimulator();` // Constructor.
    *   `~LatheSimulator();` // Destructor.
    *   `void run();` // Main simulation loop (handles events, updates, rendering).
    *   `void handleInput(GLFWwindow* window);`
    *   `void renderGUI();`
    *   `void loadGCode(const std::string& filename);` // Calls the `GCodeParser` to load and parse a G-code file.
    *   `void startSimulation();`
    *   `void pauseSimulation();`
    *   `void stopSimulation();`
    *   `void stepSimulation();` // Executes a single step of the simulation.
    *   `void skipLine();` // Skips the current line in the G-code program.
    *   `void resetSimulation();`
    *   **(Private) Helper functions for UI event handling, etc.**

### 11.5. `Tool`

*   **File:** `Tool.h`, `Tool.cpp`
*   **Purpose:** Represents a cutting tool.
*   **Members:**
    *   `int toolNumber;`
    *   `float toolDiameter;`
    *   `std::string toolType;`
    *   `glm::vec4 color;` // Or `ImVec4` if you prefer to use ImGui's color type directly.
*   **Functions:**
    *   `Tool();` // Default constructor.
    *   `Tool(int toolNumber, float toolDiameter, const std::string& toolType, const glm::vec4& color);` // Constructor.

### 11.6. `Toolpath`

*   **File:** `Toolpath.h`, `Toolpath.cpp`
*   **Purpose:** Represents a sequence of tool movements associated with a specific tool.
*   **Members:**
    *   `int toolNumber;` // The tool associated with this toolpath.
    *   `std::vector<glm::vec2> points;` // The vertices of the toolpath.
    *   `float startZ;` // Starting Z-depth of the toolpath.
    *   `float endZ;`   // Ending Z-depth of the toolpath.
*   **Functions:**
    *   `Toolpath();` // Constructor.
    *   `Toolpath(int toolNumber);` // Constructor.
    *   `void addPoint(const glm::vec2& point);`
    *   `void clear();`

### 11.7. `Workpiece`

*   **File:** `Workpiece.h`, `Workpiece.cpp`
*   **Purpose:** Represents the workpiece.
*   **Members:**
    *   `std::vector<glm::vec2> vertices;` // Vertices defining the workpiece's shape.
    *   `float length;`
    *   `float diameter;`
*   **Functions:**
    *   `Workpiece();` // Constructor.
    *   `Workpiece(float length, float diameter);` // Constructor.
    *   `void initialize();` // Initializes the workpiece vertices based on its dimensions.
    *   `void update(const std::vector<Toolpath>& toolpaths);` // Updates the workpiece geometry based on material removal (you'll need to implement the material removal algorithm here).

### 11.8. `Utility` (Optional)

*   **File:** `Utility.h`, `Utility.cpp`
*   **Purpose:** Contains helper functions and constants.
*   **Members:**
    *   `const float PI = 3.14159265359f;` // Or use a more accurate value if needed.
*   **Functions:**
    *   `glm::vec2 calculateArcPoint(const glm::vec2& center, float radius, float angle);` // Calculates a point on an arc.
    *   `float calculateArcLength(float radius, float startAngle, float endAngle);`
    *   // ... other utility functions as needed.

## 12. Main Function (`main.cpp`)

The `main.cpp` file will be simplified to:

```cpp
#include "LatheSimulator.h"
#include "glfw/glfw3.h" // Assuming you use GLFW

int main() {
    // Initialize GLFW, create a window, etc.
    // ... (GLFW and OpenGL initialization code as before)

    LatheSimulator simulator;
    simulator.run(); 

    // ... (Cleanup code as before)
    return 0;
}
```

## 13. Example Usage (Illustrative)

```cpp
// In LatheSimulator::loadGCode:
void LatheSimulator::loadGCode(const std::string& filename) {
    if (parser.loadGCode(filename)) {
        toolpaths = parser.segmentToolpaths();
        // Initialize the machine with the number of tools used
        machine.state.numTools = parser.getToolCount();
        machine.initialize();
        // ... other initialization related to the loaded G-code
    } else {
        // Handle error (e.g., display an error message in the UI)
    }
}

// In LatheSimulator::run (or a similar function in your main loop):
void LatheSimulator::run() {
    while (!glfwWindowShouldClose(window)) {
        // ... event handling ...

        if (!isPaused) {
            if (!commandQueue.empty()) {
                 GCommand cmd = commandQueue.front();
                 commandQueue.pop();
                 machine.executeCommand(cmd);
            }
             float deltaTime = /* ... calculate delta time ... */;
             machine.update(deltaTime);
        }

        // ... rendering ...
        renderer.render(machine, workpiece, toolpaths);

        // ... ImGui rendering ...
    }
}
```

## 14. Further Considerations

*   **Abstract Syntax Tree (AST):** For more complex G-code parsing and analysis, consider building an AST instead of just a queue of commands.
*   **Multi-Tool Optimization:** The `LatheSimulator` or a separate `Optimizer` class will be responsible for analyzing toolpaths, determining dependencies, and generating optimized G-code for parallel execution.
*   **OpenGL State Management:** Use a state management system or wrapper functions to reduce redundant OpenGL calls (e.g., binding the same shader program or VAO multiple times).
*   **Material Removal:** Implement the material removal algorithm in the `Workpiece` class. This will involve calculating the intersection of the tool's swept volume with the workpiece geometry.
*   **Error Handling:** Use exceptions or a consistent error reporting mechanism to handle errors during parsing, simulation, and rendering.

## 15. Conclusion

This refactoring plan provides a detailed guide for restructuring the Time-Based Lathe Simulator code to enhance its organization, readability, and maintainability. It also lays the foundation for implementing multi-tool simulation and toolpath optimization. By following this plan, you will create a more robust and extensible codebase that is easier to understand, modify, and extend with new features. Remember to test thoroughly after each refactoring step to prevent regressions.
