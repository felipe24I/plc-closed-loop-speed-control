# Closed-Loop Speed Control using Siemens PLC S7-1200 and S7-1500

Implementation of a closed-loop speed control system for a three-phase motor using industrial PLCs and a PID controller.

## Overview

This project implements a closed-loop speed control architecture using a master–slave PLC configuration. The system regulates the motor speed using a PID controller and real-time feedback from an external speed measurement system.

## System Architecture
<img width="1134" height="520" alt="image" src="https://github.com/user-attachments/assets/486a4789-b8cf-43f9-bec1-5eeb1a40ed89" />

* **Master PLC:** Siemens S7-1500 – setpoint generation, monitoring, and system supervision
* **Slave PLC:** Siemens S7-1200 – motor control and PID execution
* **Drive:** SINAMICS V20 variable frequency drive
* **Motor:** Three-phase induction motor
* **Speed Measurement:** Arduino with LM393 IR sensor for RPM measurement
* **HMI:** Operator interface for setpoint control and monitoring

## Control Strategy

The control loop is implemented using the **PID Compact block** in TIA Portal.

* Inputs: motor speed (RPM) and desired setpoint
* Signals are normalized to **0–100%** using scaling blocks
* The PID output generates the analog control signal for the VFD
* The controller runs in a **cyclic interrupt block** for deterministic timing

**PID Compact block**

<img width="377" height="369" alt="image" src="https://github.com/user-attachments/assets/0377f1ce-b9fd-4a08-acf9-397f155c8b5a" />

**setpoint and the PID output (control signal) are normalized and scaled for proper operation.**

<img width="532" height="358" alt="image" src="https://github.com/user-attachments/assets/db8f6482-b64f-4329-a042-36cc06671fe6" />

**PID parameters**

They were sellected empirically

<img width="387" height="254" alt="image" src="https://github.com/user-attachments/assets/2d4e1086-025d-46cc-8f5c-f09ca7d67305" />

**PID behaviour**

<img width="626" height="402" alt="image" src="https://github.com/user-attachments/assets/cbea727f-c5d9-4cb0-9098-b19364266c2f" />

**green line:** Real value (sensor)

**red line:** PID output (control signal)

**black line:** Desired value (setpoint)

## Communication

The two PLCs exchange data using **TSEND/TRCV communication blocks**, allowing transmission of:

* START/STOP commands
* Speed setpoint
* Measured motor speed

### Master segments Siemens S7-1500

**Segment 1 – TSEND_C Block**

<img width="398" height="295" alt="image" src="https://github.com/user-attachments/assets/afa64cb3-d22b-43c6-ad1f-9c880cb8ad0c" />

It is sending start and stop data from the master PLC to slave PLC.

**Segment 2 – Simple START/STOP Ladder Logic with Interlock**

 <img width="540" height="199" alt="image" src="https://github.com/user-attachments/assets/ddd7c6b6-4147-4565-b7a4-62f77a1f58a7" />

 control of start/stop system operation from the master

**Segment 3 – Another TSEND_C Block**

It is sending setpoint from the master PLC to slave PLC.

**Segment 4 – TRCV_C Block named “feedback”**

Here the master receives data from the slave ( measured RPM).

### Slave segments Siemens S7-1200

**Segment 1 – Slave TRCV**

The TRCV block receives START/STOP signals from the master.

**Segment 2 – Ladder Logic**

Simple START/STOP ladder logic used to activate or deactivate the motor from the HMI.

**Segment 3 – TRCV**

The TRCV block receives the desired RPM setpoint from the master.

**Segment 4 – TSEND**

The TSEND block sends the actual RPM measured by the slave back to the master.

## Tools

* TIA Portal V16
* Siemens S7-1200 and S7-1500 PLCs
* Arduino (RPM measurement)

## Repository Content

* `proyecto_final_PID.ap16` – TIA Portal project file
* `images/` – diagrams or ladder screenshots

## Author

Felipe Idárraga Quintero – Electronic Engineering

Julian Felipe Gutierrez Ramirez – Electronic Engineering

Universidad Nacional de Colombia
