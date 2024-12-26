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