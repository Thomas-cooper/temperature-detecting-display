#include<TM1637Display.h>
const int tempSensorPin = A0;
const int piezoBuzzerPin = 13;
const int redPin = 10;
const int greenPin = 12;
const int bluePin = 11;
const int 
const int
const float tempHighThreshold = 80.0;
const float tempLowThreshold = 60.0;

void setup() {
  pinMode(piezoBuzzerPin, OUTPUT);
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int tempSensorValue = analogRead(tempSensorPin);
  float voltage = tempSensorValue * (5.0 / 1023.0);
  float temperatureC = (voltage - 0.5) * 100.0;
  float temperatureF = (temperatureC * 9.0 / 5.0) + 32.0;
  Serial.print("Temperature (F): ");
  Serial.println(temperatureF);

  if (temperatureF > tempHighThreshold) {
    digitalWrite(greenPin, LOW);
    digitalWrite(bluePin, LOW);
    for (int i = 0; i < 5; i++) {
      digitalWrite(redPin, HIGH);
      
      tone(piezoBuzzerPin, 1000);
      delay(250);
      tone(piezoBuzzerPin, 1500);
      delay(250);
      digitalWrite(redPin, LOW);
      noTone(piezoBuzzerPin);
    }
  } else if (temperatureF < tempLowThreshold) {
    digitalWrite(greenPin, LOW);
    digitalWrite(redPin, LOW);
    for (int i = 0; i < 5; i++) {
      digitalWrite(bluePin, HIGH);
      delay(250);
      digitalWrite(bluePin, LOW);
      delay(250);
    }
  } else {
    digitalWrite(redPin, LOW);
    digitalWrite(bluePin, LOW);
    digitalWrite(greenPin, HIGH);
    delay(1000);
  }
}
