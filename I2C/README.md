# Intro:
The **Inter-Integrated Circuit** (I2C, pronounced I-squared-C) is a synchronous, multi-master, multi-slave, packet switched, single-ended, serial communication bus invented in 1982 by Philips Semiconductor (now NXP Semiconductors). It is widely used for attaching lower-speed peripheral ICs to processors and microcontrollers in short-distance, intra-board communication. Alternatively, I2C is spelled I2C (pronounced I-two-C) or IIC (pronounced I-I-C). Typical applications include GY series IMU and RTC clock module.[1]

# Character:
- **Master-Slave-Based**, multi-master, multi-slave.
- **Half-Duplex communication**, sharing one SDA line, can transmit and receive data, but not at the same time.
- **Synchronous communication**, the data from the master or the slave is synchronized on the clock edge.

# Interface:
- **SDA**：Serial Data 串列資料. Transmits data from the master to the slave.
- **SCL**：Serial Clock 串列時脈. Transmits data from the slave to the master.
Active low, need pull-up resistors 2K and 10K for 400kbps 100kbps
An example schematic with one master (a microcontroller), three slave nodes (an ADC, a DAC, and a microcontroller), and pull-up resistors Rp
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/I2C/I2C_interface.png" width=400>

# Address
address and maximum slave device數量？
7-bit
10-bit

# Data Transmission
ddd
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/I2C/data_transmission.png" width=600>
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/I2C/data_transmission2.png" width=600>

# Multi-device Configuration
ddd
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/SPI/multislave_daisy_chain.png" height=250> <img src="https://raw.githubusercontent.com/shannon112/Notes/main/SPI/multislave_daisy_chain2.png" height=250>

# Pros and cons 
- Advantages [1]  
- Disadvantages [1]  

# Demo: I2C GY-80 and GY-521 Reading
Original experiment from: https://howtomechatronics.com/tutorials/arduino/how-i2c-communication-works-and-how-to-use-it-with-arduino/ [2]

<img src="https://raw.githubusercontent.com/shannon112/Notes/main/I2C/GY_address.png" width=600>
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/I2C/Connection.png" width=600>

```
#include <Wire.h>
int ADXLAddress = 0x53; // Device address in which is also included the 8th bit for selecting the mode, read in this case.
#define X_Axis_Register_DATAX0 0x32 // Hexadecima address for the DATAX0 internal register.
#define X_Axis_Register_DATAX1 0x33 // Hexadecima address for the DATAX1 internal register.
#define Power_Register 0x2D // Power Control Register
int X0,X1,X_out;
void setup() {
  Wire.begin(); // Initiate the Wire library
  Serial.begin(9600);
  delay(100);
  // Enable measurement
  Wire.beginTransmission(ADXLAddress);
  Wire.write(Power_Register);
  // Bit D3 High for measuring enable (0000 1000)
  Wire.write(8);  
  Wire.endTransmission();
}
void loop() {
  Wire.beginTransmission(ADXLAddress); // Begin transmission to the Sensor 
  //Ask the particular registers for data
  Wire.write(X_Axis_Register_DATAX0);
  Wire.write(X_Axis_Register_DATAX1);
  
  Wire.endTransmission(); // Ends the transmission and transmits the data from the two registers
  
  Wire.requestFrom(ADXLAddress,2); // Request the transmitted two bytes from the two registers
  
  if(Wire.available()<=2) {  // 
    X0 = Wire.read(); // Reads the data from the register
    X1 = Wire.read();   
  }
  
  Serial.print("X0= ");
  Serial.print(X0);
  Serial.print("   X1= ");
  Serial.println(X1);
}
```
- 74HC595: "8-bit serial-in, serial or parallel-out shift register with output latches; 3-state."   
  https://www.arduino.cc/en/Tutorial/Foundations/ShiftOut
- slaveSelect: can choose any digital pin, not restrict to pin10 on arduino
- SPI.setBitOrder: depend the slave device spec/datasheet. 像是頭先進還是尾巴先進  
  ```MSB (Most Significant Bit) Bit7, LSB (Least Significant Bit) Bit0```
- SPI.setDataMode: depend the slave device spec/datasheet. Mode 0~3 as mentioned above.
- SPI.transfer: 可以傳int, hex, binary等等.  
  ```250//int, 0xFA//hex, B11111010//binary```

# Reference
[1] https://en.wikipedia.org/wiki/I%C2%B2C
[2] https://www.youtube.com/watch?v=6IAkYpmA1DQ