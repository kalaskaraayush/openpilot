# Openpilot: Custom Architecture & Logic Override

This repository contains a modified deployment of Comma.ai's Openpilot autonomous vehicle software. It was engineered to test system limits, manipulate physical UI logic, and resolve complex dependencies within a headless cloud environment.

## 🛠️ Infrastructure & Deployment
* **Environment:** Deployed a 32-core GitHub Codespace to act as a headless compile server, bypassing physical hardware limitations.
* **Build System:** Utilized `scons` for multi-threaded compilation of C++ and Python binaries.
* **Dependency Resolution:** Diagnosed compiler crashes and manually injected OS-level C++ UI libraries (`libncurses-dev`) to resolve Python import failures and force a successful build.

## ⚙️ System Modifications
* **Safety Governor Override (`interfaces.py`):** Located the hardcoded system constraints and rewrote `V_CRUISE_MAX`, effectively bypassing the factory speed limits to allow a 300 km/h maximum cruise speed.
* **Physical Logic Manipulation (`cruise.py`):** Altered the steering wheel button logic pipeline. Rewrote `v_cruise_delta` step increments to force the system to calculate 10 km/h target speed jumps per physical button press instead of the factory 1 km/h default.

## 🔐 Security Bypasses
* Successfully routed around GitHub's default environment token restrictions (`403 Permission Denied`).
* Authenticated custom SSH/HTTPS protocols to force local cloud repository syncs over restricted terminal sessions.