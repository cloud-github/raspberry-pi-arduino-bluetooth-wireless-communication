# Raspberry pi and arduino bluetooth wireless communication
Bluetooth communication between raspberry pi 3 and arduino using HC-05 bluetooth module

## Components Needed
    * Raspberry pi 3 (it has inbuilt bluetooth)
    * Arduino UNO
    * HC-05 bluetooth module (for arduino side)
    

## Let's start configuring arduino uno

## ARDUINO 

Please follow this link to setup arduino with HC-05 bluetooth module

    https://www.instructables.com/id/Controlling-Arduino-UNO-Via-Smart-Phone-Using-HC-0/
Make sure you select pin 13 for led blink.

Upload this code to your arduino

    int ledPin = 13;
    
    void setup() {
      Serial.begin( 9600 );    //  baud rate 9600 for the serial Bluetooth communication
    }
    
    void loop() {
      // listen for the data from raspberry pi
      if ( Serial.available() > 0 ) {
        // read a numbers from serial port
        int inputVal = Serial.parseInt();
    
        if (inputVal > 0) {
            Serial.print("Your input is: ");
            Serial.println(String(inputVal));
            // Here blink the LED
            blinkLED(inputVal);
        }
      }
    }
    
    void blinkLED(int inputVal) {
      for (int i=0; i< inputVal; i++) {
        digitalWrite(ledPin, HIGH);
        delay(500);
        digitalWrite(ledPin, LOW);
        delay(500);
      }
    }
    
That's all for arduino part.

Let's move to raspberry pi,

You can setup bluetooth in raspberry pi via two methods.

### METHOD 1:

Installing blueman for configuring bluetooth via graphical user interface.

     sudo apt-get install pi-bluetooth
     sudo apt-get install bluetooth bluez blueman
     
Restart you pi (essential)

Now, you should be able to see menu -> preferences -> bluetooth manager

Open bluetooth manager 

click bluetooth icon (actually you will see two icons, click blue one)

click setup new device

you will see "Welcome to bluetooth device setup assistant"

process Next

Search for HC-05

Proceed further.

if every thing goes fine, you will see such screen

![alt text](https://raw.githubusercontent.com/cloud-github/raspberry-pi-arduino-bluetooth-wireless-communication/master/snapshots/2019-07-20-103556_1366x768_scrot.png)
![alt text](https://raw.githubusercontent.com/cloud-github/raspberry-pi-arduino-bluetooth-wireless-communication/master/snapshots/2019-07-20-103649_1366x768_scrot.png)
![alt text](https://raw.githubusercontent.com/cloud-github/raspberry-pi-arduino-bluetooth-wireless-communication/master/snapshots/2019-07-20-103659_1366x768_scrot.png)

Also go to Adapter menu -> Preferences , then set visibility setting - Always visible

Python code:

    import serial
    import time
    
    port = serial.Serial("/dev/rfcomm0", baudrate=9600) # reading and writing data from and to arduino serially.
                                                        # rfcomm0 -> this could be different
    
    while True:
    	print "DIGITAL LOGIC -- > SENDING..."
    	port.write(str(3))
    	rcv = port.readline()
    	if rcv:
    	   print(rcv)
    	time.sleep( 3 )

### METHOD 2:

Run Terminal:

    sudo systemctl status bluetooth
    
    sudo bluetoothctl
    
    [bluetooth]# agent on
    
    [bluetooth]# scan on
    
    [bluetooth]# scan off
    
    [bluetooth]# pair 98:D3:32:11:06:B1  # change with your HC-05 address
    
    [bluetooth]# trust 98:D3:32:11:06:B1
    
    [bluetooth]# connect 98:D3:32:11:06:B1

if everything goes fine, you should see this screen

![alt text](https://raw.githubusercontent.com/cloud-github/raspberry-pi-arduino-bluetooth-wireless-communication/master/snapshots/2019-07-19-222407_1366x768_scrot.png)

Finally, sending logic to arduino via bluetooth from serial port /dev/rfcomm0

![alt text](https://raw.githubusercontent.com/cloud-github/raspberry-pi-arduino-bluetooth-wireless-communication/master/snapshots/2019-07-19-225441_1366x768_scrot.png)



