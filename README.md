# Automotive-InputOutput-Circuits
Circuit diagrams used with the Arduino sketches

The Power Supply schematic includes a 5vdc Sensor regulator. This reduces the load (and heat generated) on
the Arduino on-board linear regulator, and also allows more effective input protection
by diverting high voltages to the 5vdc Sensor reg via schotttky diodes, where the over-voltage can be absorbed
by a 5.6v 5watt zener, filter capacitors and schottky diode.
The LM2940CT-5 regulator is suitable for automotive applications and handles reverse voltages and load dumps,
even though those events are unlikely to happen to that regulator considering where it is on the circuit.

The Arduinos are powered via their input barrels (soldered wires) from a "Arduino" 2.5A adjustable switching
supply set to 10vdc. This means the protection diode and input caps are retained for isolation of
each Arduino, even though it also means the onboard linear regulator (heat generator) is also used.
Otherwise the Vin pin could be used but reverse voltage protection diode wouldnt be inline. 
Other options would require physical modification of the board.

https://www.jaycar.com.au/arduino-compatible-dc-voltage-regulator-module/p/XC4514

General supply voltage protection includes:
- PTC resettable fuse
- TVS diode
- High voltage polyester caps
- Reverse voltage protection diode
- Tranzorb transient protection zener diode
- Low ESR electrolytic caps
- Some capacitors are specified as Tantalum for the their Low ESR performance

The Tachometer Arduino outputs high RPM serial data to the Fuel/Temp/Volts Arduino, where a NeoPixel strip is used
as a shift light. The last LED on the strip is used to indicate various statuses.

Arduino analog and digital inputs are protected by schottky diodes or transorb/zeners as appropriate. ~~Where inverters are used
they have inbuilt protection diodes and only an external resistor is used. Most of the digital inputs
are debounced in hardware via the schmitt trigger inverters and basic noise filtering.~~
Design changed to use opto couplers for digital inputs for superior protection of the Arduinos and simplicity. The optos also provide some natural schmitt trigger/hysterisis effect to aid in switch debouncing.
The opto coupler circuits are arranged to provide active high inputs to the Arduinos.

An Op Amp is used to get a more usable voltage range from the fuel level sensor, since the standard Datsun fuel sender
only has an 8ohm to 80ohm range.

There is facilty to use a DS18B20 one-wire temperature sensor, or there's calibration values in the
sketches for Nissan SR20 or standard NTC resistor temperature sensors.

**Light inputs:**
- Parkers and Low beam are active high
- High beam is active low, inverted by a ~~schmitt trigger~~ opto coupler placement to active high

**Sounds**
Each main Arduino (Fuel/Temp / Tacho / Speedo) outputs a warning signal and/or Oil Pressure warning signal. These are actioned
by a Leonardo Tiny and small amplifier. These releives the main Arduinos from making sounds and any related blocking delays.

**Pins from sketches:**
Not all pins are used on every Arduino.

Pin definitions for digital inputs
- Oil_Press_Pin = 0;              // Oil pressure digital input pin
- Parker_Light_Pin = 1;           // Parker lights digital input pin
- Low_Beam_Pin = 2;  // Low beam digital input pin
- High_Beam_Pin = 3;              // High beam digital input pin
- Pbrake_Input_Pin = 4;           // Park brake input pin
- VSS_Input_Pin = 5;              // Speed frequency input pin
- RPM_Input_Pin = 6;  // RPM frequency input pin
- ~~RPM_PWM_In_Pin = 6;             // Input PWM signal representing RPM~~
- Button_Pin = 7;  // Button momentary input

Pin definitions for analog inputs
- Temp_Pin = A0;                  // Temperature analog input pin - not used with OneWire sensor
- Fuel_Pin = A1;                  // Fuel level analog input pin
- Batt_Volt_Pin = A2;             // Voltage analog input pin
- Alternator_Pin = A3;            // Alternator indicator analog input pin

Pin definitions for outputs
- ~~RPM_PWM_Out_Pin = 10;            // Output of RPM as a PWM signal for shift light~~
- LED_Pin = 10;                   // NeoPixel LED pin
- Warning_Pin = 11;               // Link to external Leonardo for general warning sounds
- OP_Warning_Pin = 12;            // Link to external Leonardo for oil pressure warning sound
- Relay_Pin = 13;                 // Relay for fan control

- ONE_WIRE_BUS 14                // Specify the OneWire bus pin

DS18B20 one-wire pullup resistor guide
```
Length        5.0 Volt  3.3 Volt
10cm (4")      10K0      6K8
20cm (8")      8K2       4K7
50cm (20")      4K7      3K3
100cm (3'4")    3K3      2K2
200cm (6'8")    2K2      1K0
```

Serial2 pins used on the Mega2560s for RPM shiftlight comms
pins 17(RX), 16(TX)