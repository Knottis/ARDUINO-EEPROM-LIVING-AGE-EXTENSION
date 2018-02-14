# ARDUINO-EEPROM-LIVING-AGE-EXTENSION
EEPROM Living Age Extension for Arduino - ATmega

### Introduction
Instead for using external EPROM IC you can use this library.

EEPROMLivingAgeExtension is a library that allows your project to use the internal EEPROM to save data over the allocated address space, you define. The library is moving the data in this EEPROM address space. When it’s hits the 100,000 cycles limited for the cells it’s start to move the data to a new location in EEPROM. Starts from the beginning and works up to the end of the address space you assign. The life extension for your EEPROM is depended of how large the address space and how much data space you allocate. The data is stored in a Array[] and the Data and Address type in the library is of type Long (4-bits) or Data of type Int(2-bits) if you use the Interer version. You define the size of the data array: DATAWITHINEEPROM[0-1023] .  And the size of DATASIZEWITHINEEPROM = DATAWITHINEEPROM.

Data library: Long 
- EEPROMLivingAgeExtension.zip

Data library: Integer
- EEPROMLivingAgeExtensionInt.zip

### EEPROM Life calculation for the allocated address space

Use this to calculate the optimized address space to your data and needed EEPROM life... 

```
Start address: EEPROMADRESS = 0
End address:   EEPROMADRESS_LAST = 100

DataSpace = 4 + ( DATASIZEWITHINEEPROM * "4 or 2" )     ==>>   DATA-SIZE-WITHIN-EEPROM is value of 0-1023
                                                        ==>>   4 -> Long.  or 2 -> Integer library.

LIFECOUNTDOWN = ( ( EEPROMADRESS_LAST – EEPROMADRESS ) / DataSpace )  *  100.000
```

### Using EEPROMLivingAgeExtension
The library has these three simple functions:

## Data library: Long 
#### EEPROM-LIVING-AGE-WRITE
<sub> EEPROMLivingAgeExtension.EEPROMLIVINGAGEWRITE( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, DATAWITHINEEPROM )
</sub>
#### EEPROM-LIVING-AGE-READ
<sub> EEPROMLivingAgeExtension.EEPROMLIVINGAGEREAD( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, DATAWITHINEEPROM )
</sub>

