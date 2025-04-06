# DeskAssistant v1.5 – README

## Diagrama bloc
![Diagrama bloc](./Images/diagram.png)

---

## Bill of Materials (BoM)

| Componentă | Link achiziție (Mouser/Comet) | Datasheet |
|-----------|-------------------------------|-----------|
| ESP32-C6-WROOM-1-N8 | [Mouser](https://www.mouser.com/ProductDetail/Espressif/ESP32-C6-WROOM-1-N8) | [Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c6-wroom-1_datasheet_en.pdf) |
| BME688 (senzor ambiental) | [Mouser](https://www.mouser.com/ProductDetail/Bosch/BME688) | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme688-ds000.pdf) |
| DS3231SN (RTC) | [Mouser](https://www.mouser.com/ProductDetail/Maxim-Integrated/DS3231SN) | [Datasheet](https://datasheets.maximintegrated.com/en/ds/DS3231.pdf) |
| MCP73831 (încărcător Li-Po) | [Mouser](https://www.mouser.com/ProductDetail/Microchip-Technology/MCP73831T-2ACI-OT) | [Datasheet](https://ww1.microchip.com/downloads/en/DeviceDoc/20001984g.pdf) |
| MAX17048 (fuel gauge) | [Mouser](https://www.mouser.com/ProductDetail/Maxim-Integrated/MAX17048G-T10) | [Datasheet](https://datasheets.maximintegrated.com/en/ds/MAX17048.pdf) |
| W25Q512JVEIQ (flash extern) | [Mouser](https://www.mouser.com/ProductDetail/Winbond/W25Q512JVEIQ) | [Datasheet](https://www.winbond.com/resource-files/w25q512jv%20revf%2003272018%20plus.pdf) |
| USBLC6-2SC6Y (protecție ESD) | [Mouser](https://www.mouser.com/ProductDetail/STMicroelectronics/USBLC6-2SC6Y) | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2sc6.pdf) |
| BD5229G-TR (regulator LDO 2.9V) | [Mouser](https://www.mouser.com/ProductDetail/ROHM-Semiconductor/BD5229G-TR) | [Datasheet](https://www.rohm.com/datasheet/BD5229G) |
| MBR0530 (diode Schottky) | [Mouser](https://www.mouser.com/ProductDetail/ON-Semiconductor/MBR0530T1G) | [Datasheet](https://www.onsemi.com/pdf/datasheet/mbr0530-d.pdf) |
| SI1308EDL-T1-GE3 (MOSFET P-Ch) | [Mouser](https://www.mouser.com/ProductDetail/Vishay/SI1308EDL-T1-GE3) | [Datasheet](https://www.vishay.com/docs/74275/si1308edl.pdf) |
| Conector USB-C (USB4110-GF-A) | [Mouser](https://www.mouser.com/ProductDetail/GCT/USB4110-GF-A) | [Datasheet](https://gct.co/files/drawings/usb4110.pdf) |
| Conector QWIIC | [Sparkfun](https://www.sparkfun.com/products/14417) | [Datasheet](https://cdn.sparkfun.com/datasheets/BreakoutBoards/Qwiic_Connector_Data_Sheet.pdf) |

---

## Funcționalități hardware

### 1. Microcontroller – ESP32-C6-WROOM-1-N8
- **Wireless:** WiFi 6, BLE 5.0, Zigbee, Thread
- **Interfețe:** UART, I²C, SPI, USB, ADC, PWM
- **Rol:** Unitate centrală de control, achiziție date, comunicare cu periferice și backend.

### 2. Senzori
- **BME688**: măsoară temperatură, umiditate, presiune și compuși organici volatili (VOC)
  - **Interfață**: I²C
- **DS3231SN**: RTC (Real Time Clock)
  - **Interfață**: I²C
- **MAX17048**: fuel gauge pentru baterie
  - **Interfață**: I²C

### 3. Memorie externă
- **W25Q512JVEIQ**
  - **Tip**: NOR Flash, 512Mbit
  - **Interfață**: SPI

### 4. Managementul energiei
- **MCP73831**: încărcător Li-Ion
- **MAX17048**: monitorizare baterie
- **BD5229G-TR**: LDO pentru 2.9V stabil
- **Diodă Schottky, MOSFET, varistor**: protecție la supratensiuni și polarizare inversă

### 5. Conectivitate și I/O
- **USB4110-GF-A**: conector USB-C pentru alimentare/comunicare
- **QWIIC connector**: I²C modular pentru senzori plug-and-play
- **LED status** și **buton fizic** (boot/reset sau input utilizator)

---

## Conexiuni GPIO la ESP32-C6

| Componentă | Interfață | GPIO ESP32-C6 | Funcție |
|------------|-----------|----------------|---------|
| BME688 / DS3231 / MAX17048 | I²C | GPIO8 (SDA), GPIO9 (SCL) | Magistrală partajată |
| W25Q512JVEIQ | SPI | GPIO4 (MOSI), GPIO5 (MISO), GPIO6 (CLK), GPIO7 (CS) | Comunicare cu flash extern |
| LED status | GPIO | GPIO3 | Indicator status / PWM |
| Buton | GPIO + Pull-up | GPIO0 | Reset / boot / input utilizator |
| MCP73831 – STAT | GPIO | GPIO10 | Monitorizare stare încărcare |
| MAX17048 – ALERT | GPIO (Interrupt) | GPIO1 | Notificare SoC critic |
| USB-C – D+ / D- | USB | GPIO18 (D+), GPIO19 (D-) | Comunicare USB nativă |

---

## Estimare consum energetic

| Componentă | Consum tipic |
|------------|--------------|
| ESP32-C6 | ~130 mA (activ) |
| BME688 | ~3.1 µA |
| DS3231 | ~200 nA |
| MAX17048 | ~23 µA |
| MCP73831 | până la 500 mA (în încărcare) |
| LED-uri | 2–10 mA |

**Consum total în mod activ:** aprox. **150–180 mA**

---

## Alte detalii relevante

- PCB-ul include **test pads** pentru debugging
- **Selector de afișaj E-Paper** integrat în design
- Circuit de drive pentru E-Paper separat
- Proiectul include și **slot SD Card**, dar neutilizat în versiunea curentă

### Randări PCB și design fizic

![Randare PCB](./Images/pcbBoard.png)  
![Vedere explodată](./Images/exploded.png)  
![Ansamblu complet](./Images/openbookComplete.png)
