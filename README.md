# Radar
My ardruino radar code
#include <Servo.h>

// Define pins based on your diagram
const int trigPin = 11;
const int echoPin = 10;
const int servoPin = 9;

Servo myServo;  // Create servo object

void setup() {
  // Initialize Serial Monitor to view distances
  Serial.begin(9600);
  
  // Attach the servo on pin 9
  myServo.attach(servoPin);
  
  // Configure ultrasonic sensor pins
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

void loop() {
  // Sweep from 0 to 180 degrees
  for (int angle = 0; angle <= 180; angle += 2) {
    myServo.write(angle);
    delay(30); // Give the servo time to reach the position
    
    int distance = getDistance();
    printRadarData(angle, distance);
  }
  
  // Sweep back from 180 to 0 degrees
  for (int angle = 180; angle >= 0; angle -= 2) {
    myServo.write(angle);
    delay(30);
    
    int distance = getDistance();
    printRadarData(angle, distance);
  }
}

// Function to measure distance using the HC-SR04 sensor
int getDistance() {
  // Clear the trigger pin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  // Set the trigger pin HIGH for 10 microseconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Read the echo pin (returns sound wave travel time in microseconds)
  long duration = pulseIn(echoPin, HIGH);
  
  // Calculate distance in centimeters (speed of sound is ~340 m/s)
  int distance = duration * 0.034 / 2;
  
  return distance;
}

// Function to neatly print data to the Serial Monitor
void printRadarData(int angle, int distance) {
  Serial.print("Angle: ");
  Serial.print(angle);
  Serial.print("° | Distance: ");
  if (distance == 0 || distance > 400) {
    Serial.println("Out of range");
  } else {
    Serial.print(distance);
    Serial.println(" cm");
  }
}
