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

# Phase 3: Autonomous Race Simulation and Telemetry

This phase executes the multi-vehicle racing environment, tracking driver performance in real-time and generating the final statistical output. The simulation pipeline actively monitors three distinct vehicles from the starting line through to the finish flag. It calculates real-time metrics including elapsed time, peak speeds, and fuel efficiency to determine the final rankings.   

**Official Post-Race Results**

The telemetry system captures the behavioral trade-offs of each automated driving profile. Based on the official post-race telemetry report, the performance breakdown is as follows:   
* **Aggressive Alice (Winner): Secured first place by completing the race in 53 seconds with a maximum speed of 28.0 and 87.2% fuel remaining.

* **ML Max (AI): Tied the 53-second finish time but reached a significantly higher maximum speed of 31.7, which resulted in a slightly lower remaining fuel capacity of 85.7%.

* **Safe Sam: Prioritized vehicle stability and fuel economy over pace, finishing in 65 seconds with a top speed restricted to 20.0, leaving a highly efficient 89.8% fuel in the tank. 
