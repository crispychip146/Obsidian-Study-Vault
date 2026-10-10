---
type: concept
course: cse317
status: active
order: 3
---

# PEAS Framework

> 📖 **Reading Order:** Step 03 of 43 | **Module 2:** Intelligent Agents & Architectures  
> ◄ **Previous:** [[Intelligent Agents and Rationality]] | ► **Next:** [[Environment Characterization in AI]]

---

## Starting Point and the Problem

Before designing an intelligent agent, an engineer or researcher must formally specify the operational environment and task requirements. Without a rigorous specification, it is impossible to evaluate whether an agent is rational or whether its hardware (sensors and actuators) is sufficient. The **PEAS framework** provides the standard, structured methodology to specify an agent's task environment.

---

## Developing the Idea

To design any system—whether an automated taxi driver, an online book recommendation engine, or an automated medical diagnosis bot—we must answer four fundamental questions:
1. What counts as success? (**Performance Measure**)
2. What world or context does it operate in? (**Environment**)
3. What physical or software mechanisms execute decisions? (**Actuators**)
4. What sensory inputs provide data about the state? (**Sensors**)

```
                               ┌──────────────────────────┐
                               │           PEAS           │
                               │  Task Environment Spec  │
                               └─────────────┬────────────┘
                                             │
             ┌─────────────────┬─────────────┴───┬──────────────────┐
             ▼                 ▼                 ▼                  ▼
     ┌───────────────┐ ┌───────────────┐ ┌───────────────┐  ┌───────────────┐
     │  PERFORMANCE  │ │  ENVIRONMENT  │ │   ACTUATORS   │  │    SENSORS    │
     │    MEASURE    │ │               │ │               │  │               │
     │  What defines │ │ The external  │ │ Mechanisms to │  │ Hardware to   │
     │   success?    │ │     world     │ │ modify state  │  │ read state    │
     └───────────────┘ └───────────────┘ └───────────────┘  └───────────────┘
```

---

## Definition

The **PEAS Framework** is an architectural specification standard that groups the properties of an agent's task environment into:
- **P — Performance Measure:** The external metric used to evaluate how successful an agent's behavior is.
- **E — Environment:** The external context, physical space, or virtual domain in which the agent operates.
- **A — Actuators:** The output devices, effectors, or software APIs through which the agent carries out actions.
- **S — Sensors:** The input devices, cameras, probes, or software feeds through which the agent perceives percepts.

---

## How It Works

### Detailed Case Studies

#### 1. Automated Taxi Driver
- **Performance Measure:** Safe arrival, fast travel time, legal compliance (avoid traffic violations), passenger comfort, profit maximization.
- **Environment:** Urban roads, highways, traffic, pedestrians, weather conditions, road construction, customers.
- **Actuators:** Steering wheel, accelerator pedal, brakes, turn signals, horn, display screen/speech interface.
- **Sensors:** Cameras, LiDAR, radar, GPS, speedometer, odometer, accelerometer, engine sensors, microphone.

#### 2. Medical Diagnosis System
- **Performance Measure:** Healthy patient outcomes, minimized treatment costs, zero misdiagnoses, fast diagnostic turnaround.
- **Environment:** Patients, hospital staff, laboratory facilities, medical histories, disease database.
- **Actuators:** Display screen for test recommendations, treatment plans, prescriptions, diagnostic reports.
- **Sensors:** Keyboard entry of patient symptoms, laboratory test results, doctor's clinical notes.

#### 3. Satellite Image Analysis System
- **Performance Measure:** High classification accuracy of terrain/vegetation, correct identification of environmental anomalies, minimized false positives.
- **Environment:** Downlink orbital satellite imagery, atmospheric variations, geographic areas.
- **Actuators:** Display of annotated geographic maps, alerts sent to environmental agencies.
- **Sensors:** Multi-spectral high-resolution optical cameras, infrared and radar sensors.

#### 4. Automated Part-Picking Robot
- **Performance Measure:** Percentage of parts sorted into correct bins, throughput speed, zero mechanical damage to fragile components.
- **Environment:** Conveyor belt, sorting bins, varied mechanical parts.
- **Actuators:** Robotic jointed arm, mechanical gripper, vacuum suction head.
- **Sensors:** Pixel camera array, infrared proximity sensors, joint angle encoders.

---

## Summary Matrix

| Agent Type | Performance Measure | Environment | Actuators | Sensors |
|---|---|---|---|---|
| **Automated Taxi** | Safety, speed, legality, comfort, profit | Roads, traffic, pedestrians, weather | Steering, accelerator, brake, signals, horn | Cameras, LiDAR, radar, GPS, speedometer |
| **Medical Diagnosis** | Healthy patient, low cost, accuracy | Patients, hospital, disease database | Screen display, prescription generation | Keyboard/GUI input of symptoms, lab tests |
| **Satellite Analysis** | Classification accuracy, anomaly detection | Downlink satellite imagery | Annotated maps, alert dispatch | Optical & multi-spectral satellite cameras |
| **Part-Picking Robot** | Sorting accuracy, throughput, low damage | Conveyor belt, bins, parts | Jointed robotic arm, pneumatic gripper | Video cameras, proximity sensors, angle encoders |
| **Spam Filter** | High true positive spam catch, zero false positives | Incoming email stream, user inbox | Move email to spam folder, alert user | Email headers, message body text, IP metadata |

---

## Common Mistakes

- **Confusing Actuators with Actions:** An actuator is the physical *tool* or mechanism (e.g., steering wheel, motor), whereas an action is the *choice* (e.g., turn left 15 degrees).
- **Subjective vs. Objective Performance:** Specifying performance from the agent's internal comfort rather than real-world utility.

---

## Exam Relevance

Typical exam problems ask students to:
- Formulate a full PEAS description for an unfamiliar automated system (e.g., an automated agricultural harvesting drone, an online auction bidder).
- Identify vulnerabilities in a proposed performance measure.

---

## Related Concepts

- [[Intelligent Agents and Rationality]]
- [[Environment Characterization in AI]]
- [[Agent Architectures]]

---

## Prerequisites

- [[Intelligent Agents and Rationality]]

---

## Navigation

◄ **Previous:** [[Intelligent Agents and Rationality]] | ► **Next:** [[Environment Characterization in AI]]
