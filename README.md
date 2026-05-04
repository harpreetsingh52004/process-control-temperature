# Process Control — Pressure, Flow, Level, Temperature

> Nine labs across the four core process variables. Calibration, PID tuning, RTD measurement, and step-response analysis — on a real Lab-Volt-style training system.

![Status](https://img.shields.io/badge/status-complete-brightgreen)
![Course](https://img.shields.io/badge/course-PCS455-blue)
![Domain](https://img.shields.io/badge/domain-process%20control-green)
![Sensor](https://img.shields.io/badge/sensor-RTD%20Pt100-purple)

This repo collects deliverables from **PCS455 — Process Control** at Seneca Polytechnic. The course covered the four core process variables — pressure, flow, level, temperature — with hands-on labs on real instrumentation, including pump/heater control, RTD calibration, and PID tuning from step-response data.

---

## What's inside

### Lab progression

| Lab | Process Variable | Focus |
| --- | --- | --- |
| 1 | — | Familiarization with the training system |
| 2–3 | Pressure | Measurement Pt 1 + Pt 2 |
| 4–5 | Flow | Measurement Pt 1 + Pt 2 |
| 6 | Level | Measurement |
| 7–8 | Temperature | Measurement Pt 1 + Pt 2 |
| 9 | Temperature | Characterization (step response, FOPDT fit) |

### Final deliverable — temperature loop

The capstone of the course was a **closed-loop temperature control system**:

1. **Sensor** — RTD (Pt100), 3-wire wiring to eliminate lead-resistance error, signal-conditioned to a clean analog input
2. **Calibration** — ice-point and boiling-point checks against a reference; offset and gain corrected at the controller
3. **Open-loop step response** — recorded the full PV trajectory after a step input
4. **FOPDT fit** — first-order-plus-dead-time model fit in MATLAB to extract gain, time constant, dead time
5. **PID tuning** — Cohen-Coon as a starting point, refined to balance overshoot vs. settling time
6. **Heater + fan output** — fan provides active cooling so the loop can recover from overshoot or step-down without waiting for ambient

```
              ┌──────────┐
              │ Setpoint │
              └────┬─────┘
                   ▼
   ┌───┐   error  ┌─────┐  output  ┌──────────┐  PV
   │ Σ │────────► │ PID │────────► │ Heater   │ ─────┐
   └─▲─┘          └─────┘          │  + Fan   │      │
     │                             └──────────┘      │
     │                                               ▼
     │             ┌──────────────────────┐      ┌──────┐
     └─────────────│  RTD signal cond.    │◄─────│ Pt100│
                   └──────────────────────┘      └──────┘
```

---

## Tech Stack

| Layer | Technology |
| --- | --- |
| **Sensor** | RTD — Pt100 (3-wire) with signal conditioner |
| **Controller** | PID (parallel form), tuned from data |
| **Actuators** | PWM-driven heater, fan |
| **Analysis** | MATLAB — step-response capture, FOPDT model fit |
| **Tuning method** | Cohen-Coon starting point, refined by inspection |
| **Other PVs** | Pressure transmitters, flow meters (turbine + magnetic), level sensors (capacitive + ultrasonic) |

---

## What I learned

- **PID tuning is much faster when you measure first.** A single open-loop step gives you the time constant and dead time. From those, every common tuning rule lands in the right ballpark.
- **Dead time is the silent killer.** Most of the tuning effort accommodates dead time, not gain or integral action. Once dead time was identified, the move was to back off proportional gain — not push it harder.
- **Calibration is non-negotiable.** A 2 °C offset at the sensor turns into 2 °C steady-state error no amount of integral action will remove. Fix the measurement first.
- **Two-sided plants are easier.** Adding a fan turned heat-only into heat + cool — more aggressive control without overshoot punishing disturbances.
- **3-wire RTD wiring is worth the extra cable.** Lead-resistance compensation matters in any installation longer than a meter or two. The labs that skipped this were the ones with mysterious 1–2 ° errors.

---

## Repo contents

```
.
├── README.md
├── temperature/
│   ├── step-response-data.csv
│   ├── matlab/
│   │   ├── fopdt_fit.m
│   │   └── step_response_plot.m
│   ├── pid-config.md
│   └── calibration-record.md
├── pressure/             # Lab 2-3 deliverables
├── flow/                 # Lab 4-5 deliverables
├── level/                # Lab 6 deliverables
├── labs/                 # PCS455 lab PDFs (1–9)
└── docs/
```

---

📫 **Harpreet Singh** — [harpreetsingh.cloud](https://harpreetsingh.cloud) · [GitHub](https://github.com/harpreetsingh52004)
