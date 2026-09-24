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
* **Aggressive Alice (Winner):** Secured first place by completing the race in 53 seconds with a maximum speed of 28.0 and 87.2% fuel remaining.

* **ML Max (AI):** Tied the 53-second finish time but reached a significantly higher maximum speed of 31.7, which resulted in a slightly lower remaining fuel capacity of 85.7%.

* **Safe Sam:** Prioritized vehicle stability and fuel economy over pace, finishing in 65 seconds with a top speed restricted to 20.0, leaving a highly efficient 89.8% fuel in the tank. 

# Phase 4: Dynamic Environments and Machine Learning Optimization

The latest iterations of the simulation introduce unpredictable environmental variables and an autonomous learning loop to optimize the AI driver's strategy.

**Randomized Weather Mechanics**

To test vehicle adaptability, the simulation now includes dynamic weather events, specifically unpredictable rain constraints.   

* The automated weather system is designed to trigger a downpour at a random interval between 5 and 45 seconds into the race.   

* Once rain is detected, track safety protocols force all active vehicles to restrict their target speed to a maximum of 14 meters per second.   

**AI Evolution and Training Loop**
The AI vehicle's strategy has been upgraded from a static calculation to an evolutionary learning model.   

* **Genetic Weight Parameter:** The AI now utilizes a mutable ai_weight parameter to continuously map its fuel-to-distance ratio into an optimal speed.   

* **50-Epoch Training Protocol:** Before the official race begins, the AI runs through a 50-epoch training camp simulating a private, dry-weather track.   

* **Algorithmic Mutation:** During each epoch, the system randomly mutates the weight parameter by shifting it up or down by a value between -40 and 40 to explore new pacing strategies.   

* **Optimization:** The algorithm evaluates each run, saving the weight that yields the fastest completion time. In the provided telemetry log, the training successfully concluded with an optimal weight of 314.4.   

**Updated Post-Race Results**

Equipped with its newly learned optimal weight, the AI driver's performance scaled dramatically during the dynamic main event, despite a downpour triggering at the 15-second mark. The final telemetry breakdown is as follows:   

* **ML Max (AI) (Winner):** Leveraged the pre-trained algorithm to finish first in 57 seconds. The vehicle hit a top speed of 31.7 and retained 85.2% of its fuel capacity.   

* **Aggressive Alice:** Dropped to second place, finishing in 60 seconds with a maximum speed of 27.8 and 86.6% fuel remaining.   

* **Safe Sam:** Maintained consistent stability, finishing the race in 68 seconds with a conservative top speed of 19.9 and an industry-leading 89.4% fuel left.