#### EEPROM-LIVING-AGE-LIFE-COUNTDOWN
<sub> EEPROMLivingAgeExtension.EEPROMLIVINGAGELIFECOUNTDOWN( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, LIFECOUNTDOWN 
</sub>


## Data library: Integer
#### EEPROM-LIVING-AGE-INT-WRITE
<sub> EEPROMLivingAgeExtensionInt.EEPROMLIVINGAGEINTWRITE( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, DATAWITHINEEPROM )
</sub>

#### EEPROM-LIVING-AGE-INT-READ
<sub> EEPROMLivingAgeExtensionInt.EEPROMLIVINGAGEINTREAD( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, DATAWITHINEEPROM )
</sub>

#### EEPROM-LIVING-AGE-INT-LIFE-COUNTDOWN
<sub>EEPROMLivingAgeExtensionInt.EEPROMLIVINGAGEINTLIFECOUNTDOWN( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, LIFECOUNTDOWN )
</sub>


## Demostration  - Long library.
Here is an example to demonstrate how the EEPROMLivingAgeExtension works:


```
EEPROMADRESS = 0;             // Start addresses for the data. 0 -> 1023.
EEPROMADRESS_LAST = 1023;     // End addresses for data.       0 -> 1023.

DATAWITHINEEPROM[10];         // Size of Data Arrays[]...      0 -> 1023. (Datatype Long or Integer => Library)

DATASIZEWITHINEEPROM = 10;    // Number of Data in the Array[]...  0 -> 1023.

LIFECOUNTDOWN = 0;            // Return the remaining number of Life, for the EEPROM cells!.. MAX = 12.800.000
```


![alt text](https://github.com/Knottis/ARDUINO-EEPROM-LIVING-AGE-EXTENSION/blob/master/EEPROMLivingAgeExtension.jpg "EEPROMLivingAgeExtension")



```
include <EEPROMLivingAgeExtension.h>

//=================================================================================================================
int EEPROMADRESS = 0;             // Start addresses for data.     0 -> 1023.
int EEPROMADRESS_LAST = 100;      // End addresses for data.       0 -> 1023. 
int DATASIZEWITHINEEPROM = 10;    // Number of Data inn the Arrays[]...   0 -> 1023++..
long DATAWITHINEEPROM[10] = {0};  // Size of Data Array[]...       0 -> 1024. (LONG="(+-)2.147.483.648").
long LIFECOUNTDOWN = 0;           // Return the remaining number of Life, for the EEPROM cells!..
//=================================================================================================================

int DATA_SIZE_WITHIN_EEPROM = 10;         // Size of Data in Array[]...    0 -> 1023.
int i=0;

void setup() {
  Serial.begin(115200);                   // COM1 115.200 Baud...Standard(9600).
}

void loop() {


Serial.println("------- Start.. initialization data Array[]... ----");
for(i=0; i < DATA_SIZE_WITHIN_EEPROM; i++)
{
 DATAWITHINEEPROM[i] = 1234567890;        // Fill up data in the ARRAY.
 Serial.print(i);
 Serial.print(" = ");
 Serial.println(DATAWITHINEEPROM[i]);
}
Serial.println("---------------------------------------------------");



Serial.println("--- EEPROM-LIVING-AGE-WRITE..-------");
EEPROMADRESS = 0;                         // Start addresses for data.     0 -> 1023.
EEPROMADRESS_LAST = 100;                  // End addresses for data.       0 -> 1023
i = EEPROMLivingAgeExtension.EEPROMLIVINGAGEWRITE( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, DATAWITHINEEPROM );
Serial.println(i);                        // 0= OK,  1= ERROR!...
Serial.println("--- EEPROM-LIVING-AGE-WRITE END..---");



Serial.println("Reset the Arrays....");
for(i=0; i < DATA_SIZE_WITHIN_EEPROM; i++){ // Reset the Arrays....
  DATAWITHINEEPROM[i] = 0; 
}



Serial.println("--- EEPROM-LIVING-AGE-READ.. -------");
EEPROMADRESS = 0;                         // Start addresses for data.     0 -> 1023.
EEPROMADRESS_LAST = 100;                  // End addresses for data.       0 -> 1023. 
i = EEPROMLivingAgeExtension.EEPROMLIVINGAGEREAD( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, DATAWITHINEEPROM );
Serial.println(i);                        // 0= OK,  1= ERROR!...
for(i=0; i < (sizeof(DATAWITHINEEPROM)/4); i++){
 Serial.print(i);
 Serial.print(" = ");
 Serial.println(DATAWITHINEEPROM[i]); 
}
Serial.println("--- EEPROM-LIVING-AGE-READ END -----");



Serial.println("--- EEPROM-LIVING-AGE-LIFE-COUNTDOWN.. ----");
EEPROMADRESS = 0;                         // Start addresses for data.     0 -> 1023.
EEPROMADRESS_LAST = 100;                  // End addresses for data.       0 -> 1023. 
i = EEPROMLivingAgeExtension.EEPROMLIVINGAGELIFECOUNTDOWN( EEPROMADRESS, EEPROMADRESS_LAST, DATASIZEWITHINEEPROM, LIFECOUNTDOWN );
Serial.print("EEPROM LIFECOUNTDOWN = "); 
Serial.println(LIFECOUNTDOWN);            // LIFECOUNTDOWN
Serial.println(i);                        // 0= OK,  1= ERROR!...
Serial.println("--- EEPROM-LIVING-AGE-LIFE-COUNTDOWN END --");



while(1);  // END
}
```

#### Output for the Datatype: Long

```
------- Start.. initialization data Array[]... ----
0 = 1234567890
1 = 1234567890
2 = 1234567890
3 = 1234567890
4 = 1234567890
5 = 1234567890
6 = 1234567890
---------------------------------------------------
--- EEPROMLIVINGAGEWRITE..-------
0
--- EEPROMLIVINGAGEWRITE END..---
Reset the Arrays....
--- EEPROMLIVINGAGEREAD.. -------
0
0 = 1234567890
1 = 1234567890
2 = 1234567890
3 = 1234567890
4 = 1234567890
5 = 1234567890
6 = 1234567890
--- EEPROMLIVINGAGESREAD END -----
--- EEPROMLIVINGAGELIFECOUNTDOWN.. ----
EEPROM LIFECOUNTDOWN = 315624
0
--- EEPROMLIVINGAGELIFECOUNTDOWN END -- 
```


#### Output for the Datatype: Integer
```
------- Start.. initialization data Array[]... ----
0 = 12345
1 = 12345
2 = 12345
3 = 12345
4 = 12345
5 = 12345
6 = 12345
---------------------------------------------------
--- EEPROMLIVINGAGEINTWRITE..-------
0
--- EEPROMLIVINGAGEINTWRITE END..---
Reset the Arrays....
--- EEPROMLIVINGAGEINTREAD.. -------
------------------------------------
0 = 12345
1 = 12345
2 = 12345
3 = 12345
4 = 12345
5 = 12345
6 = 12345
--- EEPROMLIVINGAGESINTREAD END -----
--- EEPROMLIVINGAGEINTLIFECOUNTDOWN.. ----
EEPROM LIFECOUNTDOWN = 561110
0
--- EEPROMLIVINGAGEINTLIFECOUNTDOWN END -- 
```


# Copyright (c) 2018, Knottis
All rights reserved.

#### Special thanks to  "Sams Teach Yorself Arduino Programming - in 24-Hours" - Richard Blum
https://www.amazon.com/Arduino-Programming-Hours-Teach-Yourself/dp/0672337126
