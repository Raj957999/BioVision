BioVision – Real-Time Patient Monitoring System

Overview
BioVision is a **real-time patient vital monitoring system** built with Arduino.  
It uses a **TMP36 temperature sensor** and a **potentiometer** (to simulate SpO₂ levels) and triggers alerts when abnormal values are detected.
Features
- Real‑time monitoring of patient temperature
- Adjustable potentiometer simulates oxygen level
- Alerts via buzzer when readings cross safety thresholds
- Displays readings on a 16x2 LCD
- Low‑cost, portable design
Components Used
- Arduino Uno
- TMP36 Temperature Sensor
- Potentiometer
- 16x2 LCD with I2C Module
- Buzzer
- Breadboard & Jumper Wires
How It Works
1. TMP36 measures temperature in °C
2. Potentiometer value simulates oxygen saturation level
3. LCD displays temperature and oxygen level
4. If readings exceed threshold → Buzzer sounds an alarm
Code
The Arduino sketch continuously reads sensor values, processes them, and updates the LCD while monitoring for alert conditions.
#include <LiquidCrystal.h>

// LCD pin connections: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

#define TEMP_SENSOR A0
#define SPO2_SENSOR A1
#define BUZZER 7

void setup() {
  lcd.begin(16, 2);   // LCD 16x2
  pinMode(BUZZER, OUTPUT);
  lcd.setCursor(0, 0);
  lcd.print("BioVision Ready");
  delay(2000);
  lcd.clear();
}

void loop() {
  // Temperature from TMP36
  int tempReading = analogRead(TEMP_SENSOR);
  float voltage = tempReading * (5.0 / 1023.0);
  float temperatureC = (voltage - 0.5) * 100.0;

  // Simulated SpO2 from Potentiometer
  int spo2Reading = analogRead(SPO2_SENSOR);
  int spo2 = map(spo2Reading, 0, 1023, 80, 100);

  // Show on LCD
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temperatureC, 1);
  lcd.print(" C  ");

  lcd.setCursor(0, 1);
  lcd.print("SpO2: ");
  lcd.print(spo2);
  lcd.print(" %   ");

  // Alert Logic
  if (temperatureC > 39.0 || spo2 < 90) {
    digitalWrite(BUZZER, HIGH);
  } else {
    digitalWrite(BUZZER, LOW);
  }

  delay(1000);
}
