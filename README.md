# greenhouse-monitor
Arduino greenhouse monitor that reads temperature and humidity and turns on a warning LED above 35°C. Built on Wokwi.

# 🌡️ Greenhouse Monitor

An Arduino-based greenhouse temperature and humidity monitor. A DHT22 sensor
continuously reads ambient conditions, streams live readings over Serial, and
triggers a red warning LED when the temperature crosses **35°C** — no manual
monitoring needed.

**▶️ Try it live on Wokwi (no hardware required):** https://wokwi.com/projects/478060405769497601

## How It Works

Every 2 seconds, the Arduino:

1. Reads temperature (°C) and humidity (%) from the DHT22 sensor
2. Prints both readings to the Serial Monitor (9600 baud)
3. Checks the temperature against the threshold:
   - **Above 35.0°C** → warning LED turns ON
   - **35.0°C or below** → LED stays OFF

The threshold is stored in a single variable (`maxTemp`), so it's trivial to
retune for different crops or climates.

## Hardware

| Part | Role |
|---|---|
| Arduino UNO | Microcontroller board |
| DHT22 sensor | Temperature + humidity sensing (digital, on pin 2) |
| Red LED (pin 13) | Over-temperature warning indicator |
| 220Ω resistor | LED current limiting |

## Wiring

| DHT22 Pin | Arduino UNO Pin |
|---|---|
| VCC | 5V |
| GND | GND |
| SDA (data) | D2 |

| LED | Arduino UNO Pin |
|---|---|
| Anode (+) | D13 |
| Cathode (−) | GND |

The exact layout is defined in [`diagram.json`](diagram.json) — open the Wokwi
link above to see the wired circuit.

## Files

- `sketch.ino` — the Arduino program (sensor reading, threshold check, LED control)
- `diagram.json` — Wokwi circuit definition (board, parts, wiring)

## Sample Serial Output

```
Temp: 27.40 C  Humidity: 62.10
Temp: 27.60 C  Humidity: 62.30
Temp: 36.10 C  Humidity: 60.80   ← LED turns ON here
```

## How to Run

**Option 1 — Wokwi (easiest):** click the link above, press ▶️ Play, and open
the Serial Monitor. Drag the DHT22's temperature slider past 35 to see the
LED trigger.

**Option 2 — Real hardware:** wire the circuit as above, flash the sketch with
the Arduino IDE, open Serial Monitor at 9600 baud.

## Concepts Demonstrated

- Reading digital environmental sensors (DHT library)
- Threshold-based decision logic (`if` / `else`)
- GPIO output control (`digitalWrite`)
- Serial communication for live telemetry
- Sensor polling with a timing loop (`delay`)

## Planned Improvements

- [ ] Add humidity alert (separate LED or buzzer)
- [ ] LCD display for local readings
- [ ] Data logging to SD card / serial CSV
- [ ] Vent fan / servo control when overheating
- [ ] Replace `delay()` with `millis()` for non-blocking reads
- [ ] ESP32 + Wi-Fi version with web dashboard

## Built With

- Arduino UNO (C++ / Arduino framework)
- [Wokwi](https://wokwi.com) — browser-based simulation
- Adafruit DHT sensor library

## Author

**Akash Bhat** — [github.com/AkashBGBhat](https://github.com/AkashBGBhat)


Arduino sketch.io code

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
