# ZMAi-90_localtuya
Reading all data from ZMAi-90 with localtuya and py

For home-assistant
Quick get all data from ZMAI-90 via local tuya and python


**** Config in HA

localtuya:
  - host: 192.168.187.23
    device_id: xxxxxxxxxxxxxxxxxxx
    local_key: xxxxxxxxxxxxxxxxxxxx
    friendly_name: solar_guesthouse_west
    protocol_version: "3.3"
    entities:
      - platform: sensor
        friendly_name: solar_guesthouse_west_energy
        id: 1
        device_class: energy
        unit_of_measurement: kWh
        scaling: 0.01
      - platform: sensor
        friendly_name: solar_guesthouse_west_phase_a
        id: 6 
        
        
**** Python script

@time_trigger("period(now, 10sec)")

def decode():
    import base64
    import binascii
    txt = str(sensor.solar_guesthouse_west_phase_a)
    txt = base64.b64decode(txt)
  
    
    #log.info('Get Voltage,,,,,,,,,,,,,,,,,,,,')
    int_val = int.from_bytes(txt[0:2], "big", signed=False)
    log.info("Volt")
    log.info( int_val)

    int_val = int.from_bytes(txt[3:5], "big", signed=False)
    log.info("Amp")
    log.info( int_val)

    int_val = int.from_bytes(txt[6:8], "big", signed=False)
    input_number.solar_guesthouse_west_watt.set_value(int_val)


**** Create 3 help tag (Im just using 1 for Watt)

that's it




