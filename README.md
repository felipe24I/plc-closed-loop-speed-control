# Closed-Loop Speed Control using Siemens PLC S7-1200 and S7-1500

Implementation of a closed-loop speed control system for a three-phase motor using industrial PLCs and a PID controller.

## Overview

This project implements a closed-loop speed control architecture using a master–slave PLC configuration. The system regulates the motor speed using a PID controller and real-time feedback from an external speed measurement system.

## System Architecture
https://github.com/felipe24I/plc-closed-loop-speed-control/blob/main/images/System_architecture/system_architecture.png?raw=true

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

## Communication

The two PLCs exchange data using **TSEND/TRCV communication blocks**, allowing transmission of:

* START/STOP commands
* Speed setpoint
* Measured motor speed

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
