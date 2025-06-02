# Project IDIAVAL - Coordinated Intelligent Agents for pyGOMAS Capture The Flag

## 1. Overview
This project implements "IDIAVAL", a team of coordinated intelligent agents designed to play Capture The Flag (CTF) in the pyGOMAS environment. The team consists of a Captain, Soldiers, a Medic, and a Field Operator, each with specialized roles and behaviors. The strategy, detailed in `memoria_AIN_IDIAVAL.pdf`[cite: 1], emphasizes arc formations[cite: 6, 16], anti-friendly fire logic[cite: 2, 3, 5], proactive support (healing and ammunition)[cite: 4, 5, 19, 20], and coordinated resource management[cite: 2, 4, 5, 21].

## 2. About This Repository
This repository contains:
* **Agent Implementations (`.zip` file(s))**: You should find one or more zipped archives. Please **unzip them**. They are expected to contain:
    * AgentSpeak (`.asl`) files for `CaptainIDIAVAL`, `SoldierIDIAVAL`, `MedicIDIAVAL`, and `FieldopIDIAVAL` agents.
    * Python code for custom actions/functions used by `CaptainIDIAVAL` (e.g., for formation calculations like `.calculatePos` and `.squadForm` mentioned in `memoria_AIN_IDIAVAL.pdf` [cite: 6, 7, 8]). This might be in a file like `idiaval_captain_actions.py`.
    * An example `myagents_IDIAVAL.json` file, pre-configured for the IDIAVAL team composition (1 Captain, 7 Soldiers, 1 Medic, 1 Field Operator [cite: 2, 3, 4, 5]).
* **Guides (PDFs, typically in a `/Guides` folder or alongside this README)**:
    * `AIN_iniciar_pygomas_guia.pdf`: **Your primary, detailed guide for setting up the pyGOMAS environment and running any match, including this project.**
    * `memoria_AIN_IDIAVAL.pdf`: A comprehensive report describing the IDIAVAL team's strategy, agent design, and specific implementation details[cite: 1].
    * `Jason_cheatsheet.pdf` (and/or other ASL guides): Useful resources for understanding the AgentSpeak Language (ASL) used to program the agents[cite: 47].
    * `pyGOMAS_Manual_v2025.pdf`: The official manual for the pyGOMAS platform[cite: 68].

## 3. Prerequisites
Before attempting to run this project, you must have a functional pyGOMAS environment. The key prerequisites, detailed in `AIN_iniciar_pygomas_guia.pdf`, are:
* Anaconda installed on your system[cite: 31, 236].
* A working XMPP server (e.g., PyJabber). `AIN_iniciar_pygomas_guia.pdf` explains setting up PyJabber[cite: 31, 33, 227].
* Python: Specific versions are required for different components (e.g., Python 3.7 for the `pygomas` environment[cite: 237, 238], Python 3.10+ for the `pyjabber` environment [cite: 227]). Refer to `AIN_iniciar_pygomas_guia.pdf`.

## 4. Setup and Execution Guide

**Important: For complete, step-by-step instructions on installing pyGOMAS, its dependencies, configuring environments, and running a match, please meticulously follow the guidance in `AIN_iniciar_pygomas_guia.pdf`.**

The following steps outline the general process, with specific considerations for the IDIAVAL project:

1.  **Prepare Project Files**:
    * Download and **unzip** the provided `.zip` file(s) containing the IDIAVAL agent code (ASL files), custom Python actions for the Captain, and the example `myagents_IDIAVAL.json`.
    * Organize these files. A suggested structure is provided at the end of this README.

2.  **Set Up Anaconda Environments (refer to `AIN_iniciar_pygomas_guia.pdf` for details)**:
    * Create an Anaconda environment for the XMPP server (e.g., `pyjabber_env`)[cite: 31, 32]. Install `pyjabber` in it[cite: 33].
    * Create a separate Anaconda environment for pyGOMAS (e.g., `pygomas_env`) using Python 3.7[cite: 36, 37, 238]. Install `pygomas` and other dependencies like `windows_curses` (if on Windows) in this environment[cite: 37, 38, 239].

