# STM32-HMC5883L-HAL-Enhanced-Precise-Calibration
# HMC5883L STM32 HAL Library with Calibration

## Overview
This is an **STM32 HAL-based driver** for the **HMC5883L 3-axis digital magnetometer**. The library provides functions for initializing the sensor, reading raw and calibrated data, performing calibration, and computing heading direction with **declination angle compensation**.

## Features
- **I2C Communication** with STM32 HAL
- **Continuous & Single Measurement Modes**
- **Calibration Support** (Offset and Scaling Correction)
- **Magnetic Declination Compensation**
- **Heading Calculation (0° - 360°)**

## Hardware Requirements
- STM32 Development Board (e.g., STM32F103C8, STM32F4xx, etc.)
- HMC5883L Magnetometer Module
- I2C Pull-up Resistors (4.7kΩ recommended)

## Wiring (I2C Connection)
| HMC5883L Pin | STM32 Pin |
|-------------|-----------|
| VCC        | 3.3V / 5V |
| GND        | GND       |
| SCL        | SCL (PB6 for STM32F1) |
| SDA        | SDA (PB7 for STM32F1) |

## Installation
1. **Copy the files** `HMC5883L.c` and `HMC5883L.h` to your project.
2. **Include the header file** in your main source file:
   ```c
   #include "HMC5883L.h"
   ```
3. **Initialize the sensor in your main function:**
   ```c
   HMC5883L mag;
   HMC5883L_Init(&mag, &hi2c1, GAIN_1090);
   ```

## Usage
### **1. Initialize the Sensor**
```c
HMC5883L mag;
HMC5883L_Init(&mag, &hi2c1, GAIN_1090);
```

### **2. Read Raw Data**
```c
int16_t raw_data[3];
HMC5883L_ReadRaw(&mag, raw_data);
printf("Raw X: %d, Y: %d, Z: %d\n", raw_data[0], raw_data[1], raw_data[2]);
```

### **3. Perform Calibration**
```c
HMC5883L_Calibrate(&mag, 500);
printf("Calibration done! Offsets: X=%d, Y=%d, Z=%d\n", mag.mag_offset[0], mag.mag_offset[1], mag.mag_offset[2]);
```

### **4. Read Calibrated Data**
```c
MagnetometerData data;
HMC5883L_ReadCalibrated(&mag, &data);
printf("Calibrated X: %.2f, Y: %.2f, Z: %.2f\n", data.x, data.y, data.z);
```

### **5. Compute Heading**
```c
float heading = HMC5883L_GetHeading(&mag, &data);
printf("Heading: %.2f degrees\n", heading);
```

## Calibration Guide
1. Rotate the sensor **in all directions** while collecting samples.
2. The function `HMC5883L_Calibrate()` stores **offset and scaling** values.
3. The corrected values are used in `HMC5883L_ReadCalibrated()`.

## Notes
- The **magnetic declination angle** should be set based on your location using `HMC5883L_SetDeclination()`.
- The **Z and Y axes are swapped** due to HMC5883L's internal orientation.
- The **STM32 HAL library is required** for I2C communication.

## License
This project is open-source under the **MIT License**.

## Contributions
Feel free to contribute by submitting issues and pull requests!

---
Enjoy coding with STM32 and HMC5883L! 🚀

