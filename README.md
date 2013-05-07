# Bluetooth Serial Plugin for PhoneGap

This plugin enables serial communication over Bluetooth. It was written for communicating between Android and an Arduino.

## Supported Platforms

* Android

## Limitations

 * The phone must initiate the Bluetooth connection
 * Data sent over the connection is assumed to be Strings

# Installing 

Use [plugman](https://github.com/imhotep/plugman) to add BluetoothSerial to your Android project.  Plugman requires [node.js](http://nodejs.org) and is installed through npm.

Install plugman

    $ npm install -g plugman

Get the latest source code

    $ git clone https://github.com/don/BluetoothSerial.git

Install the plugin

    $ plugman --platform android --project /path/to/your/project --plugin /path/to/BluetoothSerial

Modify your HTML to include bluetoothSerial.js

    <script type="text/javascript" src="cordova-2.7.0.js"></script>
    <script type="text/javascript" src="js/bluetoothSerial.js"></script>
    <script type="text/javascript" src="js/index.js"></script>

Require bluetoothSerial in your JavaScript

    var bluetoothSerial = cordova.require('bluetoothSerial');

# API

## Methods

- [bluetoothSerial.connect](#connect)
- [bluetoothSerial.disconnect](#disconnect)
- [bluetoothSerial.write](#write)
- [bluetoothSerial.available](#available)
- [bluetoothSerial.read](#read)
- [bluetoothSerial.readUntil](#readUntil)
- [bluetoothSerial.subscribe](#subscribe)
- [bluetoothSerial.unsubscribe](#unsubscribe)
- [bluetoothSerial.clear](#clear)
- [bluetoothSerial.list](#list)
- [bluetoothSerial.isEnabled](#isEnabled)
- [bluetoothSerial.isConnected](#isConnected)

## connect

Connect to a Bluetooth device.

    bluetoothSerial.connect(macAddress, connectSuccess, connectFailure);

### Description

Function `connect` connects to a Bluetooth device.  The callback is long running.  Success will be called when the connection is successful.  Failure is called if the connection fails, or later if the connection disconnects. An error message is passed to the failure callback.

### Parameters

- __connectSuccess__: Success callback function that is invoked when the connection is successful. 
- __connectFailure__: Error callback function, invoked when error occurs or the connection disconnects.

## disconnect

Disconnect.

    bluetoothSerial.disconnect([success], [failure]);

### Description

Function `disconnect` disconnects the current connection.

### Parameters

- __success__: Success callback function that is invoked when the connection is successful. [optional]
- __failure__: Error callback function, invoked when error occurs. [optional]

## write

Writes data to the serial port.

    bluetoothSerial.write(data, success, failure);

### Description

Function `write` data to the serial port.  Data must be a String.

### Parameters

- __success__: Success callback function that is invoked when the connection is successful. [optional]
- __failure__: Error callback function, invoked when error occurs. [optional]

## available

Gets the number of bytes of data available.

    bluetoothSerial.available(success, failure);

### Description

Function `available` gets the number of bytes of data available.  The bytes are passed as a parameter to the success callback.

### Parameters

- __success__: Success callback function that is invoked when the connection is successful. [optional]
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    bluetoothSerial.available(function (numBytes) {
        console.log("There are " + numBytes + " availble to read.");
    }, failure);

## read

Reads data from the buffer.

    bluetoothSerial.read(success, failure);

### Description

Function `read` reads the data from the buffer. The data is passed to the success callback as a String.  Calling `read` when no data is available will pass an empty String to the callback.

### Parameters

- __success__: Success callback function that is invoked with the number of bytes available to be read.
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    bluetoothSerial.read(function (data) {
        console.log(data);
    }, failure);

## readUntil

Reads data from the buffer until it reaches a delimiter.

    bluetoothSerial.readUntil('\n', success, failure);

### Description

Function `readUntil` reads the data from the buffer until it reaches a delimiter.  The data is passed to the success callback as a String.  If the buffer does not contain the delimiter, an empty String is passed to the callback. Calling `read` when no data is available will pass an empty String to the callback.

### Parameters

- __delimiter__: delimiter
- __success__: Success callback function that is invoked with the data.
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    bluetoothSerial.readUntil('\n', function (data) {
        console.log(data);
    }, failure);
    
## subscribe 

Subscribe to be notified when data is received.

    bluetoothSerial.subscribe('\n', success, failure);

### Description

Function `subscribe` registers a callback that is called when data is received.  A delimiter must be specified.  The callback is called with the data as soon as the delimiter string is read.  The callback is a long running callback and will exist until `unsubscribe` is called.

### Parameters

- __delimiter__: delimiter
- __success__: Success callback function that is invoked with the data.
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    // the success callback is called whenever data is received
    bluetoothSerial.subscribe('\n', function (data) {
        console.log(data);
    }, failure);

## unsubscribe

Unsubscribe from a subscription.

    bluetoothSerial.unsubscribe(success, failure);

### Description

Function `unsubscribe` removes any notification added by `subscribe` and kills the callback.

### Parameters

- __success__: Success callback function that is invoked when the connection is successful. [optional]
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    bluetoothSerial.unsubscribe();

## clear

Clears data in the buffer.

    bluetoothSerial.clear(success, failure);

### Description

Function `clear` removes any data from the receive buffer.

### Parameters

- __success__: Success callback function that is invoked when the connection is successful. [optional]
- __failure__: Error callback function, invoked when error occurs. [optional]

## list

Lists bonded devices

    bluetoothSerial.list(success, failure);
    
### Description

Function `list` lists and paired Bluetooth devices.  The success callback is called with a list of objects.

Example list passed to success callback.  See [BluetoothDevice](http://developer.android.com/reference/android/bluetooth/BluetoothDevice.html#getName()) and [BluetoothClass#getDeviceClass](http://developer.android.com/reference/android/bluetooth/BluetoothClass.html#getDeviceClass()).

    [{
        "class": 276,
        "address": "10:BF:48:CB:00:00",
        "name": "Nexus 7"
    }, {
        "class": 7936,
        "address": "00:06:66:4D:00:00",
        "name": "RN42"
    }]
    
### Parameters

- __success__: Success callback function that is invoked with a list of bonded devices.
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    bluetoothSerial.list(function(devices) {
        devices.forEach(function(device) {
            console.log(device.address);
        })
    }, failure);
    
## isConnected

Reports the connection status. 

    bluetoothSerial.isConnected(success, failure);

### Description

Function `isConnected` passes true to the success callback when connected to another device.

### Parameters

- __success__: Success callback function that is invoked with a boolean for connected status.
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    bluetoothSerial.isConnected(function (connected) {
        console.log(connected); // true or false
    }, failure);   
    
## isEnabled

Reports if bluetooth is enabled.

    bluetoothSerial.isEnabled(success, failure);

### Description

Function `isEnabled` passes true to the success callback when bluetooth is enabled.

### Parameters

- __success__: Success callback function that is invoked with a boolean for connected status.
- __failure__: Error callback function, invoked when error occurs. [optional]

### Quick Example

    bluetoothSerial.isEnabled(function (enabled) {
        console.log(enabled); // true or false
    }, failure); 

# Misc

## Where does this work? 
            
This plugin was developed with PhoneGap 2.7.0 using Android 4.2.2 on a Nexus 4 connecting to a [Sparkfun Mate Silver](https://www.sparkfun.com/products/10393). The code should be generic and work with most hardware.
    
## Props

Most of the Bluetooth implementation was borrows from the Bluetooth Chat example in the Android SDK.

The API for available, read, readUntil was influenced by the [BtSerial Library for Processing for Arduino](https://github.com/arduino/BtSerial)

## Wrong Bluetooth Plugin?

If you don't need **serial** over Bluetooth, try the [PhoneGap Bluetooth Plugin for Android](https://github.com/phonegap/phonegap-plugins/tree/master/Android/Bluetooth)

## Feedback
    
Try the code. If you find an problem or missing feature, file an issue or create a pull request.