3.  **Integrate IDIAVAL Agents into Your pyGOMAS Setup**:
    * **ASL Files**:
        * Copy the `.asl` files (`CaptainIDIAVAL.asl`, `SoldierIDIAVAL.asl`, `MedicIDIAVAL.asl`, `FieldopIDIAVAL.asl`) into the `pygomas/ASL/` directory of your pyGOMAS installation[cite: 212].
        * Alternatively, you can place them in a custom folder and update their paths within the `myagents_IDIAVAL.json` file (using the `asl` property for each agent type)[cite: 204].
    * **Custom Captain Actions (Python Code)**:
        * The `CaptainIDIAVAL` agent relies on custom Python functions/actions (like `.calculatePos` and `.squadForm` [cite: 6, 7, 8]) for its formation logic. The Python code for these actions should be in the unzipped files (e.g., in a file like `idiaval_captain_actions.py`).
        * This Python file needs to define a custom agent class (e.g., `CaptainIDIAVALCustom`) that inherits from a base pyGOMAS troop class (like `BDITroop` or `BDISoldier`) and registers these custom actions. Refer to the pyGOMAS Manual (Section 7.6 "Adding new actions" [cite: 375, 376]) for how to do this.
        * In the `myagents_IDIAVAL.json` file, update the `rank` for the `CaptainIDIAVAL` agent to point to this custom class. The format is `<python_file_name_without_py>.<ClassName>`[cite: 382, 383]. For example, if your file is `idiaval_captain_actions.py` and the class is `CaptainIDIAVALCustom`, the rank would be: `"rank": "idiaval_captain_actions.CaptainIDIAVALCustom"`.
    * **Agent Configuration File (`myagents_IDIAVAL.json`)**:
        * Use the provided `myagents_IDIAVAL.json` as your starting point.
        * Verify that it correctly lists 1 `CaptainIDIAVAL` (with the custom rank), 7 `SoldierIDIAVAL` agents, 1 `MedicIDIAVAL`, and 1 `FieldopIDIAVAL`[cite: 2, 3, 4, 5].
        * Ensure agent names are unique if your XMPP server requires it and that ASL paths/ranks are correct.

4.  **Running a Match (follow `AIN_iniciar_pygomas_guia.pdf` for terminal commands and sequence)**:
    * **Step 1: Start your XMPP Server** (e.g., PyJabber) in its own terminal, with its Anaconda environment activated[cite: 33, 35].
    * **Step 2: Start the pyGOMAS Manager** in a new terminal (with `pygomas_env` activated). You'll need to specify your manager JID, service JID, the map (e.g., `map_01`), and the total number of agents participating in the match (`-np` parameter)[cite: 38, 246]. For the IDIAVAL team (10 agents [cite: 2, 3, 4, 5]) playing against a similar team, this would be `-np 20` (as seen in an example in `AIN_iniciar_pygomas_guia.pdf` [cite: 38]).
        ```bash
        # Example: Activate pygomas_env and start manager
        # conda activate pygomas_env # (or your pygomas environment name)
        # pygomas manager -jm manager@your_xmpp_server -m map_01 -sj service@your_xmpp_server -np 20
        ```
    * **Step 3: Run the Agents** in another new terminal (with `pygomas_env` activated), using your configured `myagents_IDIAVAL.json` file[cite: 38, 251].
        ```bash
        # Example: Activate pygomas_env and run agents
        # conda activate pygomas_env
        # pygomas run -g myagents_IDIAVAL.json
        ```
    * **Step 4: Start the Renderer** in a final terminal (with `pygomas_env` activated)[cite: 45, 255].
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
* To understand the detailed strategy and design of the `CaptainIDIAVAL`, `SoldierIDIAVAL`, `MedicIDIAVAL`, and `FieldopIDIAVAL` agents, please read the **`memoria_AIN_IDIAVAL.pdf`**[cite: 1].
* If you are new to AgentSpeak Language (ASL), the **`Jason_cheatsheet.pdf`** [cite: 47] and any other provided ASL materials will be helpful.
* The **`pyGOMAS_Manual_v2025.pdf`** [cite: 68] is an essential resource for understanding the pyGOMAS platform itself, including predefined beliefs, actions, and how to develop custom agent functionalities.

## 6. Suggested Project File Structure (after unzipping)
To keep things organized, you might structure your IDIAVAL project files like this:
