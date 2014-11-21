---
layout: project
title: Home Security with Edison
author: Adam Magaluk
difficulty: experienced
duration: 1-3 hours
description: >
  Create an Internet-connected, home security system with a microphone, piezo speaker, LED, LightBlue Bean and an Intel Edison.
repo: https://github.com/zettajs/zetta-security-system-edison
tags: 
- Intel Edison
- LightBlue Bean
---

# Directions

1. [Setup the Edison and PC](#step-1-setup-the-edison-and-pc)
1. [Blink The LED](#step-2-blink-the-led).
1. [Buzz the Buzzer](#step-2-buzz-the-piezo-buzzer)
1. [Soundcheck the Microphone](#step-3-soundcheck-the-microphone)
1. [Secure the Area](#step-4-secure-the-area)
1. [Link to the Internet](#step-5-link-to-the-internet)
1. [Add a Remote Device](#step-6-add-a-remote-device)
{:.steps}

# Goal

The goal for this project is to create a simple home security system by assembling a microphone, a piezo speaker and an LED into a Zetta app running on an Intel Edison. We will also use Bluetooth low energy (BLE) to communicate with a LightBlue Bean's on-board temperature and accelerometer sensors plus an additional LED. We will connect the app to the Internet by linking the Edison with a second Zetta server running in the cloud.

![The Connected Microphone](/images/projects/security_system_edison/hardware/led_birdseye.jpg)

![Final Project](/images/projects/security_system_edison/hookup_diagram_step_4.png)

> **downloadcloud**{:.icon} Download the [Fritzing](http://fritzing.org) diagram for the finished project: [home_security_system.fzz](/images/projects/security_system_edison/fritzing/home_security_system.fzz).

# Parts

![All Materials](/images/projects/security_system_edison/hardware/empty_low.jpg){:.zoom .full}

<script src="https://www.sparkfun.com/wish_lists/98550.js"></script>

> **cart**{:.icon} [Buy the parts](https://www.sparkfun.com/wish_lists/98550) for the Home Security project from [SparkFun](http://www.sparkfun.com).

# Step #1: Setup the Edison and PC

## Connect the Edison

Follow the guide on [How to Connect an Edison to the Internet via a PC](https://communities.intel.com/docs/DOC-23148).


## Setting up the PC

1. Ensure you have a code editor on the PC.

   > **help**{:.icon} Need a code editor? Try [Atom](https://atom.io/) or [Sublime Text](http://www.sublimetext.com/).

1. Ensure you have [node.js](http://nodejs.org/) installed on the PC.

   > **help**{:.icon} Need to install node.js? Click the INSTALL button at [node.js](http://nodejs.org/).

1. From the PC terminal, install the edison-cli from the the command line.

   ```bash
   npm install -g edison-cli
   ```

   > **help**{:.icon} Problem with installation? Try `sudo npm install -g edison-cli`.

## Clone the Starter Code to the PC


1. From the PC terminal, clone [the Zetta starter project](https://github.com/zettajs/zetta-starter-project) to a new `zetta-home-security` directory.

   ```bash
   git clone https://github.com/zettajs/zetta-starter-project zetta-home-security
   ```

   > **help**{:.icon} Problem with `git clone`? Try downloading the zip file from [https://github.com/zettajs/zetta-starter-project](https://github.com/zettajs/zetta-starter-project)

## Install Zetta

1. From the PC terminal, `cd` to `zetta-home-security`.

   ```bash
   cd zetta-home-security
   ```

1. From the PC terminal, install Zetta with [NPM](/reference/2014/10/12/npm.html).

   ```bash
   npm install
   ```
   
## Write Zetta Server Code

1. Write code in `server.js` to `require` Zetta, give the server a `name` and `listen` on server port `1337`.

   > **info**{:.icon} Choose a name for the server. Consider using your first and last name.

   ```javascript
   var zetta = require('zetta');

   zetta()
     .name('FirstName-LastName')
     .listen(1337, function(){
        console.log('Zetta is running at http://127.0.0.1:1337');
   });
   ```

## Connect PC to Intel Edison

1. Connect the PC to the Intel Edison development board via a USB cable.

    From                  | Wire           | To  
    :----                 |:-----:         |----:
    PC **USB A-Female**   |**USB**         |Edison **USB Micro J16**
    {:.wiring}

> **clock**{:.icon} Upon connecting the Edison to the PC, a few minutes will elapse before the Edison is visible on the local WiFi network.

## Run the Zetta Server

1. Run the server on the PC.

   ```bash
   node server.js
   ```
   Notice the console output indicating the server is running.

   ```bash
   Nov-20-2014 22:56:39 [server] Server (FirstName-LastName) FirstName-LastName listening on http://127.0.0.1:1337
   Zetta is running at http://127.0.0.1:1337
   ```

   Upon ensuring the server will run on the PC, stop the PC server.

1. Locate the Edison on the network.

   ```bash
   edison-cli list
   ```

1. Ensure the command line output looks like the results below.

   ```bash
   Edison Devices Found: 1
   1 - {ip address}
   ```
   {:.language-bash-noln}

   Remember the `{ip address}` to use in the commands that follow.

1. Run the Zetta server on the Edison

   ```bash
   edison-cli -H {ip address} deploy
   ```

   Ensure the console begins with output like this:

   ```bash
   XDK - IoT App Dameon v0.0.13 - commands: run, list, debug, status

   XDK Message Received: clean

   |================================================================
   |    Intel (R) IoT - NPM Rebuild - (may take several minutes)
   |================================================================
   ```

   And ends with output like this:

   ```bash
   => Stopping App <=
   Application restarted
   Nov-21-2014 04:01:32 [server] Server (FirstName-LastName) FirstName-LastName listening on http://127.0.0.1:1337
   Zetta is running at http://127.0.0.1:1337
   ```

## Call the Zetta API

1. In the terminal, call Zetta's web API.

   ```bash
   curl http://{ip address}:1337
   ```

   Ensure the API response is similar to the response below.

   ```json
   {
     "actions": [
       {
         "fields": [
           {
             "name": "server",
             "type": "text"
           },
           {
             "name": "ql",
             "type": "text"
           }
         ],
         "href": "http://10.1.10.53:1337/",
         "method": "GET",
         "name": "query-devices",
         "type": "application/x-www-form-urlencoded"
       }
     ],
     "class": [
       "root"
     ],
     "links": [
       {
         "href": "http://10.1.10.53:1337/",
         "rel": [
           "self"
         ]
       },
       {
         "href": "http://10.1.10.53:1337/servers/FirstName-LastName",
         "rel": [
           "http://rels.zettajs.io/server"
         ],
         "title": "FirstName-LastName"
       },
       {
         "href": "http://10.1.10.53:1337/peer-management",
         "rel": [
           "http://rels.zettajs.io/peer-management"
         ]
       }
     ]
   }
   ```
   {:.language-json-noln}

   > **info**{:.icon} As we `use` devices in `server.js` they will appear in the web API. For the following steps we'll access the API via the [Zetta Browser](/guides/2014/10/18/Zetta-Browser.html).

# Step #2: Blink the LED

## Write the LED Code

1. Ensure the working directory is `zetta-home-security`. Install the Zetta device driver for Edison LEDs.

   ```bash
   npm install zetta-led-edison-driver --save
   ```

1. In the `server.js` file, write code to `require` and `use` the Edison on-board `LED`: 13.

   Add **line 2**:

   ```javascript
   var LED = require('zetta-led-edison-driver');
   ```
   Add **line 6**:

   ```javascript
   .use(LED, 13)
   ```

1. Ensure `server.js` looks like the code below.
   
   ```javascript
   var zetta = require('zetta');
   var LED = require('zetta-led-edison-driver');

   zetta()
    .name('FirstName-LastName')
    .use(LED, 13)
    .listen(1337, function(){
      console.log('Zetta is running at http://127.0.0.1:1337');
   });
   ```

1. Stop and restart the Zetta server.

   ```bash
   edison-cli -H {ip address} deploy
   ```

1. When Zetta discovers the LED, it will log a message about the device to the console.

   ```bash
   {timestamp} [scout] Device (led) {id} was discovered
   ```
   {:.language-bash-noln}

## Blink the LEDs from the Edison

1. Open the Zetta Browser. Point it to the **Edison server**.

   [http://browser.zettajs.io](http://browser.zettajs.io)

1. Ensure the **LED** devices are listed.

   ![Zetta Browser with LEDs](/images/projects/security_system/screens/browser-leds.png){:.zoom}

1. Click the `turn-on` button for the various LEDs.

1. Ensure the LEDs on the Edison turned on and that the device state changed in the Zetta Browser visualization.

# Step #3: Buzz the Piezo Buzzer

## Assemble the Buzzer Hardware

![Piezo Hookup Diagram](/images/projects/security_system_edison/hookup_diagram_step_1.png){:.fritzing}

1. Attach the piezo buzzer to the breadboard.

    From              | To
    :----             |----:
    Buzzer **-** pin  |Breadboard **A3**
    Buzzer **+** pin  |Breadboard **A6**
    {:.wiring}

    > **help**{:.icon} New to solderless breadboards? Read the [How to Use a Breadboard](/guides/2014/10/07/Breadboard.html) guide.

1. Create a circuit between the Edison and the buzzer.

    From                  | Wire           | To  
    :----                 |:-----:         |----:
    Breadboard **E3**     |**Green**       |Edison **DIGITAL ~3**
    Breadboard **E6**     |**Black**       |Breadboard **-**
    Breadboard **-**      |**Black**       |Edison **GND**
    {:.wiring}

After assembling the buzzer hardware, the project should look similar to the images below.

![The Connected Piezo Buzzer](/images/projects/security_system_edison/hardware/piezo_birdseye.jpg){:.zoom}
![The Connected Piezo Buzzer](/images/projects/security_system_edison/hardware/piezo_low.jpg){:.zoom}

## Write the Buzzer Software

1. From the PC's command line, install the Zetta device driver for the buzzer.

   ```bash
   npm install zetta-buzzer-edison-driver --save
   ```

2. Create a new file called `server.js` in the project directory.

3. In the `server.js` file, write Zetta code to `require` and `use` the `Buzzer` driver on Edison pin `3` and `listen` on server port `1337`.

   ```javascript
   var zetta = require('zetta');
   var Buzzer = require('zetta-buzzer-edison-driver');

   zetta()
     .use(Buzzer, 3)
     .listen(1337, function(){
       console.log('Zetta is running at http://127.0.0.1:1337');
     });
   ```

   > **info**{:.icon} You can test that the code is written properly by running `node server.js` on the PC.


4. Initialize the `package.json` file for the project. NPM will use the newly created `server.js` file as the `main` file.

   ```bash
   npm init
   ```

   > **info**{:.icon} Accept all defaults, by pressing `enter` at each command line prompt.

## Deploy Buzzer Software to the Edison

2. Deploy and run the Zetta server and buzzer code.

   ```bash
   edison-cli -H {ip address} deploy
   ```

1. Ensure the build output looks like the output below.

   ```bash
   XDK - IoT App Daemon v0.0.13 - commands: run, list, debug, status

   XDK Message Received: clean

   |================================================================
   |    Intel (R) IoT - NPM Rebuild - (may take several minutes)
   |================================================================

   ...

   |================================================================
   |    NPM REBUILD COMPLETE![ 0 ]   [ 0 ]
   |================================================================

   XDK Message Received: run
   => Stopping App <=
   Application restarted
   ```
   {:.language-bash-noln}
  
   > **clock**{:.icon} Running `deploy` the first time will take a few minutes to build the npm modules that require native code.

   > **info**{:.icon} The code that was deployed to the Edison resides at `/node_app_slot`.

1. Ensure that a message about the buzzer device is displayed in the console and looks like the output below.

   ```bash
   {TIMESTAMP} [scout] Device (buzzer) {GUID} was provisioned from registry
   Zetta is running at http://127.0.0.1:1337
   ```

## Buzz the Buzzer

1. Open the Zetta Browser: [http://browser.zettajs.io](http://browser.zettajs.io)

  Enter the address to the Edison `http://{ip address}:1337` in the box.

1. Ensure the **Buzzer** device is listed.
![Zetta Browser with Piezo Attached](/images/projects/security_system_edison/screens/browser-piezo.png){:.zoom}

1. Click the `beep` button.

1. Ensure that the buzzer buzzed and the device state changed in the Zetta Browser visualization.

   > **help**{:.icon} Didn't hear a beep? Double check the wiring and make sure there were no errors reported.

# Step #3: Soundcheck the Microphone

## Assemble Microphone Hardware

![Microphone Hookup Diagram](/images/projects/security_system_edison/hookup_diagram_step_2.png){:.fritzing}

1. If the microphone does not have headers attached, solder them in place so the microphone can be attached to the breadboard.

   > **help**{:.icon} New to soldering? Read the [How to Solder](/guides/2014/10/13/solder.html) guide.

2. Attach the microphone to the breadboard.

    From                  | To  
    :----                 |----:
    Microphone **VCC**    |Breadboard **F18**
    Microphone **GND**    |Breadboard **F19**
    Microphone **AUD**    |Breadboard **F20**
    {:.wiring}

3. Create a circuit between the Edison and the microphone.

    From                 | Wire                     | To  
    :----                |:-----:                   |----:
    Breadboard **H18**   |**Red**                   |Edison **3.3V**
    Breadboard **H19**   |**2.2k&#8486;** Resistor  |Breadboard **-**
    Breadboard **H20**   |**Green**                 |Edison **A0**
    {:.wiring}

   > **help**{:.icon} Don't know how to read resistor values? Read the [How to Read Resistor Values](/guides/2014/10/13/2014.html) guide.

After assembling the microphone hardware, the project should look similar to the images below.

![The Connected Microphone](/images/projects/security_system_edison/hardware/microphone_birdseye.jpg){:.fritzing}
![The Connected Microphone](/images/projects/security_system_edison/hardware/microphone_low.jpg){:.fritzing}

# Write Microphone Software

1. From the PC's command line, install the Zetta device driver for the microphone.

   ```bash
   npm install zetta-microphone-edison-driver --save
   ```

1. In the `server.js` file, write Zetta code to `require` and `use` the `Microphone` driver on Edison's analog pin `0`.

   Add **line 3**:

   ```javascript
   var Microphone = require('zetta-microphone-edison-driver');
   ```
   Add **line 7**:

   ```javascript
   .use(Microphone, 0)
   ```

1. Ensure `server.js` looks like the code below.

   ```javascript
   var zetta = require('zetta');
   var Buzzer = require('zetta-buzzer-edison-driver');
   var Microphone = require('zetta-microphone-edison-driver');

   zetta()
     .use(Buzzer, 3)
     .use(Microphone, 0)
     .listen(1337, function(){
       console.log('Zetta is running at http://127.0.0.1:1337');
     });
   ```

1. Deploy the new code using the `edison-cli`

   ```bash
   edison-cli -H {ip address} deploy
   ```

1. When Zetta discovers the microphone, Zetta will log a message about the device to the output.

   ```bash
   Zetta is running at http://127.0.0.1:1337
   {TIMESTAMP} [scout] Device (buzzer) {GUID} was provisioned from registry.
   {TIMESTAMP} [scout] Device (microphone) {GUID} was discovered
   ```
   {:.language-bash-noln}

## Soundcheck the Microphone

1. Open the Zetta Browser and ensure the **Microphone** device is listed.
   ![Zetta Browser root with Microphone](/images/projects/security_system_edison/screens/browser-microphone.png){:.zoom}

1. In the Zetta Browser, click on the **Microphone** link to open a detailed view of the device.

   ![Zetta Browser root with Microphone](/images/projects/security_system_edison/screens/zetta-browser-microphone-show.png){:.zoom}

1. Make a noise near or gently tap on the microphone.

1. Ensure the values and waveform for the `:volume` characteristic in the Zetta Browser are streaming over time and change as you make noise.

# Step #4: Secure the Area

## Create Security App File and Folder

1. From the PC's command line, create the app file and directory.

   ```bash
   touch apps/app.js
   ```
   {:.language-bash-noln}

## Write  Security App Logic

1. Write the app logic in `apps/app.js`.

```javascript
module.exports = function(server) {
  var buzzerQuery = server.where({ type: 'buzzer' });
  var microphoneQuery = server.where({ type: 'microphone' });

  server.observe([buzzerQuery, microphoneQuery], function(buzzer, microphone){
    microphone.streams.volume.on('data', function(msg){
      if (msg.data > 600) {
        buzzer.call('turn-on-pulse', function(){});

        setTimeout(function() {
          buzzer.call('turn-off', function(){});
        }, 3000);
      }
    });
  });
}
```

## Use Security App in the Zetta Server

1. Edit the `server.js` file. Add Zetta code to `require` and `use` the `app` from the `apps` folder.

   Add **line 5**.

   ```javascript
   var app = require('./apps/app');
   ```

   Add **line 10**.

   ```javascript
   .use(app)
   ```

1. Ensure `server.js` looks like the code below.

   ```javascript
   var zetta = require('zetta');
   var Buzzer = require('zetta-buzzer-edison-driver');
   var Microphone = require('zetta-microphone-edison-driver');

   var app = require('./apps/app');

   zetta()
   .use(Buzzer, 3)
   .use(Microphone, 0)
   .use(app)
   .listen(1337, function(){
     console.log('Zetta is running at http://127.0.0.1:1337');
   });
   ```

## Secure the Area

1. Deploy the new code using the `edison-cli`

   ```bash
   edison-cli -H {ip address} deploy
   ```

1. Make a noise near or gently tap on the microphone.

1. Ensure that the alarm beeps when you make a loud noise.

1. Open the Zetta Browser to observe state changes:

# Step #5: Link to the Internet

## Create link between two Zetta nodes

1. Ensure `server.js` looks like the code below.

```javascript
var zetta = require('zetta');
var Buzzer = require('zetta-buzzer-edison-driver');
var Microphone = require('zetta-microphone-edison-driver');

var app = require('./apps/app');

zetta()
  .use(Buzzer, 3)
  .use(Microphone, 0)
  .use(app)
  .link('http://hello-zetta.herokuapp.com/')
  .listen(1337, function(){
    console.log('Zetta is running at http://127.0.0.1:1337');
  });
```

2. Deploy the new code using the `edison-cli`

   ```bash
   edison-cli -H {ip address} deploy
   ```

## Investigate a new Zetta server

1. Open the Zetta Browser from a new location.

  [http://browser.zettajs.io/#/overview?url=http:%2F%2Fhello-zetta.herokuapp.com](http://browser.zettajs.io/#/overview?url=http:%2F%2Fhello-zetta.herokuapp.com)

2. Observe the state changes occurring, and interact with the system from the open internet.

# Step #6 Add A Remote Device

1. Make sure the Bean has its battery plugged in.

## Add the Bean to our Zetta code

![Bean Hookup Diagram](/images/projects/security_system_edison/hookup_diagram_step_3.png){:.fritzing}

1. Ensure `server.js` looks like the code below.

```javascript
var zetta = require('zetta');
var Buzzer = require('zetta-buzzer-edison-driver');
var Microphone = require('zetta-microphone-edison-driver');
var Bean = require('zetta-bean-driver');

var app = require('./apps/app');

zetta()
  .use(Buzzer, 3)
  .use(Microphone, 0)
  .use(Bean, 'Bean1') // Note Bean1 is the beans name.
  .use(app)
  .link('http://hello-zetta.herokuapp.com/')
  .listen(1337, function(){
    console.log('Zetta is running at http://127.0.0.1:1337');
  });
```

2. Deploy the new code using the `edison-cli`

   ```bash
   edison-cli -H {ip address} deploy
   ```

# Step #7: Blink the LED

## Assemble Light Hardware

![Microphone Hookup Diagram](/images/projects/security_system_edison/hookup_diagram_step_4.png){:.fritzing}

1. Add the LED to the breadboard.
  * *Annode* (long leg) on Breadboard **A26**
  * *Cathode* (short leg) on Breadboard **A28**
  * Place AUD on Breadboard **F20**
2. Connect the wires in the following way:

    From                 | Wire                 | To  
    :----                |:-----:               |----:
    Breadboard **I26**   |Orange                |Edison **D13**
    Breadboard **C26**   |47&#8486; Resistor    |Breadboard **G26**
    Breadboard **E28**   |Black                 |Breadboard's negative column
    {:.wiring}

the hardware setup should look like this when you're done:

![The Connected Microphone](/images/projects/security_system_edison/hardware/led_birdseye.jpg){:.fritzing}
![The Connected Microphone](/images/projects/security_system_edison/hardware/led_low.jpg){:.fritzing}

# Write Light Code

## Create Device File and Folder

We'll want to setup the directory where our driver will be located. Create a `/devices` directory, and within it create another folder called `led`. This folder will contain one file - `index.js`.

Use the PC's command line and run the following terminal commands to create the files and folder that you need:

```bash
mkdir devices/led
```

```bash
touch devices/led/index.js
```

You should end up with a file structure that looks like so:

<pre><code class="bash-noln">
└── zetta-security-system
    ├── apps
    │   └── app.js
    ├── devices
    │   └── led
    │       └── index.js
    ├── package.json
    └── server.js
</code></pre>

## Write the LED Driver Code

1. Install `zetta-device` module, run:

```bash
npm install zetta-device --save
```

1. Install `mraa-js` module used to talk to the Edison's pins, run:

```bash
npm install mraa-js --save
```

1. Ensure `index.js` looks like the code below.

```javascript
var Device = require('zetta-device');
var util = require('util');
var mraa = require('mraa-js');

var LED = module.exports = function(pin) {
  Device.call(this);
  this.pin = pin;
  this._led = new mraa.Gpio(this.pin);
};
util.inherits(LED, Device);

LED.prototype.init = function(config) {
  config
    .type('led')
    .state('off')
    .name('led ' + this.pin)
    .when('off', { allow: ['turn-on'] })
    .when('on', { allow: ['turn-off'] })
    .map('turn-on', this.turnOn)
    .map('turn-off', this.turnOff);

  this._led.dir(mraa.DIR_OUT);
};

LED.prototype.turnOn = function(cb) {
  this._led.write(1);
  this.state = 'on';
  cb();
};

LED.prototype.turnOff = function(cb) {
  this._led.write(0);
  this.state = 'off';
  cb();
};

```

## Run The Zetta Server

1. Ensure `server.js` looks like the code below.

```javascript
var zetta = require('zetta');
var Buzzer = require('zetta-buzzer-edison-driver');
var Microphone = require('zetta-microphone-edison-driver');
var Bean = require('zetta-bean-driver');
var LED = require('./devices/led');

var app = require('./apps/app');

zetta()
  .use(Buzzer, 3)
  .use(Microphone, 0)
  .use(Bean, 'Bean1')
  .use(LED, 13)
  .use(app)
  .listen(1337, function(){
    console.log('Zetta is running at http://127.0.0.1:1337');
  });

```


## Adding to Our App

1. Ensure `app.js` looks like the code below.

```javascript
module.exports = function(server) {
  var buzzerQuery = server.where({ type: 'buzzer' });
  var microphoneQuery = server.where({ type: 'microphone' });
  var ledQuery = server.where({ type: 'led' });

  server.observe([buzzerQuery, microphoneQuery, ledQuery], function(buzzer, microphone, led){
    microphone.streams.volume.on('data', function(msg){
      if (msg.data > 600) {
        buzzer.call('turn-on-pulse', function(){});
        led.call('turn-on', function(){});

        setTimeout(function() {
          buzzer.call('turn-off', function(){});
        }, 3000);
      }
    });
  });
}

```

## Test Interaction

1. Deploy the new code using the `edison-cli`

   ```bash
   edison-cli -H {ip address} deploy
   ```

1. Make a noise near or gently tap on the microphone.

1. Ensure that the alarm sounds for approximately 3 seconds, and the LED turns on.

1. Open the Zetta Browser to observe state changes:

   [http://browser.zettajs.io/#/overview?url=http:%2F%2hello-zetta.herokuapp.com](http://browser.zettajs.io/#/overview?url=http:%2F%2Fhello-zetta.herokuapp.com)