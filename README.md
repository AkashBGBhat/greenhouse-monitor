# greenhouse-monitor
Arduino greenhouse monitor that reads temperature and humidity and turns on a warning LED above 35°C. Built on Wokwi.
https://wokwi.com/projects/478060405769497601

    #include "DHT.h"
    
    #define DHTPIN 2
    #define DHTTYPE DHT22
    
    DHT dht(DHTPIN, DHTTYPE);
    int ledPin = 13;
    float maxTemp = 35.0;
    
    void setup() {
      Serial.begin(9600);
      dht.begin();
      pinMode(ledPin, OUTPUT);
    }
    
    void loop() {
      float temp = dht.readTemperature();
      float humidity = dht.readHumidity();
    
    Serial.print("Temp: ");
    Serial.print(temp);
    Serial.print(" C  Humidity: ");
    Serial.println(humidity);
  
    if (temp > maxTemp) {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
    delay(2000);
    }

The code below is for the diagram.json of the Greenhouse Monitor

    {
      "version": 1,
      "author": "Akash Bhat",
      "editor": "wokwi",
      "parts": [
        { "type": "wokwi-arduino-uno", "id": "uno", "top": 0, "left": 0, "attrs": {} },
        { "type": "wokwi-dht22", "id": "dht1", "top": 163.5, "left": -101.4, "attrs": {} },
        {
          "type": "wokwi-led",
          "id": "led1",
          "top": -61.2,
          "left": -53.8,
          "attrs": { "color": "red" }
        }
      ],
      "connections": [
        [ "dht1:VCC", "uno:5V", "red", [ "v0" ] ],
        [ "dht1:GND", "uno:GND.2", "black", [ "v0" ] ],
        [ "dht1:SDA", "uno:2", "green", [ "v0" ] ],
        [ "led1:A", "uno:13", "green", [ "v0" ] ],
        [ "led1:C", "uno:GND.1", "green", [ "v0" ] ]
      ],
      "dependencies": {}
    }
