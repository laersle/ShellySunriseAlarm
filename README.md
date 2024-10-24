# Shelly&reg; Sunrise Alarm
- [Introduction](#introduction)
- [Usage with Shelly&reg; Bulb RGBW](#usage-with-shelly-bulb-rgbw)
- [Usage with other Devices](#usage-with-other-devices)
- [HTTP-Data File](#http-data-file)
- [Simulation-Data File](#simulation-data-file)
- [Troubleshooting](#troubleshooting)

## Introduction
**Welcome**. First things first, if you have any problems you cannot fix after reading this manual, if you have any feedback, if you have any questions feel free to contact me: [lwss.Software.Solutions+SunriseAlarm@gmail.com](mailto:lwss.Software.Solutions+SunriseAlarm@gmail.com)

**The purpose of this App** is to control a Wifi Light Bulb to simulate a sunrise in order to wake a person using a mechanism that is drilled into our DNA for thousands of years. Especially in the winter months when the days are short I find this helpful to get out of bed when the sun is not yet up.
I have developed this App because I wanted a way to set this alarm from my phone when I lie in bed at night, a way to easily change the time the alarm goes off in the morning, a way that is **easy**.
And, as everything is done right on the phone, a way that is **secure**. There is no communication to any network/server outside the Home-Network. 

You can download the most recent App version in the Google Play Store. You will also find all the versions here as .apk files you can side load on your Android device.

## Usage with Shelly<sup>&reg;</sup> Bulb RGBW
If you are using the App with a Shelly<sup>&reg;</sup> Bulb RGBW you do not need a lot of setup to get started: 
- Enter your bulbs IP-Address: If you are using a new bulb you first need to register it in your network. Consult the manual on how to do this. If it is registered in your network you can find the IP-Address via your routers interface or in the "Shelly<sup>&reg;</sup> Smart Control App". 
- Set up your first Alarm: Simply put in the time you want the sunrise simulation to start. By default the simulation takes 30 minutes to complete. Hence, if you want to get up at 8am you should set the alarm for 7:30am.
**Tipp:** If you want the alarm to repeat the next day at the same time, you can turn on "Auto Reschedule Alarm for next Day". This way you don't need to set the alarm every day if you get up at the same time each day. 

### Customize your experience
You can customize the simulation. If the simulation is too slow or too fast, change it. You don't like the colors of the default simulation? change it. You want a sunset simulation rather than a sunrise simulation? You can also do that.

The **default Simulation** consists of 30 steps that each take 60 seconds.
- the **time inbetween Steps** can be customized in the App itself: Go to "Settings -> Show Advanced Settings -> Step Time" and enter the amount of seconds you like. If the Simulation has 30 Steps and you have a "Step Time"  of 30 seconds, that means the simulation takes 15 minutes to complete
- **changing the amount of steps or the color values**: This is a bit more complicated. You need to make changes to the "Simulation-Data" file. Read [this](#simulation-data-file) section on how to do this. You **do not** need to make changes to the "HTTP-Data" file when using a Shelly Bulb RGBW as it is setup correctly by default.
  


## Usage with other Devices
If you are using a device other than the Shelly<sup>&reg;</sup> Bulb RGBW you can still use this App as long as your device can be controlled via HTTP requests. If you are using another Shelly device (e.g. Shelly RGBW2) this is always the case. If you are using a product from a different company you will have to check if this is the case.

### Setting up the App for another Device
- Find the communication protocol to control you device via HTTP requests. You will most likely find it by checking the documentation/manual. The URL you find should look something like this `http://<ip-address>/device?r=60&g=20&b=125`
-  change the HTTP-Data file according to your devices communication protocol. When you a URL to control your device you need to break it up as described in [this](#http-data-file) Section
- change the [Simulation-Data](#simulation-data-file) file. When using a different device you will likely need to change the Simulation-Data to achieve good results. Experiment with this to find something you like

## HTTP-Data File
**Disclaimer**: This File only needs to be altered if the App is used with a device other than the Shelly<sup>&reg;</sup> Bulb RGBW.

This JSON file contains the fragments from which the URL to send commands to your device will be built. 

The file can be accessed by clicking "Show advanced settings". An App that is able to edit text files (better JSON Files) is necessary to open the File. I tested Acode for this which can be downloaded in the Playstore.

### Altering this File
The file contains multiple entries in the format `"name" : "data"` (e.g. `"url_protocol" : "http://"`). 
Only the data can be altered. The **name and the number of entries must not be changed** otherwise the App will crash during the alarm.
 If not all parameters are used they can just be left empty (e.g. `"url_parameter10" : ""`) 

When the App builds the URL to send to the device, the data will be added together.  
- The IP-address that was saved in the App will be automatically added after the `url_protocol` entry.
-  After each `url_parameter`the value from the [Simulation-Data](#simulation-data-file) file is inserted. 

The URL built from the default File will look like this: `http://<ipaddress>/light/0?turn=<sim_data1>&mode=<sim_data2>&brightness=...`

All the data in this file can be altered to build a custom URL, but **beware** that the App does not validate anything in this file which means that the App can stop working as expected if the URL is not understood by the device in the end.

## Simulation-Data File
This JSON file contains the sunrise simulation data (e.g. RGB values). If you want to customize the sunrise simulation you can do so here.

The file can be accessed by clicking "Show advanced settings". An App that is able to edit text files (better JSON Files) is necessary to open the File. I tested Acode for this which can be downloaded in the Playstore.

### Structure
The first line `"numberOfSteps" : "30"` contains the total number of steps in the simulation. It is vital that this number is **equal to the number of steps** or the App may break or misbehave during the sunrise simulation.

The rest of the entries have the format: `"stepNumber" : ["param1", "param2","param3", "param4", "param5", "param6", "param7", "param8", "param9", "param10"]`

Whatever value is in the parameter fields will be [added into the URL](#http-data-file) sent to the device to control it.

Empty fields (e.g. `""`) will be ignored.

### Changing the number of Steps
If you want to change the number of steps:
- add/remove lines at the end of the file
- when adding a line: change the `stepNumber` of that line
- change the `numberOfStep` parameter at the beginning of the file to the new total number of steps.
- when adding a line: change the `parameters` of that line


### Parameters using the Shelly<sup>&reg;</sup> Bulb RGBW
If you are using the default HTTP-Data File (with the Shelly<sup>&reg;</sup> Bulb RGBW), the values corresponding to the parameters are:

`"stepNumber" : ["turn(on/off/toggle)", "mode(color/white)","brightness(0-100)", "gain(0-100)", "temperature(3000-6500)", "r(0-255)", "g(0-255)", "b(0-255)", "w(0-255)", "unused"]`

More Information on the meaning of the values can be found in the [Shelly&reg; Documentation](https://shelly-api-docs.shelly.cloud/gen1/#shelly-bulb-rgbw-light-0)

### Parameters using a different Device
The values will need to be changed according to your [HTTP-Data](#http-data-file) file. 
However do not change the general format as described before. If you do not need a parameter field you can leave it empty.

## Troubleshooting
The App provides a log which may be helpful during troubleshooting. You can look at it by navigating to "Settings -> Show Advanced Settings -> Open Log". This requires a Text Viewer App on your Phone. 

| Issue  | possible Fix | 
|--------------------------------|------------------|
 | Alarm doesn't finish | It's possible restrictions on battery usage prevent the App from executing in the background. Try: Exclude the App from battery optimization | 
 | No connection to device | Check if the IP-Address is correct, check if the device is online by typing the URL in a browser |
 |Bad Response Exceptions| This is either because of errors in the [HTTP-Data](#http-data-file) file or the [Simulation-Data](#simulation-data-file) file, or because the IP-Address is incorrect.|
 |Time-out Exception| Check that the phone is on the same wifi-Network as the Device, check that the device is online by typing the URL in a Browser

 You can always revert to the default settings by deleting the Appdata in your phones settings
