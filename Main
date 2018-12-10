//#include <RH_ASK.h>
#include <WiFi.h>
#include <SPI.h>
#include "ThingSpeak.h"
#include <Servo.h>
#include <Wire.h>
#include <TaskScheduler.h>

Scheduler ts;
void task1Callback();
void task2Callback();
Task t1 (50, TASK_FOREVER, &task1Callback, &ts, true);
Task t2 (60000, TASK_FOREVER, &task2Callback, &ts, true);

//RH_ASK rf_driver;
char ssid[] = "Herbie Plus Net";
char pass[] = "Amelia33";
//int status = WL_IDLE_STATUS;     // the Wifi radio's status
WiFiClient  client;
unsigned long myChannelNumber = 646382;
const char * myWriteAPIKey = "USSEG587DG7B2ETI";

Servo myservo;
int x = 0;
int val = 0;

int inputPin = 8;
int buzzer = 9;
int buttonPin1 = 2;
int buttonPin2 = 3;
int ledW = 4;
int ledG = 5;
int ledR = 6;
//DONT USE PIN 7
//SERVO ON 10

//11 NO
//12 NO
//13 NO


int sendField1 = 0;
int sendField2 = 0;
int sendField3 = 0;
int sendField4 = 0;
int sendField5 = 0;
int sendField6 = 0;
int sendField7 = 0;
int sendField8 = 0;

int pirState = LOW;
String alarm = "No";
String status = "Unset";
int pos = 0;

void setup() {
  ThingSpeak.begin(client);
  // initialize serial:
  //rf_driver.init();
  Serial.begin(9600);
  //Wire.begin(9);
  //Wire.onReceive(receiveEvent);

  pinMode(ledR, OUTPUT);
  pinMode(ledW, OUTPUT);
  pinMode(buttonPin1, INPUT_PULLUP);
  pinMode(buttonPin2 , INPUT_PULLUP);
  pinMode(ledG, OUTPUT);
  pinMode(buzzer, OUTPUT);
  //pinMode(inputPin, INPUT);

  //myservo.attach(8);
  welcome();
  wifisetup();
  //resetLock();

}

/*void receiveEvent(int bytes) {
  x = Wire.read();    // read one character from the I2C
  }*/

void welcome()
{
  digitalWrite(ledR, HIGH);
  digitalWrite(ledW, HIGH);
  digitalWrite(ledG, HIGH);
  delay(500);
  digitalWrite(ledR, LOW);
  digitalWrite(ledW, LOW);
  digitalWrite(ledG, LOW);
}

void loop() {
  ts.execute();

  /*// Set buffer to size of expected message
    uint8_t buf[24];
    uint8_t buflen = sizeof(buf);
    // Check if received packet is correct size
    if (rf_driver.recv(buf, &buflen))
    {
    // Message received with valid checksum
    Serial.print("Message Received: ");
    Serial.println((char*)buf);
    //servoUnlock();
    digitalWrite(ledG, HIGH);
    delay(500);
    digitalWrite(ledG, LOW);
    tone(buzzer, 1000); // Send 1KHz sound signal...
    delay(1000);        // ...for 1 sec
    noTone(buzzer);     // Stop sound...
    delay(1000);        // ...for 1sec
    }*/
    val = digitalRead(inputPin);
    if (val == HIGH) {
    if (pirState == LOW) {
      Serial.println("Activity detected!");
      pirState = HIGH;
    }
    } else {
    if (pirState == HIGH) {
      Serial.println("Activity ended!");
      sendField4+=1;
      pirState = LOW;
    }
    }
}

/*void resetLock()
{
  Serial.println("Reseting");
  sendField1 = +1;
  buzz();
  //servoLock();
  Serial.println("Reset");
}*/
void wifisetup()
{
  Serial.println("Attempting to connect to Network");
  WiFi.begin(ssid, pass);
  Serial.println("Connected to network");
  digitalWrite(ledG, HIGH);
  digitalWrite(ledW, HIGH);
  delay(500);
  digitalWrite(ledG, LOW);
  digitalWrite(ledW, LOW);
  delay(500);
  digitalWrite(ledW, HIGH);
  digitalWrite(ledG, LOW);
}

/*void siren()
{
  //int x = ThingSpeak.writeField(myChannelNumber, 5, 1, myWriteAPIKey);
  delay(500);
  while (alarm == "Yes")
  {
    for (int i = 700; i < 800; i++) {
      tone(buzzer, i);
      delay(15);
    }
    for (int j = 800; j > 700; j--) {
      tone(buzzer, j);
      delay(15);
    }
  }
  noTone(buzzer);     // Stop sound...
}
void fireSiren()
{
  //int x = ThingSpeak.writeField(myChannelNumber, 1, 6, myWriteAPIKey);
  delay(500);
  while (alarm == "Yes")
  {
    for (int i = 700; i < 800; i++) {
      tone(buzzer, i);
      delay(15);
    }
    for (int j = 800; j > 700; j--) {
      tone(buzzer, j);
      delay(15);
    }
  }
  noTone(buzzer);     // Stop sound...
}

void servoLock()
{
  Serial.println("Locking");
  digitalWrite(ledR, HIGH);
  myservo.write(90);
  delay(15);
  buzz();
  delay(15);
  digitalWrite(ledR, LOW);
  Serial.println("Locked");
}*/
void buzz()
{
  tone(buzzer, 1000);
  delay(500);
  noTone(buzzer);
}
void servoUnlock()
{
  Serial.println("Unlocking");
  digitalWrite(ledG, HIGH);
  delay(15);
  buzz();
  delay(15);
  digitalWrite(ledG, LOW);
  Serial.println("Unlocked");
}

void task2Callback() {
  Serial.println("Sending Data");
  ThingSpeak.setField(1, sendField1);
  ThingSpeak.setField(2, sendField2);
  ThingSpeak.setField(3, sendField3);
  ThingSpeak.setField(4, sendField4);
  ThingSpeak.setField(6, sendField6);
  ThingSpeak.setField(7, sendField7);
  ThingSpeak.setField(8, sendField8);
  Serial.println(sendField1);
  Serial.println(sendField2);
  Serial.println(sendField3);
  Serial.println(sendField4);
  Serial.println(sendField6);
  Serial.println(sendField7);
  Serial.println(sendField8);
  ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);
  Serial.println("Data Sent");
  sendField1 = 0;
  sendField2 = 0;
  sendField3 = 0;
  sendField4 = 0;
  sendField5 = 0;
  sendField6 = 0;
  sendField7 = 0;
  sendField8 = 0;
  Serial.println("Data Reset");
}
void task1Callback() {

  if (digitalRead(buttonPin1)==LOW)
  {
    sendField3 += 1;
    Serial.println("Happy");
    digitalWrite(ledR, HIGH);
    buzz();
    delay(500);
    //servoUnlock();
    digitalWrite(ledR, LOW);
  }
  else if (digitalRead(buttonPin2)==LOW)
  {
    sendField7 += 1;
    Serial.println("Sad");
    digitalWrite(ledG, HIGH);
    buzz();
    //servoLock();
    delay(500);
    digitalWrite(ledG, LOW);
  }
}
