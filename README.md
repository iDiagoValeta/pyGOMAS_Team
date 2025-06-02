# Project IDIAVAL - Coordinated Intelligent Agents for pyGOMAS Capture The Flag

## 1. Overview
This project implements "IDIAVAL", a team of coordinated intelligent agents designed to play Capture The Flag (CTF) in the pyGOMAS environment. The team consists of a Captain, Soldiers, a Medic, and a Field Operator, each with specialized roles and behaviors. The strategy, detailed in `memoria_AIN_IDIAVAL.pdf`, emphasizes arc formations, anti-friendly fire logic, proactive support (healing and ammunition), and coordinated resource management.

## 2. About This Repository
This repository contains:
* **Agent Implementations (`.zip` file(s))**: You should find one or more zipped archives. Please **unzip them**. They are expected to contain:
    * AgentSpeak (`.asl`) files for `CaptainIDIAVAL`, `SoldierIDIAVAL`, `MedicIDIAVAL`, and `FieldopIDIAVAL` agents.
    * Python code for custom actions/functions used by `CaptainIDIAVAL` (e.g., for formation calculations like `.calculatePos` and `.squadForm` mentioned in `memoria_AIN_IDIAVAL.pdf`). This might be in a file like `idiaval_captain_actions.py`.
    * An example `myagents_IDIAVAL.json` file, pre-configured for the IDIAVAL team composition (1 Captain, 7 Soldiers, 1 Medic, 1 Field Operator).
* **Guides)**:
    * `AIN_iniciar_pygomas_guia.pdf`: **Your primary, detailed guide for setting up the pyGOMAS environment and running any match, including this project.**
    * `memoria_AIN_IDIAVAL.pdf`: A comprehensive report describing the IDIAVAL team's strategy, agent design, and specific implementation details.
    * `Jason_cheatsheet.pdf` (and/or other ASL guides): Useful resources for understanding the AgentSpeak Language (ASL) used to program the agents.
    * `pyGOMAS_Manual_v2025.pdf`: The official manual for the pyGOMAS platform.

## 3. Prerequisites
Before attempting to run this project, you must have a functional pyGOMAS environment. The key prerequisites, detailed in `AIN_iniciar_pygomas_guia.pdf`, are:
* Anaconda installed on your system.
* A working XMPP server (e.g., PyJabber). `AIN_iniciar_pygomas_guia.pdf` explains setting up PyJabber.
* Python: Specific versions are required for different components (e.g., Python 3.7 for the `pygomas` environment, Python 3.10+ for the `pyjabber` environment). Refer to `AIN_iniciar_pygomas_guia.pdf`.

## 4. Setup and Execution Guide

**Important: For complete, step-by-step instructions on installing pyGOMAS, its dependencies, configuring environments, and running a match, please meticulously follow the guidance in `AIN_iniciar_pygomas_guia.pdf`.**

The following steps outline the general process, with specific considerations for the IDIAVAL project:

1.  **Prepare Project Files**:
    * Download and **unzip** the provided `.zip` file(s) containing the IDIAVAL agent code (ASL files), custom Python actions for the Captain, and the example `myagents_IDIAVAL.json`.
    * Organize these files. A suggested structure is provided at the end of this README.

2.  **Set Up Anaconda Environments (refer to `AIN_iniciar_pygomas_guia.pdf` for details)**:
    * Create an Anaconda environment for the XMPP server (e.g., `pyjabber_env`). Install `pyjabber` in it.
    * Create a separate Anaconda environment for pyGOMAS (e.g., `pygomas_env`) using Python 3.7. Install `pygomas` and other dependencies like `windows_curses` (if on Windows) in this environment.

3.  **Integrate IDIAVAL Agents into Your pyGOMAS Setup**:
    * **ASL Files**:
        * Copy the `.asl` files (`CaptainIDIAVAL.asl`, `SoldierIDIAVAL.asl`, `MedicIDIAVAL.asl`, `FieldopIDIAVAL.asl`) into the `pygomas/ASL/` directory of your pyGOMAS installation.
        * Alternatively, you can place them in a custom folder and update their paths within the `myagents_IDIAVAL.json` file (using the `asl` property for each agent type).
    * **Custom Captain Actions (Python Code)**:
        * The `CaptainIDIAVAL` agent relies on custom Python functions/actions (like `.calculatePos` and `.squadForm`) for its formation logic. The Python code for these actions should be in the unzipped files (e.g., in a file like `idiaval_captain_actions.py`).
        * This Python file needs to define a custom agent class (e.g., `CaptainIDIAVALCustom`) that inherits from a base pyGOMAS troop class (like `BDITroop` or `BDISoldier`) and registers these custom actions. Refer to the pyGOMAS Manual (Section 7.6 "Adding new actions") for how to do this.
        * In the `myagents_IDIAVAL.json` file, update the `rank` for the `CaptainIDIAVAL` agent to point to this custom class. The format is `<python_file_name_without_py>.<ClassName>`. For example, if your file is `idiaval_captain_actions.py` and the class is `CaptainIDIAVALCustom`, the rank would be: `"rank": "idiaval_captain_actions.CaptainIDIAVALCustom"`.
    * **Agent Configuration File (`myagents_IDIAVAL.json`)**:
        * Use the provided `myagents_IDIAVAL.json` as your starting point.
        * Verify that it correctly lists 1 `CaptainIDIAVAL` (with the custom rank), 7 `SoldierIDIAVAL` agents, 1 `MedicIDIAVAL`, and 1 `FieldopIDIAVAL`.
        * Ensure agent names are unique if your XMPP server requires it and that ASL paths/ranks are correct.

