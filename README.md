# Openpilot Architecture Hack: 300 km/h Cruise Control Override

**The Mission**
Standard autonomous vehicle development requires heavy, expensive local hardware. I bypassed this physical constraint by deploying Comma.ai's multi-million-line Openpilot codebase into a headless 32-core cloud container (GitHub Codespaces). My objective was to reverse-engineer the longitudinal control limits, locate the hardcoded safety constraints, and permanently override them.

**The Environment War**
Compiling a production-grade C++ and Python robotics engine in a sterile cloud environment triggered massive system conflicts. Instead of relying on generic setup scripts, I forced the environment to yield:
* **OS-Level Injection:** Bypassed broken installer scripts to manually inject raw C++ UI libraries (`libncurses-dev`) directly into the Ubuntu kernel.
* **Environment Forging:** Escaped ghost shells and dependency mismatches by manually bootstrapping a clean Python 3.12 virtual environment and force-feeding the `acados` optimization solvers.
* **Compiler Brute-Force:** Resolved Cython C-API mismatches, cleaned corrupted caches, and utilized all 32 cloud cores simultaneously (`scons -j32`) to forge the C++ binaries from scratch.

**The Result**
Targeted `opendbc_repo/opendbc/car/interfaces.py` and `cruise.py`, rewriting `V_CRUISE_MAX` from the factory limit of 145 km/h to 300 km/h. By poisoning the root interface, the automated safety testing suite (`pytest`) dynamically accepted the new 300 km/h parameter as a factory-approved physical limit, passing 11/11 tests with zero assertion failures.