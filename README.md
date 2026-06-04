# Arduino Password Door Lock System 🔐

A password-protected security door lock system built with Arduino. This project uses a **4x4 matrix keypad** for password entry, a **16x2 LCD display** for user feedback, and a **micro servo motor** to simulate the locking mechanism. Access is granted only when the correct 4-digit master password is entered.

---

## 📸 Project Visuals

### 1. Breadboard Wiring Diagram
This diagram shows the connections between the Arduino Uno, LCD, keypad, and servo motor.

![Breadboard Layout](circuit%20design.png)

### 2. Circuit Schematic Diagram
The technical schematic of the system design, created in Tinkercad.

![Circuit Schematic](Surprising%20Vihelmo.png)

---

## 🛠️ Components Required

* **Microcontroller:** Arduino Uno (or compatible board)
* **Input:** 4x4 Matrix Keypad
* **Output Display:** 16x2 Character LCD (without I2C module, using 4-bit parallel connection)
* **Actuator:** Micro Servo Motor (e.g., SG90)
* **Resistors:**
  * `1kΩ` (for LCD contrast adjustment)
  * `330Ω` (for LCD backlight anode protection)
* **Power Supply:** USB cable or external power supply for the Arduino Uno
* **Miscellaneous:** Breadboard, hookup wires

---

## 🔌 Pin Connections

| Component | Pin Name | Arduino Uno Pin | Description |
| :--- | :--- | :--- | :--- |
| **LCD 16x2** | RS | **A0** | Register Select |
| | E | **A1** | Enable |
| | D4 | **A2** | Data Pin 4 |
| | D5 | **A3** | Data Pin 5 |
| | D6 | **A4** | Data Pin 6 |
| | D7 | **A5** | Data Pin 7 |
| | VCC | **5V** | LCD Power |
| | GND | **GND** | LCD Ground |
| | V0 (Contrast) | **GND** (via 1kΩ Resistor) | Adjusts screen contrast |
| | LED+ (Anode) | **5V** (via 330Ω Resistor) | Backlight Anode |
| | LED- (Cathode)| **GND** | Backlight Cathode |
| **4x4 Keypad** | Row 1 | **D0** (RX) | Keypad Row Pin 1 |
| | Row 2 | **D1** (TX) | Keypad Row Pin 2 |
| | Row 3 | **D2** | Keypad Row Pin 3 |
| | Row 4 | **D3** | Keypad Row Pin 4 |
| | Col 1 | **D4** | Keypad Column Pin 1 |
| | Col 2 | **D5** | Keypad Column Pin 2 |
| | Col 3 | **D6** | Keypad Column Pin 3 |
| | Col 4 | **D7** | Keypad Column Pin 4 |
| **Servo Motor**| SIG (Signal) | **D9** | PWM Servo Control |
| | PWR (Power) | **5V** | Servo Power |
| | GND (Ground) | **GND** | Servo Ground |

> ⚠️ **Note:** Keypad Row 1 and Row 2 utilize Digital Pins 0 (RX) and 1 (TX). If you are uploading code to the Arduino, disconnect these pins momentarily to avoid serial communication conflicts.

---

## ⚙️ How It Works

1. **System Initialization:** 
   * When powered on, the LCD displays `Protected Door` and shows a progress animation (`Loading.........`).
   * The servo motor initializes to the locked position (`0` degrees).

2. **Access Control:**
   * The LCD displays `Enter Password`.
   * You type a 4-digit code using the matrix keypad.
   * **Correct Password (`8877`):**
     * The LCD shows `Door is Open`.
     * The servo motor rotates from `0°` to `90°` to unlock.
     * The door stays open for **5 seconds**, followed by a **9-second countdown** (represented by `Waiting.........` on screen).
     * Once the countdown completes, it displays `Time is up!` and the servo automatically locks the door (rotates back to `0°`).
     * *Alternative Lock:* If you press the `#` key on the keypad while the door is open, the system bypasses the countdown and locks the door immediately.
   * **Wrong Password:**
     * The LCD shows `Wrong Password`, the door remains locked, and the input resets.

---

## 💻 Arduino Sketch

The core logic is written in C++ using the standard Arduino IDE libraries.

### Required Libraries
Ensure you have the following libraries installed in your Arduino IDE:
* `Keypad` (by Mark Stanley, Alexander Brevig)
* `LiquidCrystal` (Built-in)
* `Servo` (Built-in)

### Code Highlights
* **Default Password:** The default master code is defined on line 13 as `"8877"`. You can change this to any 4-digit code in the source code file `lock.ino`.
* **Lock/Unlock Angles:** The servo rotates smoothly in increments of 10 degrees to prevent mechanical stress:
  ```cpp
  void ServoOpen() {
    for (pos = 0; pos <= 90; pos += 10) {
      myservo.write(pos);
    }
  }
  ```

---

## 🚀 Getting Started

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-username/password-door-lock.git
   cd password-door-lock
   ```
2. **Build the Circuit:** Connect the hardware components according to the [Pin Connections](#-pin-connections) table and diagrams.
3. **Upload the Code:**
   * Open `lock.ino` in the Arduino IDE.
   * Disconnect pins D0 and D1 (Keypad Rows 1 & 2) temporarily from the Arduino.
   * Select your board (Arduino Uno) and COM Port.
   * Click **Upload**.
   * Reconnect pins D0 and D1 once upload is successful.
4. **Test the Lock:** Type `8877` to open, or press `#` while open to lock it early.

---

*Made with Tinkercad 🛠️*
