# 🎨 Smart Color-Based Object Sorting System



## 📋 Table of Contents
- [Introduction](#-introduction)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [Hardware Components](#-hardware-components)
- [System Workflow](#-system-workflow)
- [How It Works](#-how-it-works)
- [Setup Instructions](#-setup-instructions)
- [Challenges](#-challenges)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 🎯 Introduction

This project simulates an **automated color-based sorting system**, commonly used in manufacturing and packaging industries. The system identifies the color of objects placed on a conveyor and automatically sorts them into the appropriate bins based on the detected color.

The system uses the **TCS3200 color sensor** for detection and an **ATmega32 microcontroller** to control the logic and operations. It provides real-time feedback through an LCD and allows dynamic configuration using a keypad, with a built-in safety mechanism via an emergency stop feature.

---

## ✨ Features

- ✅ **Color Detection**: Accurate detection using frequency-based color analysis
- ✅ **User Configuration**: Flexible bin assignment (1-3) for each color using keypad
- ✅ **Visual Feedback**: LCD displays live status and system actions
- ✅ **Sorting Mechanism**: Stepper motor directs objects to correct bins
- ✅ **Emergency Handling**: External interrupt ensures quick stop in emergencies
- ✅ **Modular Architecture**: Clean separation between hardware and application logic
- ✅ **Real-time Processing**: Instant color classification and sorting decisions

---

## 🏗️ System Architecture

The project is divided into **four main modules**:

### 1️⃣ Input Module
- **TCS3200 Color Sensor** - Detects object color
- **4x4 Keypad** - User input for bin configuration
- **Push Button** - Emergency stop control

### 2️⃣ Processing Module
- **ATmega32 Microcontroller** - System control and logic
- **Decision-making Algorithm** - Color-based sorting logic

### 3️⃣ Output Module
- **DC Motor** - Simulates conveyor belt movement
- **Stepper Motor** - Directs objects to correct bins
- **16x2 LCD** - Displays system status
- **Buzzer & LED** - Emergency alerts

### 4️⃣ Control Logic
- **Background Calibration** - Detects ambient light levels
- **Color Comparison Algorithm** - Identifies dominant color (R/G/B)
- **Sorting Algorithm** - Routes objects to assigned bins

---

## 🛠️ Hardware Components

| Component | Quantity | Description |
|-----------|----------|-------------|
| **ATmega32** | 1 | 8-bit AVR microcontroller (main processor) |
| **TCS3200** | 1 | RGB color sensor with frequency output |
| **16x2 LCD** | 1 | Character display for system feedback |
| **4x4 Keypad** | 1 | User input for bin configuration |
| **DC Motor** | 1 | Conveyor belt simulation |
| **Stepper Motor** | 1 | Bin selector mechanism (28BYJ-48) |
| **Push Button** | 1 | Emergency stop trigger (INT1 on PD3) |
| **LED** | 1 | Emergency indicator (PA5) |
| **Buzzer** | 1 | Emergency alert sound (PA6) |
| **Resistors** | - | Pull-up resistors for buttons |
| **Power Supply** | 1 | 5V DC for microcontroller and peripherals |

---

## 📊 System Workflow

```
┌─────────────────────────────────────────────────────────┐
│                  1. INITIALIZATION                       │
│  • LCD displays "Production Line Ready"                 │
│  • User assigns bins to colors via keypad               │
│  • Prevents duplicate bin assignments                   │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│              2. BACKGROUND CALIBRATION                   │
│  • Reads ambient RGB values (no object)                 │
│  • Sets detection threshold                             │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│               3. SORTING OPERATION                       │
│  • DC motor runs (conveyor moves)                       │
│  • Color sensor detects object                          │
│  • Determine dominant color (R/G/B)                     │
│  • Stepper motor rotates to assigned bin                │
│  • Loop repeats for next object                         │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│              4. EMERGENCY HANDLING                       │
│  • User presses emergency stop button                   │
│  • System halts immediately                             │
│  • Buzzer + LED activate                                │
│  • System resumes after button release                  │
└─────────────────────────────────────────────────────────┘
```

---

## 🔬 How It Works

### TCS3200 Color Sensor Principle
The TCS3200 sensor uses a **grid of photodiodes** filtered for Red, Green, Blue, and Clear light:

1. **Filter Selection**: Control signals select which color filter is active
2. **Light-to-Frequency Conversion**: Converts light intensity to frequency signal
3. **Frequency Output**: Output frequency is proportional to detected color intensity
4. **Color Determination**: Microcontroller reads frequencies and compares R, G, B intensities

### Sorting Logic
```c
1. Read RGB frequencies from TCS3200
2. Compare intensities: max(R, G, B)
3. Identify dominant color
4. Lookup user-configured bin number
5. Rotate stepper motor to bin position
6. Release object into bin
7. Return to home position
8. Repeat
```

### Pin Configuration
```
ATmega32 Pinout:
├─ PORTA: Stepper Motor (PA0-PA3) + DC Motor (PA4, PA7) + Alerts (PA5-PA6)
├─ PORTB: Keypad Rows
├─ PORTC: LCD Data + Keypad Columns  
└─ PORTD: Color Sensor Input (PD0-PD2) + Emergency Stop (PD3/INT1)
```

---

## ⚙️ Setup Instructions

### Software Requirements
- **Proteus 8.x** or higher (for simulation)
- **Atmel Studio** or **AVR-GCC** (for compilation)
- **Git** (for version control)

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yourusername/color-sorting-system.git
   cd color-sorting-system
   ```

2. **Compile the Code**
   ```bash
   # Using AVR-GCC
   avr-gcc -mmcu=atmega32 -DF_CPU=16000000UL -Os -o main.elf src/main.c
   avr-objcopy -O ihex main.elf main.hex
   ```

3. **Run Simulation**
   - Open `simulation/color_sorting.pdsprj` in Proteus
   - Load `main.hex` into ATmega32 component
   - Click **Run** to start simulation

4. **Physical Implementation** (Optional)
   - Flash `main.hex` to ATmega32 using USBasp/AVRISP
   - Connect hardware according to circuit diagram
   - Power on and follow LCD instructions

---



## 🚧 Challenges Faced

| Challenge | Solution |
|-----------|----------|
| **Sensor Calibration** | Implemented background calibration to differentiate object colors from ambient light |
| **Color Conflicts** | Used dominant color algorithm with threshold-based classification |
| **Interrupt Handling** | External interrupt (INT1) with proper ISR for non-blocking emergency stop |
| **Motor Synchronization** | Implemented sequential control with delays between DC and stepper operations |

---

## 🔮 Future Improvements

- [ ] Add support for more colors (Yellow, Orange, White)
- [ ] Implement object counting and statistics
- [ ] Add UART communication for PC monitoring
- [ ] Replace delays with timer-based non-blocking code
- [ ] Add EEPROM to save bin configurations
- [ ] Implement PID control for smoother motor operation
- [ ] Add weight sensor for multi-parameter sorting
- [ ] Web interface for remote monitoring

---

## 📁 Project Structure

```
color-sorting-system/
│
├── README.md                          # This file
├── LICENSE                            # MIT License
│
├── docs/
│   └── Project_Documentation.pdf      # Complete project report
│
├── src/
│   ├── main.c                        # Main application code (commented)
│   │
│   ├── HAL/                          # Hardware Abstraction Layer
│   │   ├── LCD_Driver/
│   │   │   ├── LCD.h
│   │   │   └── LCD.c
│   │   ├── KEYPAD/
│   │   │   ├── keypad.h
│   │   │   └── keypad.c
│   │   └── ColorSensor_Driver/
│   │       ├── ColorSensor_interface.h
│   │       └── ColorSensor.c
│   │
│   └── MCAL/                         # Microcontroller Abstraction Layer
│       ├── DIO_Driver/
│       │   ├── DIO_interface.h
│       │   └── DIO.c
│       ├── EXT_INT_Driver/
│       │   ├── INT_interface.h
│       │   └── INT.c
│       └── MACROS/
│           └── std_types.h
│
├── simulation/
│   ├── color_sorting.pdsprj         # Proteus simulation file
│   └── demo_video.mp4                # System demonstration video
│
└── images/
    ├── circuit_diagram.png           # Complete circuit schematic
    ├── flowchart.png                 # System workflow diagram
    └── screenshots/                  # LCD output screenshots
```

---

## 🎓 Project Context

This project was developed as part of the **Embedded Systems Track** at **ITI (Information Technology Institute)** during the summer training program. It demonstrates practical implementation of:

- Microcontroller interfacing
- Sensor integration
- Motor control systems
- Real-time embedded programming
- Interrupt handling
- Modular software design

---



## 👤 Author

**[Ahmed Helal]**  
🎓 Embedded Systems - ITI Summer Training  
📧 Email: hlal28182@gmail.com  
💼 LinkedIn: [your-linkedin-profile](https://linkedin.com/in/ahmed-helal1)  
🐙 GitHub: [@yourusername](https://github.com/AhmeDHelaL58)

---

## 🙏 Acknowledgments

- **ITI** for providing excellent training in embedded systems
- **Instructors** for guidance and support throughout the project
- **TCS3200 Datasheet** for sensor implementation details
- **ATmega32 Datasheet** for microcontroller specifications

---

## 📞 Contact & Support

If you have questions or suggestions:
- 📧 Email: hlal28182@gmail.com
- 💬 Open an [Issue](https://github.com/AhmeDHelaL58/color-sorting-system/issues)
- ⭐ Star this repo if you found it helpful!

---

<div align="center">

### ⭐ If you found this project useful, please give it a star! ⭐


</div>
