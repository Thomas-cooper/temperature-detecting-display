This project is ment to read the temperature and display it in numerous different ways those beings through a seven segment display monitor, a buzzer, and an rgb light these three components were used to comply with the assignment to utilize a seven segment display in conjunction with a heat sensor. This was achieved throught the usage of a library meant to allow for the digital display to function using code within c++.

The first section sets up the variables used within the code as well as including the necesary libraries and adding temperature thresholds where at which the buzzer and rgb light will preform the tasks assigned to them within the rest of the code

#include<TM1637Display.h>
const int tempSensorPin = A0;
const int piezoBuzzerPin = 13;
const int redPin = 10;
const int greenPin = 12;
const int bluePin = 11;
const float tempHighThreshold = 80.0;
const float tempLowThreshold = 60.0;

The next section wihtin the code sets it up were only the tempperature sensor will be taking in external information whilst the also making it so the other componenets are only capable of opperating within the capacity allowed by the heat sensor and the environmnet around it and putting in the command that activates the code in the beginning

void setup() {
  pinMode(tempSensorPin,INPUT);
  pinMode(piezoBuzzerPin, OUTPUT);
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
  Serial.begin(9600);
}

The next segments of code take place within in loop meaning throughout the codes duration commands layed out within the it will be implemented anytime their requiremnets are met

This part of the loop converts the temperature data obtained by the temperature detector into fahrenheit and celceuis while printing what the temperature is on the seven segment display

void loop() {
  int tempSensorValue = analogRead(tempSensorPin);
  float voltage = tempSensorValue * (5.0 / 1023.0);
  float temperatureC = (voltage - 0.5) * 100.0;
  float temperatureF = (temperatureC * 9.0 / 5.0) + 32.0;
  Serial.print("Temperature (F): ");
  Serial.println(temperatureF);

The next segment uses the previous temperature thresholds established previously within the code by adding the fahrenheit to the high threshhold before then activating the buzzer

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

The final few segments will be activated by the conditions not being met which will lead to everthing being deactivated 

else if (temperatureF < tempLowThreshold) {
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

      
