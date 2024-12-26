# States of heater operation

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


# Error cases

A robust system must anticipate and handle various error scenarios. We have tried to identify potential issues, such as overheating, ignition failure, and sensor malfunctions. For each error case, it outlines the trigger conditions, the sensors involved, and the actions taken to mitigate risks and ensure safety.

| **Error Case**             | **Trigger Condition**                                                          | **Sensor(s)**           | **Outcome**                                                                                                                     |
| -------------------------- | ------------------------------------------------------------------------------ | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Overheating                | `case_temp > MAX_TEMP_THRESHOLD`                                               | Case Temp               | Shut off fuel pump. Keep fan running to cool combustion chamber. Transition to **Error** state.                                 |
| Failure to Ignite          | `flame_sensor == FALSE` or `case_temp < IGNITION_TEMP_THRESHOLD` after timeout | Flame Sensor, Case Temp | Stop pump to prevent flooding. Keep fan running to clear unburned fuel. Transition to **Error** state.                          |
| Flame Extinguished         | `flame_sensor == FALSE` or `case_temp < FLAME_OUT_TEMP`                        | Flame Sensor, Case Temp | Shut off fuel pump. Transition to **Cooldown** state to clear residual heat.                                                    |
| Overcooling                | `case_temp < MIN_TEMP_THRESHOLD`                                               | Case Temp               | Shut off pump and glow plug. Transition to **Idle** state to prevent unnecessary operation.                                     |
| Ambient Overheat           | `ambient_temp > AMBIENT_MAX_TEMP_THRESHOLD`                                    | Ambient Temp            | Shut down the system (pump, glow plug, fan). Transition to **Error** state to signal overheating.                               |
| Fuel Pump Malfunction      | `flame_sensor == FALSE` or case temp doesn't rise                              | Flame Sensor, Case Temp | Stop all components (fuel pump, fan, glow plug). Transition to **Error** state.                                                 |
| Fan Failure                | No temp changes or flagged error                                               | Case Temp               | Transition to **Error** state. Attempt to restart the fan (if possible). Shut down other components if fan cannot be restarted. |
| Glow Plug Failure          | `case_temp` doesn't rise during **Glow** phase                                 | Case Temp               | Transition to **Error** state. Prompt a restart or alert user for maintenance.                                                  |
| Sensor Malfunction         | Invalid or static values (`sensor_change_rate < MIN_RATE`)                     | Any                     | Transition to **Error** state. Attempt sensor recalibration or notify user of faulty sensor.                                    |
| Blocked Air Intake/Exhaust | `temp_change_rate > MAX_RATE` or `inlet_temp drops unexpectedly`               | Case Temp, Inlet Temp   | Shut off fuel pump and glow plug. Keep fan running to clear combustion chamber. Transition to **Error** state.                  |
| Low Ambient Temperature    | `ambient_temp < AMBIENT_MIN_TEMP_THRESHOLD`                                    | Ambient Temp            | Notify user to address low temperature conditions. Transition to **Idle** state to avoid inefficient operation or damage.       |

### 1. **Overheating**

- **Cause**: Combustion chamber temperature exceeds safe operating limits.
- **Sensor**: Case Temperature Sensor
- **Variable/Trigger**: `case_temp > MAX_TEMP_THRESHOLD`
    - `MAX_TEMP_THRESHOLD` could be a value like 250°C or based on manufacturer specs.

---

### 2. **Failure to Ignite**

- **Cause**: The flame is not established within the expected timeframe during the ignition phase.
- **Sensor**: Flame Sensor (optional), Case Temperature Sensor
- **Variable/Trigger**:
    - `flame_sensor == FALSE` after a specific time (`ignition_timeout`).
    - `case_temp < IGNITION_TEMP_THRESHOLD` after `ignition_timeout`.

---

### 3. **Flame Extinguished**

- **Cause**: Flame goes out unexpectedly during the **Fire** state.
- **Sensor**: Flame Sensor (optional), Case Temperature Sensor
- **Variable/Trigger**:
    - `flame_sensor == FALSE` during **Fire** state.
    - `case_temp` drops rapidly or below a predefined value (`FLAME_OUT_TEMP`).

---

### 4. **Overcooling**

- **Cause**: Case temperature drops too low, possibly due to excessive airflow or incorrect operation.
- **Sensor**: Case Temperature Sensor
- **Variable/Trigger**: `case_temp < MIN_TEMP_THRESHOLD`

---

### 5. **Ambient Overheat**

- **Cause**: The ambient temperature exceeds safe limits.
- **Sensor**: Ambient Temperature Sensor
- **Variable/Trigger**: `ambient_temp > AMBIENT_MAX_TEMP_THRESHOLD`

---

### 6. **Fuel Pump Malfunction**

- **Cause**: Fuel pump fails to deliver fuel as expected.
- **Sensor**: Flame Sensor, Case Temperature Sensor
- **Variable/Trigger**:
    - `flame_sensor == FALSE` despite pump being active.
    - `case_temp` does not rise as expected during **Fire**.

---

### 7. **Fan Failure**

