# Sentinel-Drive-Simulation

A Python autonomous vehicle simulation featuring P-controller physics, a real-time cybersecurity firewall (IDS) protecting against sensor-spoofing attacks, and a genetic evolutionary training algorithm.

# Phase 1: Core Physics & Object-Oriented Architecture
This initial upload establishes the foundational physics engine for the vehicle simulation. It tracks basic kinematics (speed, distance, fuel burn) and transitions the raw procedural time-loop into a robust Object-Oriented Programming (OOP) architecture. By encapsulating the vehicle data into a dynamic RaceCar class, the simulation can now instantiate and track multiple independent vehicles simultaneously on the same track.

# Phase 2: Cyber-Physical System Simulation: Autonomous Race Car & Firewall 🏎️🛡️

This Phase is a Python-based cyber-physical system (CPS) simulation that models an autonomous race car navigating a track while under a cyber attack. It demonstrates the critical danger of sensor spoofing in autonomous vehicles and showcases a physics-based Intrusion Detection System (IDS) / Firewall designed to prevent catastrophic crashes.

**Core Features**

* **Cyber Attack Simulation (Sensor Spoofing):** The simulation introduces a vulnerability where hackers can spoof the car's raw sensor input by injecting a `sensor_spoof_offset`. This causes the car's internal logic to miscalculate its actual speed.


* **Physics-Based Firewall:** Cars equipped with the firewall (`has_firewall=True`) feature a built-in memory of their `last_trusted_speed`. The firewall performs a sanity check on incoming data: if the perceived speed changes by an impossible physics metric (e.g., $> 15$ m/s in a single tick), the firewall blocks the attack, drops the hacked data, and safely acts on the last known reality.


* **Proportional Control Actuation:** The car adjusts its acceleration using a Proportional (P) controller ($speed\_change = K_p \times error$), mimicking real-world robotic actuation based on the difference between target speed and perceived speed.


* **Crash Dynamics & Logging:** If the firewall fails or is absent and the car accelerates beyond 40 m/s, a critical failure is triggered, resulting in a crash. All telemetry (time, speed, fuel) is logged internally for post-race analysis and visualization.
