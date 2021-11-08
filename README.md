# Homeassistant-NodeRed-Systemair-VTR300
Sytemair VTR300++ integration to Homeassistant, modbus R/W is handled in Node Red.

## Functionality and features in HA:
A decription below of most of the features in this integration (most of it in Norwegian).
For further details about your ventilation unit, read the applicable manual by Systemair.

###	System overview: 
Showing speed, temperatures, moisture, ppm, efficiency (estimated), power (consumption and estimated added power), actual mode and A/B/C alarms.
![VTR300_system_overview](https://user-images.githubusercontent.com/69414078/140724408-cf12fe03-ffba-41cf-8b2b-23cc5718db4d.png)
UI setup: Picture element and custom:text-element(thanks to @ludeeus).
```
elements:
  - entity: sensor.tp_link_vtr300_watts
    style:
      color: ''
      font-size: 120%
      left: 47.5%
      top: 22%
    title: Effektforbruk
    type: state-label
  - entity: sensor.vtr300_faktisk_tilfort_kwh_daily_kr
    style:
      color: white
      font-size: 100%
      left: 48.7%
      top: 70.5%
    title: Kr. tilført i dag (Estimert)
    type: state-label
  - entity: sensor.vtr300_forbruk_kwh_daily_kr
    style:
      color: white
      font-size: 100%
      left: 49.6%
      top: 74%
    title: Kr. forbruk i dag (Estimert)
    type: state-label
  - entity: sensor.vtr300_faktisk_tilfort_watt
    style:
      color: white
      font-size: 120%
      left: 47.7%
      top: 18%
    title: Faktisk tilført effekt - Estimert
    type: state-label
  - entity: sensor.vtr300_inntaks_temperatur
    style:
      color: ''
      font-size: 120%
      left: 12.5%
      top: 12%
    title: Inntaks temperatur
    type: state-label
  - entity: sensor.netatmo_netatmo_indoor_outdoor_temperature
    style:
      color: grey
      font-size: 100%
      left: 16.5%
      top: 16%
    title: Netatmo utetemp.
    type: state-label
  - entity: sensor.netatmo_netatmo_indoor_outdoor_humidity
    style:
      color: grey
      font-size: 100%
      left: 4.5%
      top: 16%
    title: Netatmo utefukt.
    type: state-label
  - entity: sensor.vtr300_avkast_temperatur
    style:
      color: ''
      font-size: 120%
      left: 35.5%
      top: 12%
    title: Avkast temperatur
    type: state-label
  - entity: sensor.vtr300_avtrekks_temperatur
    style:
      color: ''
      font-size: 120%
      left: 63.5%
      top: 12%
    title: Avtrekkstemperatur
    type: state-label
  - entity: sensor.vtr300_avtrekksluft_sp
    style:
      color: orange
      font-size: 80%
      left: 60.5%
      top: 8.5%
    title: Ønsket avtrekks temperatur
    type: state-label
  - entity: sensor.ppm_sensor_selected
    style:
      color: lightblue
      font-size: 120%
      left: 70.2%
      top: 21.9%
    title: Luftkvalitet
    type: state-label
  - entity: sensor.netatmo_netatmo_indoor_co2
    style:
      color: grey
      font-size: 100%
      left: 71.2%
      top: 26%
    title: Stue(S1) ppm.
    type: state-label
  - entity: sensor.scd30_1_co2
    style:
      color: grey
      font-size: 100%
      left: 71.2%
      top: 29%
    title: Soverom (S2) ppm.
    type: state-label
  - entity: sensor.climate_vtr300_tilluft_sp
    style:
      color: orange
      font-size: 80%
      left: 80.5%
      top: 8.5%
    title: Ønsket tillufts temperatur
    type: state-label
  - entity: sensor.vtr300_tillufts_temperatur
    style:
      color: ''
      font-size: 120%
      left: 83.5%
      top: 12%
    title: tilluftstemperatur
    type: state-label
  - entity: sensor.vtr300_overhetingstemperatur_tilluft
    style:
      color: ''
      font-size: 120%
      left: 92%
      top: 29%
    title: Overhetningstemperatur tilluft
    type: state-label
  - entity: sensor.vtr300_fukt_avtrekk_sp
    style:
      color: orange
      font-size: 80%
      left: 69.5%
      top: 37.7%
    title: Settpunkt - Fukt avtrekk
    type: state-label
  - entity: sensor.vtr300_relativ_fukt_avtrekk
    style:
      color: ''
      font-size: 120%
      left: 71%
      top: 41.5%
    title: Fukt avtrekk
    type: state-label
  - entity: sensor.vtr300_avtrekksvifte
    style:
      color: black
      font-size: 120%
      left: 36%
      top: 36.3%
    title: Avtrekksvifte pådrag
    type: state-label
  - entity: input_select.vtr300_fan_extract_man_override
    style:
      left: 26.5%
      top: 35.3%
    title: Avtrekksvifte manuell overstyring
    type: state-icon
  - entity: sensor.vtr300_tilluftsvifte
    style:
      color: black
      font-size: 120%
      left: 78.9%
      top: 75.5%
    title: Tilluftsvifte pådrag
    type: state-label
  - entity: sensor.vtr300_tilluft_rf
    style:
      color: ''
      font-size: 90%
      left: 67%
      top: 75.5%
    title: Tilluftmengde
    type: state-label
  - entity: input_select.vtr300_fan_supply_man_override
    style:
      left: 86.9%
      top: 75.5%
    title: Tilluftsvifte manuell overstyring
    type: state-icon
  - entity: sensor.vtr300_avtrekk_rf
    style:
      color: ''
      font-size: 90%
      left: 46.5%
      top: 36%
    title: Avtrekksmengde
    type: state-label
  - entity: sensor.vtr300_varmegjenvinner
    style:
      color: ''
      font-size: 150%
      left: 49%
      top: 91%
    title: Varmegjenvinner pådrag
    type: state-label
  - type: custom:text-element
    text: Pådrag
    style:
      left: 47.5%
      top: 86.5%
  - entity: sensor.vtr300_gjenvinningsgrad
    style:
      color: ''
      font-size: 150%
      left: 49%
      top: 56%
    title: Gjenvinningsgrad varmegjenvinner
    type: state-label
  - type: custom:text-element
    text: Gjenv.grad
    style:
      left: 47.5%
      top: 50%
  - entity: sensor.vtr300_el_kolbe_padrag
    style:
      color: black
      font-size: 110%
      left: 82.1%
      top: 52%
    title: El. varmer pådrag
    type: state-label
  - entity: sensor.vtr300_el_kolbe_padrag
    icon: ''
    state_color: true
    style:
      left: 80.5%
      top: 57%
    title: El. varmer pådrag
    type: state-icon
  - entity: sensor.vtr300_avtrekksluft_max_sp
    style:
      color: orange
      font-size: 80%
      left: 90.0%
      top: 47%
    title: Makstemp. tilluft
    type: state-label
  - entity: sensor.vtr300_avtrekksluft_min_sp
    style:
      color: orange
      font-size: 80%
      left: 90.0%
      top: 59%
    title: Mintemp. tilluft
    type: state-label
  - entity: input_number.vtr300_eco_offset_temp_sp
    style:
      color: orange
      font-size: 80%
      left: 89.5%
      top: 25%
    title: VTR300 Eco offset (tillegsvarme)
    type: state-label
  - entity: binary_sensor.vtr300_eco_mode_state
    icon: mdi:leaf
    state_color: true
    theme: green
    style:
      left: 90%
      top: 20%
    title: VTR300 Eco modus
    type: state-icon
  - entity: sensor.vtr300_modus_status
    style:
      color: ''
      font-size: 180%
      left: 47%
      top: 96.8%
    title: VTR300 Modus
    type: state-label
  - entity: switch.vtr300_set_system_time
    icon: ''
    state_color: true
    style:
      left: 3.0%
      top: 58%
    title: Intern ur
    type: state-icon
  - entity: input_boolean.vtr300_utekomp_tilluft_reg
    icon: ''
    state_color: true
    style:
      left: 3.3%
      top: 64%
    title: Utekompensert regulering
    type: state-icon
  - entity: sensor.ppm_sensor_selected
    icon: ''
    state_color: true
    style:
      left: 3.0%
      top: 79%
    title: Luftkvalitet
    type: state-icon
  - entity: sensor.vtr300_relativ_fukt_avtrekk
    icon: ''
    state_color: true
    style:
      left: 3.0%
      top: 84%
    title: Fukt avtrekk
    type: state-icon
  - entity: climate.vtr300_tilluft_sp
    icon: ''
    state_color: true
    style:
      left: 3.0%
      top: 90%
    title: Gjenvinner (Av/Varme/kjøler)
    type: state-icon
  - entity: sensor.vtr300_cooling_recovery_states
    icon: ''
    state_color: true
    style:
      left: 9.0%
      top: 90%
    title: Kjølegjenvinning (Av/Standby/kjøler)
    type: state-icon
  - entity: sensor.vtr300_freecooling_state
    icon: ''
    state_color: true
    style:
      left: 15.0%
      top: 90%
    title: Frikjøling (Av/Standby/kjøler)
    type: state-icon
  - entity: sensor.vtr300_a_alm
    icon: ''
    state_color: true
    style:
      left: 3.0%
      top: 96%
    title: A-Alarmer
    type: state-icon
  - entity: sensor.vtr300_b_alm
    icon: ''
    state_color: true
    style:
      left: 9.0%
      top: 96%
    title: B-Alarmer
    type: state-icon
  - entity: sensor.vtr300_c_alm
    icon: ''
    state_color: true
    style:
      left: 15.0%
      top: 96%
    title: C-Alarmer
    type: state-icon
image: local/images/VTR300/VTR300_flytskjema_A.png
type: picture-elements
```
