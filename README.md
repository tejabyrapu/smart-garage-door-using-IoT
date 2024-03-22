# smart-garage-door-using-IoT
int object_distance = 0;

int door_status = 0;

long readUltrasonicDistance(int pin)
{
  pinMode(pin, OUTPUT);  // Clear the trigger
  digitalWrite(pin, LOW);
  delayMicroseconds(2);
  // Sets the pin on HIGH state for 10 micro seconds
  digitalWrite(pin, HIGH);
  delayMicroseconds(10);
  digitalWrite(pin, LOW);
  pinMode(pin, INPUT);
  // Reads the pin, and returns the sound wave travel time in microseconds
  return pulseIn(pin, HIGH);
}

void setup()
{
  pinMode(3, INPUT);
  pinMode(5, OUTPUT);
  pinMode(6, OUTPUT);
  pinMode(13, OUTPUT);
}

void loop()
{
  object_distance = 0;

  object_distance = 0.01723 * readUltrasonicDistance(3); // Read the distance to an object in CM

  if (object_distance < 100) { // If an object is say a metre away (or less), open the door
    if (door_status < 1) {     // If door is closed then open it
      digitalWrite(5, HIGH);   // Open the door
      digitalWrite(6, LOW);
      digitalWrite(13, HIGH);  // Turn the red LED on
      delay(5000);             // Wait for 5 seconds for door to open
      digitalWrite(5, LOW);    // Stop the door opening
      door_status = 1;         // Door is now open
    }
  }
  else {
    if (door_status > 0) {     // Close the door only if it is open
      digitalWrite(13, LOW);   // Turn the red LED off
      delay(1000);             // Leave the door open for 1 seconds past when object last detected
      digitalWrite(5, LOW);    // Close the door
      digitalWrite(6, HIGH);
      delay(5000);             // Wait for 5 seconds for the door to close
      digitalWrite(6, LOW);    // Stop the door closing
      door_status = 0;         // Door is now closed
    }
  }
}
