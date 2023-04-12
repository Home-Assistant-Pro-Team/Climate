# Homeassistant climate

This project can be used in three different modes. 
- Remote control base card
  > Allows you to simulate the remote control for a climate entity created with smart ir (valid for all languages)
- Advanced card with pkg for managing automations and statistics
  > Complete package to manage the integrated air conditioner with smart ir automatically with a more complete card than the previous one (text in Italian)
- Blueprint for automation management
  > Easy configuration blueprint suitable for all climate entities for automatic air conditioner management (English and Italian translations)
 ## Index
- [Card base for remote control](#card-base-for-remote-control)
  - [Requirements](#basic-card-requirements)
  - [Functions card](#functions-card)
  - [Loading](#basic-card-loading)
- [Advanced card with pkg for automation and statistics management](#advanced-card-with-pkg-for-automation-and-statistics-management)
  - [Requirements ](#requirements-pkg)
  - [Functions pkg](#functions-pkg)
  - [Functions card ](#functions-card-pkg)
  - [Loading](#loading-card-e-pkg)
- [Blueprint per gestione automazione](#blueperint-per-gestione-automazione)
  - [Requirements ](#requirements-blueprint)
  - [Functions blueprint](#functions-blueprint)

## Card base for remote control:
 <img src="https://user-images.githubusercontent.com/62516592/230682774-197c62dc-205e-47e4-8195-d32e354635cc.jpg" width="200">
 
### Basic card requirements:
- [custom button card](https://github.com/custom-cards/button-card)
- [integrazione smart ir](https://github.com/smartHomeHub/SmartIR)
### Functions card:
This card allows to simulate remote control for climate entities and is not bound to use other files.
The card is made with svg image and custom button-card

  - **Display standard:** on the display you can see the status of the air conditioner (off or on), temperature, hvca mode, set ventilation speed, and indoor temperature and humidity.
 <img src="https://user-images.githubusercontent.com/62516592/230679944-f0c45d7c-95cd-42da-9c99-498a5de8c6fe.jpg" width="200" >
  
  - **1:** With a tap, the air conditioner turns on or off
  - **2:** with a tap you can change the ventilation
  - **3:** with a tap you can change the hvca mode (dry, cool, auto...)
  - **4:** with a tap you can increase the set temperature
  - **5:** with a tap you can customize the action. By default it opens more-info of the air conditioner.
  - **6:** with a tap you can customize the action. By default it opens more-info of the conditioner.
  - **7:** with a tap you can decrease the set temperature
 
### Basic card loading:
To run the card simply copy the file inside a new manual card and replace the climate variable with your own entity 

``` 
type: custom:button-card
variables:
  climate: climate.condizionatore_salone
```

# Advanced card with pkg for automation and statistics management
https://user-images.githubusercontent.com/62516592/230683695-5766ce0c-f760-4e4b-99d1-73056ebbfd53.mp4
### Requirements pkg: 
- [custom button card](https://github.com/custom-cards/button-card) con template abilitato
- [integrazione smart ir](https://github.com/smartHomeHub/SmartIR)
- dashboard in yaml mode, well described by [MaxAlbani](https://www.maxalbani.it/2023/04/home-assistant-dashboards-lo-strumento-per-creare-interfacce-grafiche/#modalita)
- window sensor (not essential)
- flood sensor (not essential)
- absorption sensor in w (not indispensable)
### Functions pkg:
This use is certainly the most complex but also the most comprehensive, because it involves operating the air conditioner in automatic mode taking into account several factors:
- **Mode or period of use:** Four modes of operation can be chosen:
  - [Summer thom index:](https://indomus.it/progetti/definire-un-indicatore-di-benessere-estivo-sulla-domotica-home-assistant/) The air conditioner will turn on or off if the detected thom index is greater than the set one
  - Summer Degrees Celsius: The air conditioner will turn on or off if the detected temperature and humidity is higher than the set one
  - Winter: The air conditioner will turn on or off if the detected temperature is lower than the set temperature
  - Humidity: The air conditioner will turn on or off if the detected humidity is greater than the set humidity
- **Detected indoor temperature:** based on the selected mode, a detected temperature/humidity/thom can be set to manage turning the air conditioner on or off in automatic mode
- **Ventilation speed:** it is possible to set the ventilation speed of the air conditioner to be used with automatic ignition
- **Hvca mode:** it is possible to set the hvca mode (dry,cool,auto...) of the air conditioner to be used with automatic ignition
- **Aconditioner temperature:** and you can set the temperature of the air conditioner to be used with automatic ignition
- **Time slot:** you can choose a time slot for auto on or auto off
- **Home presence:** automations will work only if the status of the group or individual person entity is in the home state. If you switch to the not_home state, the air conditioner will be turned off.
```
# Example of a family group
group:
  famiglia:
    name: Famiglia
    entities:
      - person.marco
      - person.serena
```
- **Notifications:** you can decide whether to enable or disable notifications. The pkg is set to receive them on all devices with companion app installed. In case you want to use different devices or media_player you need to change it.
- **Window Status:** a window status check is performed:
    - If the conditioner is on and the window will be opened you will receive a notification to close it, if this does not happen within 30 seconds the conditioner will be turned off.
    - If the air conditioner is off and is turned on manually with the window open you will receive a notification to warn you
    - If auto on is enabled and there are requirements to turn on the air conditioner but the window is open you will receive a notification
- **Outdoor temperature:** It is performed in two modes.
   - Respecting its own time slot: it is possible to set a detected temperature difference between indoor and outdoor that advises to open or close the window if the window control is active it also checks its status (e.g., when it detects the outdoor temperature is 5° higher than the indoor temperature)
   - Tied to the status of the air conditioner:
      - At the time when the air conditioner is supposed to turn on automatically but the outdoor temperature is higher/lower (depending on the set mode) than the target temperature, the air conditioner does not turn on but you will receive a notification to open the window.
      - If the air conditioner is on but the outdoor temperature is greater/minor (based on the set mode) than the target temperature, you will receive a notification to open or close the window and turn off the air conditioner.
- **Water level:** I use aqara flood sensor to check the status of the tank where the air conditioner drains water.
  - if the air conditioner is on and the tank is full you get a notification to empty it
  - if the air conditioner is on and the tank has been full for 5 minutes the air conditioner will turn off with notification
  - if the tank is full and the air conditioner will be turned on, you will receive a notification to empty it
  - if you turn on the air conditioner and the tank is full but it will not be emptied within 5 minutes it will turn off with notification.
- **Use statistics:** Using a device to detect the power input, in my case shelly-em you can see displayed, the on time, cost and consumption of the air conditioner without using the recorder and have the ability to reset the data at any time.

### Functions card pkg:
 <img src="https://user-images.githubusercontent.com/62516592/230679628-4aa84cd4-3cbd-45fe-87e7-e33b6362e0dd.jpg" width="200" >
 
  - **Standard display:** on the display you can see the status of the air conditioner (off or on), temperature, hvca mode, set ventilation speed, and indoor temperature and humidity, and if active, automatic on/off.
 <img src="https://user-images.githubusercontent.com/62516592/230679944-f0c45d7c-95cd-42da-9c99-498a5de8c6fe.jpg" width="200">
 
  - **1:** with a tap the air conditioner is turned on or off
  - **2:** with a tap you can change the ventilation
  - **3:** with a tap you can change the hvca mode (dry, cool, auto...)
  - **4:** with a tap you can increase the set temperature
  - **5:** with a tap you can view the 4 pages of settings on the display, with a hold tap you can force exit from the menu
  - **6:** with a tap it is possible to show statistics on the display
  - **7:** with a tap you can decrease the set temperature
 <img src="https://user-images.githubusercontent.com/62516592/230681033-1e8d73eb-9df6-4b3a-8e85-5f1b748f2185.jpg" width="200">
 
  - **Statistics display**: this screen is informational only and you cannot interact
  
<img src="https://user-images.githubusercontent.com/62516592/230681298-782bfe27-80ac-401c-a02b-647845cdeadc.jpg" width="200" > <img src="https://user-images.githubusercontent.com/62516592/230681321-41e9f25a-0914-4d77-8749-6c3d2827ca2a.jpg" width="200" >
 <img src="https://user-images.githubusercontent.com/62516592/230681339-1a0ca9c5-5e31-4327-8e60-494acac2bfe9.jpg" width="200" >
 <img src="https://user-images.githubusercontent.com/62516592/230681353-a568a529-7292-49a4-bc04-7b0ac99d903e.jpg" width="200" >
 
  - **Settings display**: each page consists of 5 sections, many of them two rows, to change settings simply tap to change the values found on the first row while a hold tap to select the second row where present
### Loading card and pkg
- **Loading pkg:**
  - Load the contents of the packages folder you just downloaded into the packages folder present in your instance
  - Open each individual file and replace the entities present in the anchors
```
# example
homeassistant:
  customize:
    package.node_anchors:
        Entità clima:                               &climate        climate.condizionatore_salone
```
  - Modify highlighted arrays with their own entities
```
# example
{% set climate = 'climate.condizionatore_salone' %}
```
  - If you want to use the pkg for a second conditioner you have to replace EVERYWHERE the word ac_salone with a new one as you like
  - By default notifications are set to be received on all devices with companion app installed, if you wantioni receive different notifications ex.media player you need to customize the files  
```
# service used by default
- service: notify.notify
  data:
    title: title
    message: message
```            
- **Loading card:**
  - To run the card simply copy the file inside your dashboard yaml
  - change the climate variable to your own entity
  - change (if previously replaced) the variable name with the custom one
``` 
type: custom:button-card
variables:
  climate: climate.condizionatore_salone
  name: ac_salone
```
## Blueperint per gestione automazione:
### Requirements Blueprint

In contrast to the above discussion, this project is compatible with all climate entities 
- Configured climate entity
Optional:
- Window sensor
- Flood sensor
### Functions blueprint:
https://github.com/marco-hacs/Blueprint-Automatic-air-conditioner

This design provides automatic use of the air conditioner in both winter and summer, based on a starting and ending temperature. Optionally, it can be enabled:

- Window status control
- Control of the water level in the tank
- Home presence control
- Notifications (English and Italian)
- Decide the time slot for operation

The following settings for operation are:

1) **Select Language**: Choose the language for notifications (default: Italian).
2) **Climate entity**: Choose the climate entity to be used.
3) **Select season**: Choose the season for use (default: Summer).
4) **Set climate temperature**: Select the temperature to be set to the climate.
5) **Hvac mode**: Select Hvac usage mode (heating, cooling, dehumidification, ventilation only)
6) **Fan mode**: Select the fan usage mode (automatic, high, medium, low). In case your climate settings are different from mine, these can be customized from the source file.
7) **Home Presence**: OPTIONAL Select from the list the group created with person entities to:
- Turn on the air conditioner if the conditions set for turning on are met.
- Turn off the air conditioner at the time of switching to the out-of-home state
```
group:
  famiglia:
    entities:
      - person.marco
      - person.serena
```
8) **Water level**. OPTIONAL: select the track\sensor used to indicate full water tank. To function, it must be set with *device\class: humidity*.
9) **Window**: OPTIONAL select the binary sensor used for window contact. It must be set with *device\_class:window* to function.
10) **Starting target temperature**: Set the starting temperature:
- If set to winter, the climate will be activated if the indoor temperature is lower than the set temperature
- If set to summer, the climate will be activated if the indoor temperature is higher than the set temperature
11) **Target temperature shutdown**: set the shutdown temperature:
- If set to winter, the climate will be turned off if the indoor temperature is higher than the set temperature
- If set to summer, the climate will be turned off if the indoor temperature is lower than the set temperature
12) **Temperature stop delay**: Sets a delay expressed in minutes for the air conditioner to turn off once "*Target temperature stop*" is reached
13) **Start time**: Set the start time of automatic operation of the air conditioner. NB: If you want the climate to be automatic h24, set Start time and End time with the time 00:00:00.
14) **Stop time**: set the end time and shutdown time of automatic climate operation. NB: If you want the climate to be automatic h24, set Start time and End time with the time 00:00:00.
15) **Device for push notification**: OPTIONAL select the device on which you want to receive push notification. The official HomeAssistant app must be installed on the device.

This project was created respecting my personal needs and the climate entities used with Broadlink.

I remain open to feedback and any ideas to make this project more usable for everyone.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmarco-hacs%2FAutomatic-air-conditioner%2Fblob%2Fmain%2Fautomatic_air_conditioner.yaml)
https://community.home-assistant.io/t/automatic-air-conditioner/511251
