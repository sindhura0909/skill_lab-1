# skill_lab-1
## TRAFIC LIGHTS 
## To control a **Traffic Light System** using an **Arduino Uno** and a **Raspberry Pi**, you can use the Raspberry Pi to act as the control center that communicates with the Arduino Uno to manage the traffic light system.

### **Aim:**
To design a **Traffic Light Control System** using an **Arduino Uno** for controlling the LEDs and a **Raspberry Pi** to handle the logic and communication. The Raspberry Pi will send commands to the Arduino to control the traffic lights, and the Arduino will manage the LEDs (Red, Yellow, Green) accordingly.

### **Components Needed:**
1. **Arduino Uno**
2. **Raspberry Pi (Model 3 or later)**
3. **3 LEDs** (Red, Yellow, Green)
4. **Resistors** (220Ω or 330Ω)
5. **Breadboard**
6. **Jumper wires**
7. **USB cable** to connect Arduino to Raspberry Pi
8. **Raspberry Pi OS** with Python installed

### **Overview:**
1. **Raspberry Pi** will send commands to **Arduino** to control the traffic light sequence (Red, Yellow, Green).
2. The **Arduino Uno** will control the actual LEDs based on the commands received from the Raspberry Pi.
3. The Raspberry Pi can be programmed in **Python** to send commands to the Arduino over **Serial Communication** (USB or UART).

### **Steps:**

#### 1. **Wiring Setup:**
- **Connect the LEDs to the Arduino:**
  - Connect the **Red LED** to Pin 12.
  - Connect the **Yellow LED** to Pin 11.
  - Connect the **Green LED** to Pin 10.
  - Use a 220Ω resistor for each LED in series to limit the current.

- **Connect the Arduino to the Raspberry Pi:**
  - Use a **USB cable** to connect the **Arduino Uno** to the **Raspberry Pi**.

#### 2. **Arduino Code:**

This code will allow the **Arduino** to receive serial commands from the **Raspberry Pi** and control the LEDs accordingly.

```cpp
// Pin Definitions
int redPin = 12;
int yellowPin = 11;
int greenPin = 10;

void setup() {
  // Set LED pins as output
  pinMode(redPin, OUTPUT);
  pinMode(yellowPin, OUTPUT);
  pinMode(greenPin, OUTPUT);

  // Start serial communication
  Serial.begin(9600);
}

void loop() {
  // Check if data is available from Raspberry Pi
  if (Serial.available() > 0) {
    char receivedData = Serial.read();  // Read the incoming byte

    if (receivedData == 'G') {  // Green light command
      digitalWrite(greenPin, HIGH);
      digitalWrite(yellowPin, LOW);
      digitalWrite(redPin, LOW);
    }
    else if (receivedData == 'Y') {  // Yellow light command
      digitalWrite(greenPin, LOW);
      digitalWrite(yellowPin, HIGH);
      digitalWrite(redPin, LOW);
    }
    else if (receivedData == 'R') {  // Red light command
      digitalWrite(greenPin, LOW);
      digitalWrite(yellowPin, LOW);
      digitalWrite(redPin, HIGH);
    }
  }
}
```

#### 3. **Raspberry Pi Code:**

Now, you can use **Python** on the Raspberry Pi to send commands to the Arduino via the USB serial port.

```python
import serial
import time

# Setup the serial communication (adjust the port if needed)
arduino = serial.Serial('/dev/ttyUSB0', 9600)  # or use /dev/ttyACM0 on some systems
time.sleep(2)  # Wait for Arduino to reset

# Function to control traffic lights
def traffic_light_sequence():
    while True:
        # Green Light for 10 seconds
        print("Green Light")
        arduino.write(b'G')  # Send Green signal to Arduino
        time.sleep(10)

        # Yellow Light for 2 seconds
        print("Yellow Light")
        arduino.write(b'Y')  # Send Yellow signal to Arduino
        time.sleep(2)

        # Red Light for 10 seconds
        print("Red Light")
        arduino.write(b'R')  # Send Red signal to Arduino
        time.sleep(10)

# Run the traffic light sequence
traffic_light_sequence()
```

#### 4. **Setting Up the Raspberry Pi and Arduino:**
1. **Install Python and PySerial** on the Raspberry Pi if not already installed:
   ```bash
   sudo apt-get update
   sudo apt-get install python3-pip
   pip3 install pyserial
   ```

2. Connect your **Raspberry Pi** to the **Arduino** using a USB cable.

3. **Run the Python script** on the Raspberry Pi to start the traffic light sequence:
   ```bash
   python3 traffic_light.py
   ```

4. The **Arduino Uno** will execute the commands sent by the Raspberry Pi, lighting up the LEDs (Green, Yellow, Red) based on the received command.

### **Explanation of the Process:**

1. **Raspberry Pi sends signals** to the **Arduino** via the serial communication.
2. The **Arduino receives these signals** and uses them to turn on the corresponding LED (Green, Yellow, or Red).
3. The **Python script on Raspberry Pi** controls the flow of the traffic light by sending commands at the appropriate times.

This setup demonstrates how a **Raspberry Pi** can communicate with an **Arduino** to control a traffic light system, making use of serial communication for simple and effective control.
