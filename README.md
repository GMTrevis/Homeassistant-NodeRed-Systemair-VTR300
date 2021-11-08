# Homeassistant-NodeRed-Systemair-VTR300
Sytemair Save VTR300 integration to Homeassistant, modbus R/W is handled in Node Red.
- **Mobile view**

https://user-images.githubusercontent.com/69414078/140788652-341a1d53-b56f-461f-85dd-b349424d7963.mp4


https://user-images.githubusercontent.com/69414078/140788687-d45e4cbd-352a-48d4-a690-b0cef9e9948a.mp4

## Functionality and features in HA:
A decription below of most of the features in this integration (most of it in Norwegian).
For further details about your ventilation unit, read the applicable manual by Systemair.

###	System overview: 
Showing speed, temperatures, moisture, ppm, efficiency (estimated), power (consumption and estimated added power), actual mode and A/B/C alarms.
- **UI setup:** Picture element and custom:text-element(thanks to @ludeeus). See folder "UI_Config" --> "UI_System_overview.txt" for UI config.
- ![VTR300_system_overview](https://user-images.githubusercontent.com/69414078/140724408-cf12fe03-ffba-41cf-8b2b-23cc5718db4d.png)

###	Mode control buttons: 
Showing state of the actual mode and the current setting/actual time remaining for the applicable “timer modes”. All mode buttons have confirm feature when being set.
- **UI setup:** Custom button card (thanks to @RomRider), entity card and custom:home-feed-card (thanks to @gadgetchnnel). See folder "UI_Config" --> "UI_Mode_control_buttons.txt" for UI config.
- ![VTR300_mode_control_buttons](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_mode_control_buttons.png)
- **Auto: “Auto schedule”:**: When active the ventilation speed are running according to the speed given in the schedule properties, ref. pop-up “Auto Sch.”, further displayed in chapter “Pop-up’s”.
- **PPM Auto:** This is an additional control handled in Node Red which sets the speed of the ventilation unit based on moisture (priority), ppm and presence detection, limits are given in the pop-up “Luftkvalitet”(Air quality) further displayed in chapter “Pop-up’s”.  Modes controlled in Node Red are, Manual low, Manual normal, Manual high and Boost.
- **Borte (Away):** Time limits are given in the pop-up “Varighet” (Duration) further displayed in chapter “Pop-up’s”.
- **Ferie (Vacation):** Time limits are given in the pop-up “Varighet” (Duration) further displayed in chapter “Pop-up’s”.
- **Lav (Manual low speed):** Sets the fans in low speed, PPM Auto must be deselected as PPM Auto writes to the manual mode speed modbus register.
- **Normal (Manual Normal speed):** Sets the fans in normal speed, PPM Auto must be deselected as PPM Auto writes to the manual mode speed modbus register.
- **Høy (Manual High speed):** Sets the fans in high speed, PPM Auto must be deselected as PPM Auto writes to the manual mode speed modbus register.
- **Party:** Sets the fans in full speed (100%). PPM Auto will be set to standby if active prior to selecting “Party”. Time limits are given in the pop-up “Varighet” (Duration) further displayed in chapter “Pop-up’s”.
- **Boost:** Sets the fans in full speed (100%). PPM Auto will be set to standby if active prior to selecting “Boost”. Time limits are given in the pop-up “Varighet” (Duration) further displayed in chapter “Pop-up’s”.
- **Ildsted (Fireplace):** Sets the fans in full speed (100%). PPM Auto will be set to standby if active prior to selecting “Ildsted”. Time limits are given in the pop-up “Varighet” (Duration) further displayed in chapter “Pop-up’s”.
- **Stopp (Off):** Switch the ventilation unit to off (fans 0%). PPM Auto will be switched off if active prior to selecting “Stopp”. Mostly used when changing filters, for more comprehensive maintenance remember to cut the main power supply for safety reasons.
- **Komfyravtrekk (cooker etc.):** including other “DI” inputs are overriding all other operator given modes. This as most other features, are handled in the ventilation unit controller.

### Other mode control buttons: 
Showing state if the feature is enabled on or off.Most mode buttons have confirm feature.
- **UI setup:** custom:button-card. See folder "UI_Config" --> "UI_Mode_control_buttons.txt" for UI config.
- ![VTR300_other_mode_control_buttons](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_other_mode_control_buttons.png)
- **Frikjøling/Nattkjøling(Free/Nightcoling):** This feature can help to cool down the house during night. The heatexchanger is of during the period and the fans can be set to a given speed according to the setpoints given in the pop-up. Not a wow effect as normal ventilation units for houses/apartments have low airflow compared to bigger ventilation units, but it can help.
- **Kjølegjenv(Cooling):** If you by any reason manage to cool down the house so that the extract air temp. is x C° cooler than the in inlet temp. then you can use this feature to make the heat exchanger operate as a cooling-exchanger. My house and ventilation unit is not efficient enough to make any use of this feature. 
- **Eco.:** Toggles the economic mode on/off, preventing the electoral heating element to turn on until temperature difference between the supply/extract air and the given offset is reached. This will lower the load on the el. Heater and lower power consumption/cost if that is desired. It is not “possible” to switch the el.heater off, nor do we want it either during the winter and due to condensation ect. The Eco. Offset setpoint is given given in the “Luftkvalitet” (Airquality) pop-up in this case.
- **Utekomp.(Outdoor compensated temperature setpoint):** Toggles the outdoor compensated temperature feature on/off. This feature is controlled in Node Red and automatically sets (calculate) the temperature setpoint according to the given outdoor temperature. This feature can be used for other heaters/coolers as well if you desire temperature setpoint to change with the change of the outdoor temperature.

###	Neste filterskift (Next filter replacement): 
Converts the seconds from the modbus register (byte swapped) into months, weeks, days, hours and minutes. Each time filter replacement is confirmed from the ventilation unit local panel, NodeRed store the date and time displayed in the next filter change attributes area.
- **UI setup:** custom:button-card.See folder "UI_Config" --> "UI_Filter.txt" for UI config.
- ![VTR300_filter](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_filter.png)

### Supply temperature setpoint (Climate):
Not really a climate controlled in HA or Node Red as the supply temperature control are handled in the ventilation unit. This is a generic climate configured in HA. “Operasjon” (Operation) only visualize if the heat exchanger is on or off. The temperature setpoint is changed as other climates by clicking the up/down arrows. If the outdoor temperature compensation is active the climate setpoint will be written to from Node Red, displaying the actual setpoint, given by the compensation. 
- **UI setup:** custom:simple-thermostat (thanks to @nervetattoo).See folder "UI_Config" --> "UI_Climate.txt" for UI config.
- ![VTR300_climate](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_climate.png)
- **Kalkulert.(Calculated):** The calculated supply (or extract depending on the given regulation type) temperature setpoint, calculated by the outdoor temperature compensation curve.
- **Aktiv utetemp.(Active outdoor temp. sensor):** This is the outdoor temperature used for the outdoor temperature compensation curve, ref. “Pop-up’s” chapter. The inlet for my ventilation unit is located near the main door on the house, causing dips on the outdoor temperature if the door is open a certain time during the winter. The get rid of the temp. dips, I have another outdoor temp. sensor as a primary sensor (netatmo), if the netamo sensor fails does the VTR300 inlet air temp. sensor take over readings as a secondary device. This feature is controlled in Node Red and automatically sets the temperature setpoint according to the outdoor temperature if the “Utekomp”(Outdoor compensation) is active.
- **Auto valg(Auto selected):** Shows which outdoor temp. sensor that is active, this is only being used for the outdoor temperature compensation feature.
- **Auto valg(Auto selected):** Shows which ppm sensor that is active, this is only being used when the PPM Auto feature is active.
- **Ønsket reg.(Desired regulation method):** Click the entity and you can switch between supply temperature regulation or extract air temperature regulation.
- **Type:** Only displays if the ventilation unit is in summer or winter mode (not selectable).

## Pop-up’s:
![VTR300_pop_ups](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_pop_ups.png)
### Frikjøling (Freecooling):
Configurable according to the setpoints given in it’s pop’up, speed settings are Normal, High and Max.
- ![VTR300_free_night_cooling_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_free_night_cooling_popup.png)

### Kjølegjenvinning (Cooling recovery):
Configurable according to the setpoints given in it’s pop’up. Unless your house is extremely well isolated or you manage to cool down your house by using multiple air-conditions during hot days, you will probably not make any use of this feature.
- ![VTR300_cooling_recovery_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_cooling_recovery_popup.png)

### Overstyr. (Manual override):
This feature will give you the opportunity to manually control the extract and supply air fans individually 0-100%. Can be useful during house maintenance causing a lot of dust around the airinlet area. In my case I stopped the supply air fan when cutting the plates preventing a lot of fine dust being trapped in the airfilter. When using this feature you will get an active C-Alarm, this alarm is triggered when manual override are active, the alarm will be cleared when putting the fans back to auto. Configurable according to the setpoints given in it’s pop’up.
- ![VTR300_manual_override_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_manual_override_popup.png)

### Viftekomp. (Utekompensert viftehastighet/Outdoor compensated fan speed):
This feature will change the fan speed with the change of the outdoor temperature, the temperature change is linear between each given setpoints and will change with the outdoor temperature. Can be useful when its’ very cold outside. Configurable according to the setpoints given in it’s pop’up. This feature is always active.
- ![VTR300_outdoor_fan_compensation_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_outdoor_fan_compensation_popup.png)

### Utekomp. (Outdoor compensated temperature setpoint):
This feature is controlled in Node Red and automatically sets (calculate) the temperature setpoint according to the outdoor temperature. When active, the calculated setpoint write the temperature setpoint to the ventilation unit. The calculated temperature setpoint is linear between each given setpoints and will change with the outdoor temperature. Can be useful when its’ very cold and/or warm outside. Configurable according to the setpoints given in it’s pop’up. This feature can be used for other heaters/coolers as well if you desire the temperature setpoint to change with the change of the outdoor temperature.
- ![VTR300_outdoor_temp_compensation_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_outdoor_temp_compensation_popup.png)
- **Auto aktiv sensor:** Indicate which outdoor temp. sensor that is active.
- **Tilluftstemp:** The supply air temp.
- **Kalkulert:** Calculated temperature setpoint given by the compensation curve.
- **Eco. Offset:** Economic offset temperature setpoint.
- **Utetemp.:** Outdoor temperature given by the active temp sensor.

### Luftkvalitet (Air quality) 
This is where the setpoints is given for PPM Auto mode and economic feature, the PPM Auto mode is controlled in Node Red which sets the speed of the ventilation unit based on moisture (priority 1 (Normal and High speed)), ppm (priority 2(Low, Normal, High and Boost speed)) and presence detection (Lo speed) if highest ppm value is <  PPM Normal Limit as criteria.
- **What?:** Some might wonder why Boost aren’t the highest speed if the actual %RH(RF) > %RH(RF) hight setpoint (Fukt Høy grense). In my house the %RH is quickly ventilated after 1, 2 and 3 showers. I didn’t see the reason to enable max speed due to the already efficient ventilation of %RH in Normal and High speed.
- ![VTR300_air_quality_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_air_quality_popup.png)
- **Forespurt hastighet (requested speed):** This only displays the requested speed in PPM Auto mode.
- **El.kolbe nåverdi (El.heater actual value):** This only displays the 0-100% command to the el.heater.
- **Eco.offset el.kolbe (Eco.offset el.heater):** Supply/extract air temperature offset for activating the el.heater.
- **Fukt nåverdi (Moisture actual value):** Extract air moisture.
- **Fukt Normal grense (Moisture Normal Limit):** If %RH > limit, Normal speed I set.
- **Fukt Høy grense (Moisture Hi Limit):** If %RH > limit, High speed I set.
- **Aktiv sensor (Active sensor):** I have two ppm sensors (Netatmo and SCD30 using ESPHome) where the sensor with the highest ppm value are auto selected in Node Red and used as input to fan speed control for PPM Auto. This field display which sensor that is active (S1 or S2) and the actual ppm of the auto selected ppm sensor. If the moisture is within normal values and both ppm sensors fail when in PPM Auto, the ventilation unit will/shall “recover” to Auto Schedule.
- **Stue (S1) (Livingroom (S1)):** PPM sensor 1 actual value.
- **Soverom (S2) (Bedroom (SS)):** PPM sensor 2 actual value.
- **PPM Lav grense (PPM Low Limit):** If ppm < limit, Low speed I set. If ppm > limit, Low Normal speed is set.
- **PPM Normal grense (PPM Normal Limit):** If ppm < limit, Normal speed I set. If ppm > limit, High speed is set.
- **PPM Høy grense (PPM High Limit):** If ppm < limit, High speed I set. If ppm > limit, Boost speed is set. Boost is only running its given time limit, given in the pop-up “Varighet”. Boost will be re-triggered if the actual ppm value > PPM High Limit.

### Auto Schedule 
This is where the setpoints is given for the Auto mode (Auto schedule), where Sch.1 (Schedule 1) and Sch.2 (Schedule 2) is two schedules configurations that can be configured for each day.
- ![VTR300_auto_schedule_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_auto_schedule_popup.png)
- **Aktuell modus (Actual mode):** This only displays the actual mode.
- **Hast./Speed (Sch1/Sch.2):** Drop down menu to choose the requested speed for Sch.1 and Sch.2. (Low, Normal and High speed). Each schedule is active if within given time range and if set to “Active”.
- **Offset (Sch1/Sch.2):** Supply/extract air temperature offset if you want the supply/extract air to be reduced when schedules are active or inactive. 
- **Sch1. Start:** Start time (setpoint) for when you want Sch.1 to activate. 
- **Sch1. Stopp:** Stop time (setpoint) for when you want Sch.1 to become inactive.
- **Sch1.:** Must be given as “Aktiv”(Active) if the ventilation unit to run according to the given time schedule channel. If “Av”(Off), the given time schedule channel will be ingored and the ventilation unit will run according to the previous given time schedule channel.
The same applies to all other days for Sch.1 and Sch.2.
**Note!** The system time and date for the ventilation unit must be correct for the schedule feature to work according to your time zone.

### Varighet (Duration)
- ![VTR300_mode_duration_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_mode_duration_popup.png)
- **Boost:** Requested runtime in minutes.
- **Party:** Requested runtime in hours.
- **Ildsted:** Requested runtime in minutes.
- **Borte:** Requested runtime in hours.
- **Ferie:** Requested runtime in days.

### System
Display misc. system info.
- ![VTR300_system_popup](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_system_popup.png)
- **Intern-ur (Internal clock):** Displays the ventilation units internal clock.
- **Intern-Kalender (Internal calendar):** Displays the ventilation units internal date.
- **Synkroniser intern-ur (Sync. Internal clock with system clock:** After time the internal clock on the ventilation unit will drift off, this switch will sync. the internal clock with the system time. The switch toggles itself off after 10s and will be only be available if hours is off for more than 10 min or in minutes are off for more than 5 min.
- **Modbus Queue state:** Displays the level (Lo,LoLo,Hi & HiHi) of the queue state given in the “Modbus Queue Info” Node in Node Red if an error on the Modbus client (slave) is detected in the “Catch all” node.
- **Client:** Given modbus client name.
- **Adr.:** Given modbus address.
- **Queued items at trigger:** Displays the quantity of queued items, provided by the “Modbus Queue Info” Node in Node Red if an error on the Modbus client (slave) is detected in the “Catch all” node.
- **Hovedbryter (Main switch):** The switch for the main power supply. Im using the Kasa smart wifi plug HS110 with power and energy monitoring
- **Strøm (Current):** Actual current consumption in ampere.
- **Spenning (Voltage):** Actual voltage consumption in volts.
- **Forbruk (Consumption):** Actual power consumption in watts provided by HS110.
- **Tilført (Produced):** Estimated power production in watts calculated in Node Red.
- **Faktisk tilført (Actually produced):** Estimated actual power production in watts calculated in Node Red (Produced – consumption = Actually produced).

## Tellere (Counters)
### Forbruk (Consumtion)
**Not finished!** All calculation/estimations are finished, just need to finish the pop-up.
- Will include daily, previous day, weekly, previous week, monthly, previous month, yearly and previous year power consumption in kWh and Kr. 
### Tilført (Produced)
**Not finished!** All calculation/estimations are finished, just need to finish the pop-up.
- Will include daily, previous day, weekly, previous week, monthly, previous month, yearly and previous year estimated produced energy in kWh and Kr. 
###	Fakt. tilført (Actually produced)
**Not finished!** All calculation/estimations are finished, just need to finish the pop-up.
- Will include daily, previous day, weekly, previous week, monthly, previous month, yearly and previous year estimated actually produced energy in kWh and Kr. (Produced – consumption = Actually produced).
###	Total
**Not finished!** All calculation/estimations are finished, just need to finish the pop-up.
- Will include total energy consumption, estimated produced energy and estimated actually produced energy in kWh and Kr.

## Graphs:
###	Effekt <- 48t (Power <- 48h)
A history graph displaying power consumption, estimated power produced and estimated power actually produced the latest 48 hours.
- ![VTR300_power_graph](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_power_graph.png)
###	Energi <- 7d (Energy <- 7d)
A history graph displaying energy consumption, estimated energy produced and estimated energy actually produced for each day, the last 7 days.
- ![VTR300_energy_graph](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_energy_graph.png)
###	Energy cost <- 7d (Energy <- 7d)
A history graph displaying the costs of the energy consumption, estimated energy produced and estimated energy actually produced for each day the last 7 days.
- ![VTR300_energy_cost_graph](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_energy_cost_graph.png)
I’m using the custom component “nordpool” provided by @Hellolol and add the grid rent and chargerate to the Nordpool price to get the correct price for my region and consumption cost.

## Notifications: 
### Persistant notifications:
- Modbus Data sist lest + last timestamp (reads the internal clock register as an attempt to try to notify if modbus communication is down is down, this will be as an alternative to check info in the “system” pop-up).
- Moist override Hi and Manual override enabled.
- ppm sensor failure.
- Outdoor temp. sensor failure.
- Boost enabled + actual ppm.
- Recovery to Auto schedule due to ppm sensor failure.
- A-Alarm
- B-Alarm
- C-Alarm
- Filter change, pre-alarm
- Filter change
- Supply temp. Lo
- Internal clock hour is off for longer than 10min.
- Internal clock min. is off for longer than 5min.
- Triggered by catching all modbus client error and checking that the queue build-up is != low level. This is probably not working as intended.
### Mobile notifications:
- A-Alarm
- B-Alarm
- C-Alarm

## Node Red “map”:
A “map” of the flow in Node Red where modbus read/write is handled, numbered to give an idea where you need to modify the flow in Node Red. I might miss some points below, but it will give you a rough idea.
- ![VTR300_node_red_map](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/VTR300_node_red_map.png)

### Outdoor temperature compensation:
- If you don’t want this feature in your flow, you need to modify the following flows shown in the picture below: 1 and 15.
### Alarms:
- You will need to change the “Call service nodes” to get the alarms to your phone in section 11(Notify mobile app) and 20 (Modbus client name) in the “map” below.
### PPM Auto Mode:
- If you don’t want this feature in your flow, you need to modify the following flows shown in the picture below: 3, 4, 5 and 6.
- If you want this feature you will need to change the “Presence entity”, “ppm entity” given in “ppm Fan speed” template node
### Estimated power and energy:
- If you don’t want this feature in your flow or don’t measure the actual power consumption of your ventilation unit, you can delete the following flows shown in the picture below: 18.
- If you want this feature in your flow but don’t measure the actual power consumption of your ventilation unit for now, you can disable the following flows shown in the picture below: 18.
- If you want this feature you will need to change the “Poll state node” providing the actual power consumption of your ventilation unit.
### Auto select outdoor temperature sensor:
- You need to delete or change parts of the flow in section 15.

### Flow context:
-	If not already enabled/added, you will need to enable/add flow context in your Node Red “settings.js” file configuration file as the modbus read/write handling uses flow context. This will detect (update Node Red/HA setpoints) if settings/setpoints are changed from the ventilation unit local panel. In my case the following are added to my “settings.js” fil:
```
// Lagre vairable permanent i "memory" eller "file" - Default to memory, require a name for storing in a file
  contextStorage: {
      default    : { module: "memory" },
	  storeInFile: { module: "localfilesystem"}
  }, 
```  

## Modbus  setup options
-	You can choose Modbus TCP (Ethernet interface) and Modbus RTU (serial interface), there are also some modbus gateways providing modbus data wirelessly over Wi-Fi, I’m not familiar with this but it might be useful to check out for those where hardwiring not is an option. The modbus registeres (addresses) are the same if using Modbus TCP or modbus RTU.
-	I’m using the IAM gateway and configured “connection mode” set to “Modbus gateway”, by following its manual (Gateway setup in the web interface). If I didn’t have the IAM gateway I would use the Modbus RTU. In my case was modbus tcp the easiest and cleanest method, you must choose the modbus setup that suits your installation. If you setup the IAM gateway using modbus, then you won’t be able to use the systemair app, you must choose either modbus or the app/cloud.
-	Adresses: Modbus registers are offset by “-1”, see the Note in chapter Installation.


## Installation:

###	Before you begin: 
- You should have your modbus TCP or Modbus RTU setup and configured, you should also make sure that your ventilation unit uses the same modbus registers as my setup. There are various modbus tools available to check that you can read data from the modbus registers over modbus TCP and modbus RTU, this is not a must but it’s always nice to know if the modbus connection are ok or not (easier to start trouble shooting). If you are not certain that the modbus registers on your ventilation unit is the same as this setup, then you should do spot checks reading modbus registers and verify that the registers are the very same as this setup, ref. the pdf modbus adr. list for my VTR300 in the folder "Views". If the adr. in your ventilation unit does not match, then you should not use this integration, because a lot of the modbus handling in Node Red has to be re-configured to match the modbus registers for your ventilation system. I have been using the “modscan” modbus tool for reading modbus data (a lot of trial and error… and success in the end!).
- NOTE! The modbus addresses in the systemair documentation has an offset of “-1”. That means if you are trying to read the supply temperature by reading modbus register 12103 (from the manual), then you will need to read modbus register 12102 (12103-1=12102), this applies to all modbus registers. 
- Node Red : This addon need to be installed as all modbus read and write are done using Node Red. You will also need to install some additional nodes like “node-red-contrib-cron-plus” and a few others. You will see which nodes that are missing when you have imported the VTR300 Node Red flow, you can then install the missing nodes through Manage palette  Install.

### HA and Node Red manual installation: 
This is not as smooth as I would like it to be as it is a manual installation, but it’s not hard either.Copy the following yaml configurations to tour configuration:
- Configuration (Generally include and climate, need to be adapted to your configuration if you want I your way).
- input_boolean_vtr300
- input_datetime_vtr300
- input_number_vtr300
- input_select_vtr300
- scripts_vtr300
- scripts_poup_ vtr300
- sensors_time_date_misc (only a few of them are being used).
- switches_vtr300
- customize_vtr300 (If you want icon state changes, the ppm entities must be adapted to your setup)
- customize_global (If you want icon state changes)
#### UI .png files
Copy the .png files to your dedicated "picture" folder, I have mine in the following folders www -> images -> VTR300
- VTR300_flytskjema_A
- VTR300_utekomp_03e
- VTR300_utekomp_tilluft_B
	
#### Import the following Node Red flows:
- vtr300.json
- glob_time.json
#### Modbus Client: 
Click the “Configuration nodes” in the upper right corner in the flow page and configure the modbus client node according to your setup, you need to change the “Type” (if not using TCP) and “Host” .
- **IP address:**  If you use a different IP as you most likely are, then you also need to change the IP in all the function nodes called “Modbus read” and “Modbus write”.
If you use a different slave adr. as you maybe are, then you also need to change the “addresses” in all the function nodes called “Modbus read” and “Modbus write”.
- **Install missing nodes:** Then install the nodes that are missing through Manage palette  Install.
#### Before deploy: 
- 1. I would disable all modbus write nodes, this applies to all Modbus –Flex-Write-Nodes marked “(w)” on the applicable nodes, this is done by double clicking the node and click the “Enabled” on the lower left corner, it will then change to “Disabled”. It is a few nodes and this should prevent writing “default” values/setpoints from HA to the ventilation unit. The setpoints in HA should now be updated by the ventilation unit. 
- 2. You will need to change some nodes, ref. chapter “Node Red “Map””.

#### Ready for deploy: 
- When all the Modbus –Flex-Write-Nodes marked “(w)” are disabled, then you can deploy if the is no error in HA or Node Red.
#### Enable modbus write nodes:
- When all the setpoint for the ventilation unit is updated in HA (by the ventilation unit), then you can Enable all the Modbus –Flex-Write-Nodes marked “(w)”, then you can deploy. 
- Note! You should change the “Deploy” option to “Modified Nodes”, to only deploy the nodes that have been changed. I have seen that deploying all nodes (Full) have stopped the modbus communication, changing to “Modified Nodes” have eliminated this issue for me. If the modbus communication should stop, a restart of Node Red addon re-established the modbus communication for me when using deployning everything in the workspace.

You should now have quite a lot of new entities in the Node Red integration.

## Air inlet – Insect net cover:
- Due to surrounding lights near the inlet, my ventilation system get some insects in the filters. Probably not a big deal, but to try to get less insects and extend the filter change periods I made a simple model in Fusion 360 and printed a cover with magnets (glued)  and a hand cut insect net or similar. This filter is vacuum cleaned twice a month or so and minimize insects in the airfilter. This model was made first time using Fusion 360 and 3D printer, the model have potential of being improved, but it works very well for its intension. In my case I open the door, take a step out and start the vacuum cleaner. 
- ![Inlet_cover_outside](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/Inlet_cover_outside.png)
- ![Inlet_cover_inside](https://github.com/GMTrevis/Homeassistant-NodeRed-Systemair-VTR300/blob/main/Views/Inlet_cover_inside.png)

## Remarks:
This Systemair Save VTR300 integration has now been running for about a year, started out as a clean HA modbus integration. In order to get “all” the available features integrated as well as additional control (PPM Auto, Outdoor temp. compensation ++), all the modbus handling was re-located to Node Red, with trail and error along the way. Not everything is fully tested and cannot be expected to be 100% bulletproof but its working as it should in my case. The most time consuming part of the integration was to get control of the modbus registers. This is the first time creating a repo. In github, the approach could probably have been done better.




