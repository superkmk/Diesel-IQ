# Operational states of heater

This section defines the operational states of the diesel heater and the role of each component (glow plug, pump, fan) during these states. It explains how the system progresses from idle to active heating and eventually shuts down safely. Each state is accompanied by a description and sensor monitoring requirements.

| **State** | **Glow Plug** | **Pump** | **Fan** |                               **Description**                               |
| :-------: | :-----------: | :------: | :-----: | :-------------------------------------------------------------------------: |
|   Prime   |      Off      |    On    |   Off   |               Prepares the fuel line by priming it with fuel.               |
|   Glow    |      On       |   Off    |   On    |             Heats the combustion chamber and provides airflow.              |
| Ignition  |      On       |    On    |   On    | Glow plug remains active while fuel pump feeds fuel to establish the flame. |
|   Fire    |      Off      |    On    |   On    |         Combustion starts; pump feeds fuel; fan maintains airflow.          |
| Cooldown  |      Off      |   Off    |   On    |         Allows for safe shutdown by cooling the combustion chamber.         |
|   Idle    |      Off      |   Off    |   Off   |               System is in standby; no components are active.               |
|   Error   |      Off      |   Off    |   On    |              Fan runs to clear the chamber if an error occurs.              |
|  Preheat  |      On       |   Off    |   Off   |       Activates the glow plug to bring the chamber to operating temp.       |

## Prime
The **Prime** state prepares the fuel line by priming it with fuel. The pump is activated to ensure the system has an adequate fuel supply before proceeding to the ignition process. 

- **Glow Plug**: Off
- **Pump**: On
- **Fan**: Off

### Sensors and Values:
- **Case Sensor**: Monitor for significant temperature increases to ensure no residual combustion is occurring.
- **Inlet Temp Sensor**: Check air inlet temperature to verify the system is ready for operation.
- **Ambient Temp Sensor**: Monitor room temperature to determine if heating is necessary (optional).
- **Flame Sensor**: Not monitored in this state.

---

## Glow
The **Glow** state activates the glow plug to heat the combustion chamber. The fan is also activated to provide airflow, ensuring proper oxygen availability for ignition.

- **Glow Plug**: On
- **Pump**: Off
- **Fan**: On

### Sensors and Values:
- **Case Sensor**: Monitor for temperature rise, ensuring the combustion chamber reaches ignition-ready temperatures.
- **Inlet Temp Sensor**: Ensure air temperature is stable and within operational ranges.
- **Ambient Temp Sensor**: Not critical in this state.
- **Flame Sensor**: Not monitored in this state.

---

## Ignition
The **Ignition** state begins the process of establishing a flame. The glow plug remains active while the pump delivers fuel, and the fan ensures sufficient airflow. This state continues until the flame sensor detects a stable flame or the case sensor indicates sufficient combustion temperature.

- **Glow Plug**: On
- **Pump**: On
- **Fan**: On

### Sensors and Values:
- **Case Sensor**: Monitor for a rapid temperature increase to verify combustion is starting.
- **Inlet Temp Sensor**: Check air stability to support proper combustion.
- **Ambient Temp Sensor**: Not critical in this state.
- **Flame Sensor**: Actively monitored to confirm flame.

---

## Fire
The **Fire** state indicates that combustion has been successfully established. The glow plug is deactivated, and the pump continues to deliver fuel while the fan maintains airflow.

- **Glow Plug**: Off
- **Pump**: On
- **Fan**: On

### Sensors and Values:
- **Case Sensor**: Monitor for consistent combustion temperatures, ensuring stable operation.
- **Inlet Temp Sensor**: Ensure air supply remains within expected temperature ranges.
- **Ambient Temp Sensor**: Used to regulate heating output.
- **Flame Sensor**: Periodically monitored to ensure flame stability.

---

## Cooldown
The **Cooldown** state allows the system to safely shut down by cooling the combustion chamber. The fan remains active to dissipate heat, and the glow plug is deactivated.

- **Glow Plug**: Off
- **Pump**: Off
- **Fan**: On

### Sensors and Values:
- **Case Sensor**: Monitor for a temperature drop to a safe level before transitioning to idle.
- **Inlet Temp Sensor**: Ensure air remains stable and does not contribute to overheating.
- **Ambient Temp Sensor**: Not critical in this state.
- **Flame Sensor**: Ensure no flame is present during the cooldown process.

---

## Idle
The **Idle** state represents a standby mode where no components are active. The system waits for a signal to begin the next cycle.

- **Glow Plug**: Off
- **Pump**: Off
- **Fan**: Off

### Sensors and Values:
- **Case Sensor**: Monitor for ambient stabilization to ensure the heater is cool and safe.
- **Inlet Temp Sensor**: Not critical in this state.
- **Ambient Temp Sensor**: Continuously monitored to determine if heating is needed.
- **Flame Sensor**: Ensure no residual flame is detected.

---

## Error
The **Error** state is triggered when an issue occurs, such as a failure to ignite or overheating. The fan runs to clear the combustion chamber and ensure safety.

- **Glow Plug**: Off
- **Pump**: Off
- **Fan**: On

### Sensors and Values:
- **Case Sensor**: Monitor for abnormal temperature changes or overheating.
- **Inlet Temp Sensor**: Verify air supply for unexpected fluctuations.
- **Ambient Temp Sensor**: Not critical in this state.
- **Flame Sensor**: Ensure no flame persists during error handling.

---

## Preheat
The **Preheat** state activates the glow plug to bring the combustion chamber to the required operating temperature. This state occurs before the **Glow** state if additional heating is needed.

- **Glow Plug**: On
- **Pump**: Off
- **Fan**: Off

### Sensors and Values:
- **Case Sensor**: Monitor for rising temperatures to confirm the chamber is heating effectively.
- **Inlet Temp Sensor**: Ensure air remains stable and does not cool the chamber.
- **Ambient Temp Sensor**: Not critical in this state.
- **Flame Sensor**: Not monitored in this state.





