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
