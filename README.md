# Interfacing-Arduino-Uno-and-LM35-Temperature-sensor
Interface LM35 temp sensor assuming the sensor is connected to analog pin A0 with an Arduino Uno and control the onboard LED based on temp readings: When the temp falls below 30°C, make the onboard LED blink every 250 ms and If the temp rises above 30°C, make the onboard LED blink every 500 ms, without using Millis(), delay() and micros() function.
const int lm35_pin = A1;  // LM35 O/P pin
const int led_pin = 13;   // Onboard LED pin

void setup() {
  pinMode(led_pin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int temp_adc_val;
  float temp_val;

  temp_adc_val = analogRead(lm35_pin);  // Read Temperature
  temp_val = (temp_adc_val * 4.88);      // Convert adc value to equivalent voltage
  temp_val = (temp_val / 10);           // LM35 gives output of 10mV/°C

  Serial.print("Temperature = ");
  Serial.print(temp_val);
  Serial.print(" Degree Celsius\n");

  // Check temperature conditions and control LED accordingly
  if (temp_val < 30.0) {
    blinkLED(250);  // Blink every 250 milliseconds
  } else if (temp_val > 30.0) {
    blinkLED(500);  // Blink every 500 milliseconds
  } else {
    digitalWrite(led_pin, LOW);  // Temperature is 30.0°C, turn off LED
  }
}

void blinkLED(unsigned long interval) {
  static unsigned long previousMillis = 0;
  static bool ledState = LOW;

  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;
    ledState = !ledState;
    digitalWrite(led_pin, ledState);
  }
}
