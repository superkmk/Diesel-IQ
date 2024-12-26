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