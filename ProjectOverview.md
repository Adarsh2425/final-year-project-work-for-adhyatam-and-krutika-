# Time-Based Lathe Simulator - Project Overview

## Introduction

The Time-Based Lathe Simulator is a software application designed to simulate the operation of a CNC lathe machine. It takes G-code programs as input, interprets the commands, and visually simulates the movement of tools, material removal, and the overall machining process. The simulator aims to provide a realistic representation of lathe operations, including time-based movements, acceleration/deceleration, and eventually, multi-tool operations and cycle time optimization.

## Goals

*   Develop a functional single-tool lathe simulator that accurately interprets and visualizes G-code programs.
*   Implement time-based simulation, where tool movements are accurately timed based on feed rates, distances, and acceleration/deceleration.
*   Visualize the workpiece, toolpaths, tool movements, and work origin.
*   Provide a user-friendly interface for loading G-code, controlling the simulation, and inspecting machine state.
*   Refactor the codebase for improved structure, readability, and maintainability.
*   Extend the simulator to support multiple tools operating simultaneously (parallel machining).
*   Implement algorithms to analyze toolpaths and optimize G-code for reduced cycle time.
*   Generate optimized G-code that can potentially be used on real CNC machines (with appropriate post-processing).

## Target Audience

*   CNC programmers and operators who want to visualize and optimize their G-code programs before running them on a physical machine.
*   Students and educators in the field of CNC machining and manufacturing.
*   Hobbyists and makers interested in CNC machining.

## Key Features

*   **G-code Parsing:**  Interprets a wide range of standard G-code and M-code commands.
*   **Time-Based Simulation:** Accurately simulates the time it takes to execute each G-code command, taking into account feed rates, rapid rates, acceleration, and deceleration.
*   **Visualization:** Provides a graphical representation of the workpiece, tool, toolpath, and work origin.
*   **User Interface:**  Offers an intuitive interface for controlling the simulation (start, pause, stop, step, load G-code, etc.).
*   **Machine State Display:** Shows the current state of the machine (position, feed rate, mode, spindle speed, etc.).
*   **Multi-Tool Support (Future):**  Simulates the simultaneous operation of multiple tools.
*   **Cycle Time Optimization (Future):** Analyzes toolpaths and generates optimized G-code to reduce cycle time.

## Technology Stack

*   **Programming Language:** C++
*   **Graphics Libraries:** OpenGL, GLAD, GLFW
*   **UI Library:** ImGui
*   **Math Library:** GLM
*   **Build System:** CMake (or similar)
*   **Version Control:** Git
