Index:
1) NODE MCU as a SERVER
2) ThingSpeak Code
3) Bluetooth Sensor
4) GPS Interfacing with NODEMCU


# NODE MCU as a SERVER
#include <ESP8266WiFi.h>
#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

ESP8266WebServer server(80);
void handleRoot() {
  server.send(200, "text/plain", "Hello from NodeMCU!");
}
void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
     }
  Serial.println("Connected to WiFi");
  server.on("/", handleRoot);
  server.begin();
  Serial.println("Server started");
}
void loop() {
  server.handleClient();
}

-------------------****************-----------------

# ThingSpeak Code
#include <ESP8266WiFi.h>
#include <ThingSpeak.h>

// Replace with your Wi-Fi credentials
const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

// Replace with your ThingSpeak channel details and write API key
unsigned long channelID = your_Channel_ID;
const char* writeAPIKey = "your_write_API_key";

// Analog input pin for temperature sensor
const int temperaturePin = A0;

WiFiClient client;
void setup() {
  Serial.begin(115200);

  // Connect to Wi-Fi network
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");

  // Initialize ThingSpeak client
  ThingSpeak.begin(client);
}
void loop() {
  // Read temperature sensor value
  int sensorValue = analogRead(temperaturePin);
  float temperature = (sensorValue / 1024.0) * 330.0;

  // Send temperature data to ThingSpeak
  ThingSpeak.writeField(channelID, 1, temperature, writeAPIKey);

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" degrees Celsius");

  // Wait for 10 seconds before uploading again
  delay(10000);
}

----------------------**************--------------------

# Bluetooth Sensor
#include <SoftwareSerial.h>

// Replace with your Bluetooth module pins
const int BT_TX_PIN = 2;
const int BT_RX_PIN = 3;

SoftwareSerial bluetooth(BT_RX_PIN, BT_TX_PIN); // RX, TX

void setup() {
  Serial.begin(115200);

  // Initialize Bluetooth Serial connection
  bluetooth.begin(9600);
}
void loop() {
  // Send data to Bluetooth module
  bluetooth.println("Hello, world!");

  // Wait for incoming data from Bluetooth module
  while (bluetooth.available()) {
    char incomingData = bluetooth.read();
    Serial.print(incomingData);
  }

  delay(1000); // Wait for 1 second before sending again
}

-----------------**************-------------------

# GPS Interfacing with NODEMCU
#include <SoftwareSerial.h>

// Replace with your GPS module pins
const int GPS_TX_PIN = 2;
const int GPS_RX_PIN = 3;

SoftwareSerial gpsSerial(GPS_RX_PIN, GPS_TX_PIN); // RX, TX

void setup() {
  Serial.begin(115200);
  gpsSerial.begin(9600);

  // Configure GPS module
  gpsSerial.println("$PMTK251,9600*17"); // Set baud rate to 9600
  gpsSerial.println("$PMTK300,1000,0,0,0,0*1C"); // Set update rate to 1Hz
}
void loop() {
  // Read incoming GPS data
  while (gpsSerial.available()) {
    char incomingData = gpsSerial.read();
    Serial.print(incomingData);
  }

  delay(1000); // Wait for 1 second before reading again
}