4.  **Running a Match (follow `AIN_iniciar_pygomas_guia.pdf` for terminal commands and sequence)**:
    * **Step 1: Start your XMPP Server** (e.g., PyJabber) in its own terminal, with its Anaconda environment activated.
    * **Step 2: Start the pyGOMAS Manager** in a new terminal (with `pygomas_env` activated). You'll need to specify your manager JID, service JID, the map (e.g., `map_01`), and the total number of agents participating in the match (`-np` parameter). For the IDIAVAL team (10 agents) playing against a similar team, this would be `-np 20`.
        ```bash
        # Example: Activate pygomas_env and start manager
        # conda activate pygomas_env # (or your pygomas environment name)
        # pygomas manager -jm manager@your_xmpp_server -m map_01 -sj service@your_xmpp_server -np 20
        ```
    * **Step 3: Run the Agents** in another new terminal (with `pygomas_env` activated), using your configured `myagents_IDIAVAL.json` file.
        ```bash
        # Example: Activate pygomas_env and run agents
        # conda activate pygomas_env
        # pygomas run -g myagents_IDIAVAL.json
        ```
    * **Step 4: Start the Renderer** in a final terminal (with `pygomas_env` activated).
        ```bash
        # Example: Activate pygomas_env and start Pygame renderer
        # conda activate pygomas_env
        # pygomas render
        # Or for the text-based renderer:
        # pygomas render --text
        ```

5.  **Troubleshooting Tips**:
    * Double-check all paths in `myagents_IDIAVAL.json`, especially for ASL files and the custom Captain class.
    * Ensure your XMPP server is running correctly and accessible by pyGOMAS components.
    * Confirm you are in the correct Anaconda environment in each terminal and that all dependencies are installed.
    * Consult the `Output/logs` directory in your pyGOMAS folder for error messages.
    * The `AIN_iniciar_pygomas_guia.pdf` also contains troubleshooting information.

## 5. Understanding the Agent Logic and ASL
* To understand the detailed strategy and design of the `CaptainIDIAVAL`, `SoldierIDIAVAL`, `MedicIDIAVAL`, and `FieldopIDIAVAL` agents, please read the **`memoria_AIN_IDIAVAL.pdf`**.
* If you are new to AgentSpeak Language (ASL), the **`Jason_cheatsheet.pdf`** and any other provided ASL materials will be helpful.
* The **`pyGOMAS_Manual_v2025.pdf`** is an essential resource for understanding the pyGOMAS platform itself, including predefined beliefs, actions, and how to develop custom agent functionalities.

## Visual Gameplay Overview

This section provides a visual walkthrough of a game session, showcasing the progression from the initial phase to later stages of the match.

*(The images are displayed in chronological order. Ensure these images are placed in a folder like `gameplay_screenshots/` in your repository. If you haven't renamed the image files to remove spaces, the links below might need URL encoding for spaces, i.e., replacing spaces with `%20`, or it's best to rename them, e.g., to `game_step_1.png`, `game_step_2.png`, etc.)*

**1. Early Game Setup / Initial Deployment**

![Game Screenshot 1: Early Game](./imagenes/Captura%20de%20pantalla%202025-05-01%20221343.png)


**2. Mid-Game Progression**

![Game Screenshot 2: Mid-Game](./imagenes/Captura%20de%20pantalla%202025-05-01%20221416.png)


**3. Tactical Engagement**

![Game Screenshot 3: Engagement](./imagenes/Captura%20de%20pantalla%202025-05-01%20221441.png)


**4. Advancing / Objective Focus**

![Game Screenshot 4: Advancing](./imagenes/Captura%20de%20pantalla%202025-05-01%20221457.png)


**5. Later Stages of the Match**

![Game Screenshot 5: Later Stages](./imagenes/Captura%20de%20pantalla%202025-05-01%20221527.png)


**6. Final Moments / Outcome**

![Game Screenshot 6: Final Moments](./imagenes/Captura%20de%20pantalla%202025-05-01%20221629.png)
