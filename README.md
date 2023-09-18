# SWE415: Programming a Baseball Pitching Device Using Bluetooth Data Transmission
**Group members:** Joshua Delaney, Spencer Hooper, Rachel Johnston, Jacob Karpovich, Austin Pliska, Gabe Stotler

**Background:**
The Shippensburg University Varsity Baseball Team approached the Engineering department looking for a reliable form of technology that can communicate 4-digit codes from the coach to the players. Using this information, our group decided to use the Android Bluetooth approach for the project, allowing us to use a wireless connection service. The device itself is a keypad which is wired to a Kindle Fire tablet, and that tablet is then connected via Bluetooth to an Android Watch. The tablet and watch have a different UI that is suited for each device, so that sending and receiving is practical for each device. The coach is able to discreetly type a message through the keypad; when the appropriate button on the keypad is pressed, the message will be transmitted to the connected bluetooth watch.  The NCAA requires that the connection is only “one way”. This is superficially met, as the watch UI is programmed to be unable to send a code to the tablet, however, technically speaking, the connection between the watch and the tablet can be considered a “two-way” connection, since the watch can have cellular capabilities to send other messages, like text messages. Due to the lack of a hardware budget and lack of ability to create a physical device in our Agile development process, we decided this method was the next best approach, as it focuses on software development and meets the requirements of our customer. The device we created is able to transmit a 4-digit code via wireless bluetooth connection and the transmission device is able to be stored discreetly while the keypad is available for in-hand use.

**Introduction:**
Our tablet application needed three main components: the ability to connect to the watch via bluetooth, the ability to send a code provided by the coach, and the ability to clear that code from the players watches. The CONNECT button takes the user to a second screen that handles bluetooth connectivity. It allows the user to find the device they want to connect to and select it. This process needs to be done on both the watch and the phone to create a successful connection. Once the devices are connected, the keypad is used to input a code into the textbox. The coach is then given the option to send a code to the player or to clear the code from the players watch. This allows the coach to prevent the code from being stolen by an opposing team. Similar to the tablet application, the watch application also allows the user to connect to the tablet. This method allows the user to enable Bluetooth for the watch, and make it discoverable as a Bluetooth device. Then the user needs to select the phone from a list of available Bluetooth devices.  Once connected, the watch only needs to display the code that is sent by the coach’s phone. The player checks the code, recognizes what play the coach is calling, and is able to continue play.

**Bluetooth Data Transmission:**
Arguably, the most important feature of the tablet application is its ability to send codes to the watch application. To do this, the tablet must first pair with the watch, and then it must establish an RFCOM bluetooth connection using a unique UUID code as an identifier. To connect, an RFCOM server socket is generated from the tablet, and the connection request is sent to the watch application’s bluetooth API. Once the watch’s user accepts the connection request, the tablet and watch are then connected via a bluetooth connection thread. To actually transmit data, the code entered by the user is turned into bytes and is written to the bluetooth connection that has already been established by the users. Once the byte string is received by the watch’s end of the connection, it turns the string back into readable numbers and displays them in the watch’s UI.

**Tablet UI:**
The tablet application is designed to initiate a bluetooth connection with a desired device (a watch) and allow the user to send/clear codes from the connected device. The bluetooth initiation functionality is implemented in the CONNECT button of the user interface (UI). The CONNECT button sends the user to another UI that displays a BT ON/OFF button, a DISCOVERABLE button, a DISCOVER button, and a START CONNECTION button, all placed in the order they should be pushed. The BT ON/OFF button allows the user to enable/disable bluetooth on the device. Once bluetooth is enabled, the DISCOVERABLE button makes the device available to other local bluetooth devices. The DISCOVER button starts the discovery process; it finds other local bluetooth devices and displays a list of them for the user to select their desired device. The START CONNECTION button starts a direct bluetooth connection with the selected device. The data transmission functionality is implemented in the SEND and CLEAR buttons. Firstly, the text field at the top of the starting UI accepts input from the device keyboard and allows the user to type into it. The SEND button sends the text in the text field to the receiving device, which was previously connected. The CLEAR button clears the text in the textfield, as well as the text of the receiving device, and resets it to the default text. Figures 1 and 2 located below display the tablet UI.

<img width="799" alt="Screen Shot 2023-09-18 at 3 12 30 PM" src="https://github.com/racheljohnston0/BTBaseballPitchingDevice/assets/144559202/ff42ac50-b818-4b16-9f18-56a9f70d799a">

**Watch UI:**
The watch application connects to a device with the tablet application and receives the code sent from that device. When the application is opened, the user will first see a BT ON/OFF button, this enables/disables bluetooth on the watch. When the button is pressed, it will ask the user for permission to enable bluetooth from the user. Once the user approves the request, it will display the main watch UI. There will be a fixed text field and a CONNECT button. The CONNECT button initiates the bluetooth connection process, similar to the tablet UI. When the CONNECT button is pressed, it will show a DISCOVER button: once this button is clicked, it will start the discovery process, displaying the list of local bluetooth devices. The user will select their desired device from the list and press START CONNECTION, located above the list of local devices. This will take the user back to the main watch UI, as the application has now set up the watch for receiving data. The text field will display the code that is sent by the connected device with the tablet application. Figure 3 from above displays the watch UI.

**In-Hand Keypad Inegration:**
The In-Hand Keypad is an Arduino board, 16 button keypad, an I2C OLED screen, and indicator LEDs. On power, the Arduino will begin a serial output, and displays key presses on the screen. The indicator LEDs are used to alert the user if there was a problem initializing any of the components, and when a signal is successfully written to the serial output. Pressing “F” will make the Arduino write the code you input to the serial data line, and pressing “C” will clear the screen and send a clear screen signal on the data line. The android device then has an event listener for the serial data input line, set to the same baudrate (the rate at which information is transferred in a communication channel) as the Arduino board. On a signal detected, the device will search for a “$”, which indicates the start of a code, and a “x” which indicates the end of a code. It then takes the characters in between, writes them to the screen, and triggers the “Send” button to send this code to the connected bluetooth device.

<img width="593" alt="Screen Shot 2023-09-18 at 3 14 55 PM" src="https://github.com/racheljohnston0/BTBaseballPitchingDevice/assets/144559202/f6fe048c-e98a-48b2-9130-fa3101863f07">

**Results:**
The results of our project are promising. Starting with no software, and many of us with little Android App Development experience, our team was capable of creating a usable Bluetooth wireless connection Android App for (Android) tablets/phones and watches. Bluetooth data transmission and Bluetooth connection/pairing have been achieved and allow us to communicate 4-digits codes wirelessly between coach and player.  We have also created user-friendly interfaces for the tablet and watch, as seen in Fig 1, Fig 2, and Fig 3. One of the primary issues that we have encountered is in regard to the opening and closing of data transmission sockets. Sometimes, if the watch or tablet is disconnected for any amount of time, the Bluetooth data transmission receiver on the watch ceases to reopen. Although the user may be able to reconnect the devices in their respective apps, the socket remains closed, therefore, codes sent from the tablet are unable to be received by the watch. Currently, restarting both apps fixes the issue. However, further development/troubleshooting would need to be conducted to resolve this problem. The other main issue is the disconnection of the devices.  It seems that (as stated before) sometimes the Bluetooth connection services end, causing the devices to disconnect and the sockets to close, resulting in an inconvenient manual reconnect by the user. Solutions regarding a fix to this issue would be to manage a fallback BroadcastReceiver(s), however our team had very limited time to solve this issue.  Further development would also need to be conducted to resolve this issue.
