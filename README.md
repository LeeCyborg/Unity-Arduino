Unity Writes Arduino via Serial Port
====================================

Tap <kbd>1</kbd> <kbd>0</kbd> keys in Unity will make Arduino LED light on/off.

## Arduino

``` 
const int ledPin = 13;
int ledState = 0;

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  ledState = recvSerial();
  delay(100);
  if (ledState == 1)
    digitalWrite(ledPin, HIGH);
  else
    digitalWrite(ledPin, LOW);
}

int recvSerial() {
  if (Serial.available()) {
    int serialData = Serial.read();
    switch (serialData) {
      case '1':
        return 1;
        break;
      case '0':
        return 0;
        break;
      default:
        return -1;
    }
  }
}

```

## Unity 3D

1. Enable Serial Port: Menu bar **File** > **Build Settings...** > Button **Player Settings...** > In the pane select tab **Other Settings** and find **Api Compatibility Level** switch the default **.Net 2.0 Subs** to **.Net 2.0**

2. Create an asset, for example an **Empty GameObject**. **Add Component** and enter `ArduinoControl` to create a **C#** file:

    ``` cs
    using UnityEngine;
    using System.Collections;
    using System.IO.Ports;

    public class ArduinoControl : MonoBehaviour {
      public string portName;
      SerialPort arduino;

      void Start () {
        arduino = new SerialPort (portName, 9600);
        arduino.Open ();
      }
      
      void Update () {
        if (arduino.IsOpen) {
          if (Input.GetKey ("1")) {
            arduino.Write ("1");
            Debug.Log (1);
          } else if (Input.GetKey ("0")) {
            arduino.Write ("0");
            Debug.Log (0);
          }
        }
      }
    }

    ```

3. Enter Serial **Port Name** in **Inspector**, for example `/dev/cu.usbmodem1411`, and run the scene.
