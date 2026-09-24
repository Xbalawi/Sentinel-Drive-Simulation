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


# Phase 5: Autonomous Cyber-Physical Race Car Simulation

**The File "PID_Contorller_Simulation"** that combines classical control systems, machine learning parameter optimization, cybersecurity defenses, and telemetry analysis.

---

## 1. Core Architecture & Components

### 🧠 The Strategy Engine & RaceCar Blueprint

The `RaceCar` class simulates a vehicle driving along a 1000m track with realistic speed caps, fuel consumption, dynamic weather adjustments, and crash conditions (speed $> 40\text{ m/s}$):

* **Aggressive Strategy ("Aggressive Alice"):** Targets $28\text{ m/s}$ while fuel $> 15\%$, dropping to $12\text{ m/s}$ otherwise.


* **Safe Strategy ("Safe Sam"):** Targets a conservative $20\text{ m/s}$ while fuel $> 10\%$, dropping to $12\text{ m/s}$ otherwise.


* **AI Strategy ("ML Max"):** Dynamically calculates target speed based on remaining distance and available fuel:

$$\text{calculated\speed} = \left(\frac{\text{fuel}}{\max(1, 1000 - \text{distance})}\right) \times \text{ai\weight}$$



The target speed is bounded between $10\text{ m/s}$ and $32\text{ m/s}$.


* **Rain Condition:** During downpours, maximum target speed across all strategies is capped at $14\text{ m/s}$.



---

### 🧮 PID Speed Controller

Instead of instantaneously changing speed, cars adjust velocity using a **Proportional-Integral-Derivative (PID)** controller:

1. **Proportional Error (Present):** $\text{error} = \text{target\speed} - \text{perceived\speed}$

2. **Integral Error (Past):** $\text{integral\error} = \sum \text{error}$ (clamped between $-50$ and $50$ for anti-windup protection)


3. **Derivative Error (Future):** $\text{derivative\error} = \text{error} - \text{previous\error}$


The throttle/brake command is calculated with gains $K_p = 0.3$, $K_i = 0.05$, and $K_d = 0.1$:


$$\text{speed\-change} = (0.3 \cdot \text{error}) + (0.05 \cdot \text{integral\error}) + (0.1 \cdot \text{derivative\error})$$

---

### 🛡️ Cybersecurity & Firewall Protection

The simulation includes a sensor spoofing cyber-attack:

* At $t = 15\text{s}$, a spoofing attack introduces a $25\text{ m/s}$ sensor offset to Safe Sam.


* Vehicles with `has_firewall = True` evaluate the raw speed delta ($\Delta = \vert{}\text{raw\sensor\speed} - \text{last\trusted\speed}\vert{}$). If $\Delta > 15\text{ m/s}$, the attack is identified, the spoofing offset is discarded, and the car falls back to its last trusted speed.



---

## 2. AI Evolutionary Training Protocol

Before the race, `train_ai(epochs=50)` runs a genetic mutation loop over 50 dry-weather trial races to optimize `ai_weight`:

1. **Mutation:** Shifts `best_weight` by a random offset $\in [-40, 40]$.


2. **Evaluation:** Runs a 1000m test drive.


3. **Selection:** Updates the record weight if the car completes the track in less time.



**Training Progression Output:**

* **Epoch 01:** Weight $150.0 \rightarrow 43\text{s}$ finish ($86.0\%$ fuel)


* **Epoch 05:** Weight $180.1 \rightarrow 38\text{s}$ finish ($86.0\%$ fuel)


* **Epoch 14:** Weight $228.0 \rightarrow 34\text{s}$ finish ($85.5\%$ fuel)


* **Epoch 24:** Weight $318.8 \rightarrow 31\text{s}$ finish ($84.1\%$ fuel)


* **Optimal AI Weight Found:** **`318.8`**


---

## 3. Main Event & Post-Race Telemetry

During the live race event:

* A random rain event hit at $t = 9\text{s}$, capping track speed limits to $14\text{ m/s}$.


* Safe Sam successfully absorbed the $25\text{ m/s}$ cyber attack at $t = 15\text{s}$ thanks to its firewall.


* ML Max used its learned weight ($318.8$) to balance acceleration prior to the rain and maintain optimum power output during track limit changes.



### 🏆 Telemetry Standings

| Driver | Status | Finish Time | Max Speed | Fuel Remaining |
| --- | --- | --- | --- | --- |
| **ML Max (AI)** 🥇 | FINISHED | **62s** | $36.5\text{ m/s}$ | $82.8\%$ |
| **Aggressive Alice** | FINISHED | **64s** | $32.9\text{ m/s}$ | $84.5\%$ |
| **Safe Sam** | FINISHED | **68s** | $24.2\text{ m/s}$ | $88.3\%$ |
