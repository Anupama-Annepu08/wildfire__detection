A Real-Time Priority Scheduling Framework for Autonomous Wildfire Detection and Response Coordination

RTOS Project Review-1 | Department of Computer Science and Engineering | KLH, KL University

Overview

Wildfires spread rapidly and unpredictably, and delayed detection or poorly coordinated emergency response often results in preventable loss of life and property. Most existing wildfire monitoring systems rely on periodic sensor polling with no formal guarantee on how quickly a genuine fire alert is actually processed — meaning a critical alert can get silently delayed behind routine, low-priority tasks under real sensor load.

It is a real-time task scheduling framework that treats routine sensor monitoring as low-priority periodic tasks and fire-detection events as high-priority tasks with strict response deadlines. It applies classical real-time scheduling theory — Rate-Monotonic Scheduling (RMS) and Earliest-Deadline-First (EDF) — to guarantee timely response, and explicitly detects and resolves priority inversion using priority inheritance.

Unlike most wildfire detection projects, which focus on detection accuracy, BLAZE-RT focuses on guaranteed response timing — proving that once a fire is detected, the system reacts within a bounded, predictable time, even under heavy load.

Problem Statement
Wildfires spread fast; even small delays in detection or response can be costly.
Existing systems poll sensors periodically but give no timing guarantee on alert processing.
Under heavy sensor load, urgent fire alerts can be delayed behind routine checks.
Research question: How do we guarantee an urgent fire alert is handled within a fixed time limit, even when the system is busy?
Objectives
Design BLAZE-RT, a scheduling framework separating routine sensor monitoring (low priority) from fire-detection response (high priority).
Implement and compare Rate-Monotonic Scheduling (RMS) and Earliest-Deadline-First (EDF) for this task set.
Detect and resolve priority inversion using priority inheritance.
Simulate realistic wildfire spread using a Cellular Automaton model.
Integrate a physical sensor layer (Arduino-based) for live data.
Evaluate the system using deadline-miss rate, response latency, and system utilization, against a naive First-Come-First-Served (FCFS) baseline.
System Architecture
Physical Sensors (Flame / DHT11)
        │
Temperature / Flame Readings ──► Serial (USB) ──► Python Serial Reader
        │                                                │
Simulated Fire-Spread Grid (Cellular Automaton) ──────────┤
        │                                                │
        ▼                                                ▼
              Real-Time Scheduler (RMS / EDF)
                          │
              Priority / Deadline Decision
                          │
              Priority Inversion Handler
              (Priority Inheritance)
                          │
                    Task Execution
                          │
                 Alert / Response Dispatch
                          │
                    Live Visual Dashboard
                 (task state, deadline-miss log)
Tech Stack
Layer	Tools / Technologies
Core language	Python
Scheduling core	Custom Task / Scheduler classes (RMS, EDF)
Simulation	Cellular Automaton fire-spread model
Hardware	Arduino Uno, Flame sensor, DHT11 (temperature/humidity), Buzzer, 16x2 LCD
Hardware ↔ software link	Serial communication (pyserial)
Visualization	pygame / matplotlib (live dashboard)
Development aids	Claude / ChatGPT (planning, debugging, documentation)

BLAZE-RT's core scheduling logic (RMS, EDF, priority inheritance) is deterministic real-time systems theory, not AI/ML. AI tools listed above were used only as development aids during building and documentation, not as a runtime component of the scheduler itself.

Key Concepts
Periodic Tasks — routine sensor checks, released at fixed intervals (e.g., every 500ms).
Aperiodic Tasks — fire-detection alerts, triggered on demand with a hard deadline.
Rate-Monotonic Scheduling (RMS) — shorter-period tasks get higher priority; used for periodic sensor checks.
Earliest-Deadline-First (EDF) — the task with the nearest deadline runs first; used for urgent fire alerts.
Priority Inversion — a low-priority task blocks a high-priority task from running.
Priority Inheritance — the fix: temporarily raise the blocking task's priority so it finishes quickly and releases the resource.
Evaluation Metrics
Deadline-Miss Rate — proportion of tasks that fail to complete within their deadline.
Response Latency — time between fire detection and system response.
System Utilization — how efficiently the scheduler uses available processing time.

Comparisons are made between BLAZE-RT (RMS/EDF scheduling) and a naive FCFS baseline, under varying sensor-load conditions.

Project Structure (planned)
blaze-rt/
├── scheduler/
│   ├── task.py            # Task, PeriodicTask, AperiodicTask classes
│   ├── scheduler.py       # RateMonotonicScheduler, EDFScheduler
│   └── priority_inheritance.py
├── simulation/
│   ├── fire_grid.py       # Cellular automaton fire-spread model
│   └── clock.py           # Simulation clock
├── hardware/
│   ├── arduino_sketch/    # Arduino code for sensors + LCD + buzzer
│   └── serial_reader.py   # pyserial bridge to Python scheduler
├── dashboard/
│   └── visual_dashboard.py
├── evaluation/
│   └── metrics.py         # deadline-miss rate, latency, utilization
├── docs/
│   └── report/
└── README.md
Novelty
Applies formal real-time scheduling guarantees (RMS/EDF) to wildfire response — most existing systems focus only on detection accuracy, not response timing.
Explicitly models and resolves priority inversion in a wildfire-response context, using priority inheritance.
Combines a low-cost physical sensor layer with a software-simulated fire-spread grid, keeping the system realistic yet inexpensive.
Live dashboard makes scheduling decisions and deadline compliance visible and demonstrable, not just theoretical.
Reproducible blueprint, extensible to other safety-critical, resource-constrained edge monitoring domains beyond wildfire.
Project Timeline
Phase	Activity	Duration
Phase 1	Literature survey, requirement analysis, finalizing scheduling approach	Week 1-2
Phase 2	Implementing RTOS scheduler core (Task/Scheduler classes, priority inversion handling)	Week 3-4
Phase 3	Fire-spread simulation, Arduino sensor integration, live dashboard	Week 5-6
Phase 4	Testing, evaluation, report writing, final presentation prep	Week 7-8
Team
Under the guidance of: Dr. Srikanth Cherukuvada, Assistant Professor, Dept. of CSE, KLH CSE Bowrampet Campus
Team members: [add names and roll numbers]
References
Kalyanasundaram et al., A Survey on Scheduling Algorithms in Real-Time Systems — comparative analysis of RM and EDF scheduling.
Avazov et al., An Edge Computing Environment for Early Wildfire Detection — YOLOv5-based wildfire detection on edge hardware.
Status

🚧 In development — RTOS Project Review-1 stage.
