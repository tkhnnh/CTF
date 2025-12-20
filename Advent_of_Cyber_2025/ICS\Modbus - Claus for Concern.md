# What is SCADA?
command centres of industrial operations -> senses what's happening, processes that info, and sends commands to make things happen

## Components of a SCADA system
1. Sensors & Actuators : eyes and hands of the system -> Sensors measure real-world conditions (temperature, pressure, and weight) while actuators perform physical acitons ( motors turn, valves open, robotic arms move)
2. PLCs (Programmable Logic Controllers) -> make decisions based on programmed rules, and send commands to actuators
3. Monitoring Systems
4. Historians -> databases storing operational data for  later analysis

## SCADA in the Drone Delivery System
- Package type selection
- Delivery zone routing
- Visual Monitoring
- Inventory verification
- System protection mechanisms
- Audit logging

## Why SCADA Systems are targeted?
- Legacy software 0> systems installed decades ago and never updated
- Default credentials
- reliability, not security
- physical processes insteal of virtual one like web


  What port is commonly used by Modbus TCP?
  `502`

# What is a PLC?
industrial computer designed to control machinery and processes n real-world environment

### Purpose
- Survive harsh environments
- Run continously without failure
- Execute control logic in real-time
- Interface directly with physical hardware

## What is Modbus?
communication protocols that allow industrial devices talk to each other
-> no authentication, no encryption, no authorisation checking

### Modbus Data types
- Coils (Digital outputs - on/off) value (0 or 1) -> Motor running? Valve open? Alarm active
- Discrete Inputs (Digital outputs - on/off) value ( 0 or 1) -> button pressed, door closed, sensor triggered
- Holding Registers (Analouge outputs - numbers) value (0-65535) -> temperature setpoint, motor speed, zone selection
- Input Registers (Analogue inputs - numbers) value (0-65535) -> current temperature, pressure reading, flow rate

what's the flag?
`THM{eGgMas0v3r}`