- **Cause**: Fan does not operate or airflow is insufficient for combustion or cooling.
- **Sensor**: None directly; inferred from system behavior.
- **Variable/Trigger**:
    - No change in `case_temp` during **Glow** or **Cooldown**.
    - Error flag from the fan motor (if equipped).

---

### 8. **Glow Plug Failure**

- **Cause**: Glow plug fails to heat the combustion chamber.
- **Sensor**: Case Temperature Sensor
- **Variable/Trigger**: `case_temp` does not rise during **Glow** phase.

---

### 9. **Sensor Malfunction**

- **Cause**: Sensor reports invalid values or stops responding.
- **Sensor**: Any
- **Variable/Trigger**:
    - Out-of-range or invalid readings, e.g., `case_temp == NULL` or `inlet_temp < 0°C`.
    - Sensor readings are static when they should be dynamic (`sensor_change_rate < MIN_RATE`).

---

### 10. **Blocked Air Intake or Exhaust**

- **Cause**: Airflow is restricted, causing incomplete combustion or overheating.
- **Sensor**: Case Temperature Sensor, Inlet Temperature Sensor
- **Variable/Trigger**:
    - `case_temp` rises abnormally fast (`temp_change_rate > MAX_RATE`).
    - `inlet_temp` drops despite fan activity.

---

### 11. **Low Ambient Temperature**

- **Cause**: Ambient temperature too low for normal operation.
- **Sensor**: Ambient Temperature Sensor
- **Variable/Trigger**: `ambient_temp < AMBIENT_MIN_TEMP_THRESHOLD`


# Error state Handling

Building upon the identified error cases, we discusses how the system transitions into and recovers from the **Error** state. We showcase user notifications, fail-safe measures, and system restarts for recoverable errors. The focus is on maintaining safety.

- **Error States**:
    
    - Most errors transition the system to an **Error** state where the system components are either stopped or placed in a safe configuration. In many cases, the fan continues running to clear heat or fumes.
- **User Notifications**:
    
    - Some errors (e.g., sensor malfunction, low ambient temperature) might require a user intervention or maintenance alert.
- **System Restarts**:
    
    - For recoverable errors (e.g., blocked air intake or flame extinguished), the system can attempt a restart after clearing conditions.
- **Fail-Safe Measures**:
    
    - The system prioritizes safety, ensuring no unburned fuel is left in the chamber, avoiding damage from overheating, and preventing unsafe operation under suboptimal conditions

# Sensor Polling 

Sensor data is critical to the heater’s performance and safety. We explore recommendations for sensor polling frequencies based on their role in the system. It introduces dynamic polling, which adjusts frequency based on operational states, and filtering techniques to smooth noisy data.
### **Sensor Polling Recommendations**

- **High-Resolution Sensors (e.g., Flame and Case Temp Sensors):**
    
    - **Frequency**: Poll every 100–200 milliseconds (5–10 Hz).
    - **Reason**: Rapid changes (e.g., flame ignition or overheating) require quick detection for safety.
- **Medium-Resolution Sensors (e.g., Inlet and Ambient Temp Sensors):**
    
    - **Frequency**: Poll every 500 milliseconds to 1 second (1–2 Hz).
    - **Reason**: These temperatures typically change slowly, so less frequent polling reduces system overhead.
- **Error Monitoring**:
    
    - For critical conditions (e.g., overheating, flame-out), ensure error checks are performed on each sensor poll cycle to respond in real-time.


|**Sensor**|**Recommended Frequency**|**Reason**|
|---|---|---|
|Case Temp Sensor|100–200 ms|Detect rapid temperature changes in combustion chamber.|
|Flame Sensor|100–200 ms|Ensure real-time detection of flame establishment or loss.|
|Inlet Temp Sensor|500 ms – 1 sec|Monitor air input stability without excessive overhead.|
|Ambient Temp Sensor|1–2 sec|Room temperature changes slowly; frequent updates unnecessary.|

### **Dynamic Polling (nice to have)**

- **Adjust Frequencies Based on State**:
    
    - **Prime/Glow/Ignition/Fire**: Use higher polling frequencies (100–200 ms) for critical sensors (e.g., flame, case temp).
    - **Idle**: Reduce polling frequency to save resources (e.g., 1–2 seconds for all sensors).
    - **Error States**: Use medium polling (500 ms – 1 second) for diagnostics.
- **Event-Driven Polling**:
    
    - Implement event-based interrupts for significant sensor changes (e.g., rapid temperature rise) to complement polling. This ensures critical events are handled promptly without constant high-frequency polling.

### **Filtering and Smoothing Sensor Data**

- Use **moving averages** or low-pass filters to smooth noisy sensor readings:
    - For example, compute a running average of the last 5–10 samples to avoid reacting to transient spikes.
- For flame sensors, consider using a **debounce timer** (require a steady flame signal for 1–2 seconds before confirming ignition).

### **Safety and Redundancy**

- Implement **fail-safe timers** for critical states (e.g., ignition, fire):
    - If expected conditions (e.g., flame detected, temp rising) are not met within a certain time, trigger an error.
- Perform **sensor health checks**:
    - Verify sensors are returning valid data (e.g., within expected ranges) on each poll cycle to prevent erroneous readings from causing unsafe behavior.