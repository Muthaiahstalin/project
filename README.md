# project
fire fighting robots
// Pin Definitions
const int flameSensorLeft = A0;   // Left flame sensor
const int flameSensorRight = A1;  // Right flame sensor
const int motorLeft1 = 2;         // Left motor forward
const int motorLeft2 = 3;         // Left motor backward
const int motorRight1 = 4;        // Right motor forward
const int motorRight2 = 5;        // Right motor backward
const int pumpRelay = 6;          // Relay for water pump
const int threshold = 500;        // Flame sensor threshold (adjust based on sensor)

void setup() {
  // Initialize pins
  pinMode(flameSensorLeft, INPUT);
  pinMode(flameSensorRight, INPUT);
  pinMode(motorLeft1, OUTPUT);
  pinMode(motorLeft2, OUTPUT);
  pinMode(motorRight1, OUTPUT);
  pinMode(motorRight2, OUTPUT);
  pinMode(pumpRelay, OUTPUT);
  
  // Start with pump off
  digitalWrite(pumpRelay, LOW);
  
  // Serial for debugging (optional)
  Serial.begin(9600);
}

void loop() {
  // Read flame sensor values
  int leftFlame = analogRead(flameSensorLeft);
  int rightFlame = analogRead(flameSensorRight);
  
  // Debugging output
  Serial.print("Left: ");
  Serial.print(leftFlame);
  Serial.print(" Right: ");
  Serial.println(rightFlame);
  
  // Check if fire is detected
  if (leftFlame < threshold || rightFlame < threshold) {
    stopMotors();           // Stop moving
    activatePump();         // Turn on water pump
    delay(2000);            // Spray water for 2 seconds
    deactivatePump();       // Turn off pump
    
    // Decide movement based on flame location
    if (leftFlame < rightFlame) {
      turnLeft();           // Fire is more on the left
      delay(500);
    } else if (rightFlame < leftFlame) {
      turnRight();          // Fire is more on the right
      delay(500);
    }
  } else {
    moveForward();          // No fire detected, keep searching
  }
}

// Motor control functions
void moveForward() {
  digitalWrite(motorLeft1, HIGH);
  digitalWrite(motorLeft2, LOW);
  digitalWrite(motorRight1, HIGH);
  digitalWrite(motorRight2, LOW);
}

void turnLeft() {
  digitalWrite(motorLeft1, LOW);
  digitalWrite(motorLeft2, HIGH);
  digitalWrite(motorRight1, HIGH);
  digitalWrite(motorRight2, LOW);
}

void turnRight() {
  digitalWrite(motorLeft1, HIGH);
  digitalWrite(motorLeft2, LOW);
  digitalWrite(motorRight1, LOW);
  digitalWrite(motorRight2, HIGH);
}

void stopMotors() {
  digitalWrite(motorLeft1, LOW);
  digitalWrite(motorLeft2, LOW);
  digitalWrite(motorRight1, LOW);
  digitalWrite(motorRight2, LOW);
}

// Pump control functions
void activatePump() {
  digitalWrite(pumpRelay, HIGH);  // Turn on pump
}

void deactivatePump() {
  digitalWrite(pumpRelay, LOW);   // Turn off pump
}
