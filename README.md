# Raspberry Pi: Raspberry Pi und MCP3008 (Analog-Digital Wandler auf eigener Platine)

![](images/20170114_192104.jpg)

## Einleitung
Viele Sensoren bieten keine digitale Schnittstelle und sind nur analog auslesbar.
Der Raspberry Pi mit seinen GPIOs kann keine analogen Signale auslesen. Um analoge Sensoren am Raspberry Pi auslesen zu könnenen, benötigen wir einen Analog-Digital Wandler z.B. den MCP3008. Damit können bis zu 8 analoge Eingänge über den SPI Bus am Raspberry Pi ausgelesen werden.

## Vorbereitung
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python-dev git
```

### Hinweis Rasperry Pi Zero bzw. bei Raspbian Stretch Lite
Das PHP-Modul **spidev** muss noch installiert werden!

```
sudo apt-get install python-pip
sudo pip install spidev
sudo apt-get install python3-dev
sudo apt-get install python3-pip
sudo pip3 install spidev
```

Aktiviere den SPI Bus über die Config-Oberfläche: `sudo raspi-config`

![](images/raspi-config1.png)

![](images/raspi-config2.png)

![](images/raspi-config3.png)

![](images/raspi-config4.png)

Starte danach zur Sicherheit das System neu (Reboot).

### SPI in der grafischen Oberfläche aktivieren
Bevor es weiter geht, muss der SPI Bus noch aktiviert werden, wenn das noch nicht geschehen ist. Aktiviere in der Konfiguration des Raspberry Pi unter dem Punkt „Schnittstellen“ die Option „SPI“. Starte danach zur Sicherheit das System neu (Reboot).

![](images/spi_raspberry-pi.png)

Bevor es weiter geht, muss der SPI Bus noch aktiviert werden, wenn das noch nicht geschehen ist. Aktiviere in der Konfiguration des Raspberry Pi unter dem Punkt „Schnittstellen“ die Option „SPI“. Starte danach zur Sicherheit das System neu (Reboot).

![](images/spi_raspberry-pi.png)

## weiter für alle Versionen des Raspberry Pi

Lade dir jetzt das passende Github-Repository für raspberry-pi-mcp3008-Sensor herunter:

```
cd Documents
git clone https://github.com/pediehl/raspberry-pi-mcp3008-board.git
```


## Aufbau

![](images/pidi_sensor_verbinden_mit_Raspberrypi.jpg)

MCP3008-Sensor | Raspberry Pi       | Raspberry Pi GPIO-PINS
---------------|--------------------|-----------------------
\+3V             | Raspberry Pi 3.3 V | 3.3V
GND            | Raspberry Pi GND   | GND
SCLK           | Raspberry Pi SCLK  | GP 11
MISO           | Raspberry Pi MISO  | GP 9
MOSI           | Raspberry Pi MOSI  | GP 10
CE0            | Raspberry Pi CE0   | GP 8


## Beispiele starten

Auf deinem Raspberry solltest du jetzt unter _**Documents**_ einen weiteren Ordner  haben. Öffnne den Ordner _**raspberry-pi-mcp3008-board**_ und gehe auf _**code**_. Hier gibt es drei Python-Skripte:

+ ldr.py
+ poti_ohm.py
+ poti_prozent.py
+ wassersensor.py


## Wie starte ich das Python-Skript auf meinem Raspbery Pi?
### grafische Benutzeroberfläche
Wechsel in den Ordner _**Documents/raspberry-pi-mcp3008-board/code/**_
Wähle eine gewünschte Datei, mach einen Klick mit der _rechten Maustaste_. Das Kontext-Menü erscheint.

![Datei auswählen und Kontext-Menü pber die rechte Mausstaste](images/dateibereich_python3_auswaehlen.png)

Starte das Programm mit F5.

### Kommandozeile
Wechsel in den Ordner _**Documents/raspberry-pi-mcp3008-board/code/**_
Starte ein Beispiel mit: `python3 ldr.py`

#### **_Achtung: Die Beispiele funktionieren nur mit Python 3 !_**

## Aufbau Foto-Widerstand (LDR)
### Material
* Kabel
* Photo-Zelle
* Widerstand 10 KOhm

Der Widerstand wird benötigen um einen Spanungsteiler aufzubauen. Besser: Über den Widerstand wird dafür gesorgt, dass nicht zu viel Spannung auf dem MCP3008 ankommt.

![](images/pidi_sensor_steckplatine_helligkeitssensor_Steckplatine.jpg)

#### Berechnung in Ohm

```python
import time, gpiozero

ldr = gpiozero.MCP3008 (channel = 0)

while True :
 wert =  int((ldr.raw_value / 1023) * 1000)
 print ("Der Wert am Foto-Widerstand beträgt: " , wert , "Ohm")
 time.sleep (1)
```

## Aufbau Potentiometer
### Material
* Kabel
* Potentiometer

![](images/pidi_sensor_steckplatine_potentiometer_Steckplatine.jpg)

#### Berechnung in Prozent
```python
import time, gpiozero

poti = gpiozero.MCP3008 (channel = 0)

while True :
 wert = int ( poti.value * 100)
 print ("Der Poti steht auf" , wert , "%")
 time.sleep (1)
```

#### Berechnung in Ohm
```python
import time, gpiozero

poti = gpiozero.MCP3008 (channel = 0)

while True :
 wert =  int((poti.raw_value / 1023) * 1000)
 print ("Der Poti steht auf" , wert , "Ohm")
 time.sleep (1)

```

## Aufbau Wassersensor
### Material
* Kabel
* Wassersensor

![](images/pidi_sensor_steckplatine_wassersensor_Steckplatine.jpg)

#### Berechnung Spannung in Volt
```python
import time, gpiozero

adc = gpiozero.MCP3008 (channel = 0)

while True:
 voltage = adc.voltage
 print("Spannung am Wassersensor: %.2f V" % voltage)
 time.sleep (1)
```
