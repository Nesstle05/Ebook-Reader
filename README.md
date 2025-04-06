# Ebook-Reader


### Pasii de implementare
* Am creat schematicul 2D.
* Am creat PCB-ul, am intampinat niste erori:
    * **Via(hole) error** la componenta USB: am modificat footprint-ul componentei.
    * **Wire to wire error**: am rutat firele manual
* Am atribuit fiecarei componente din library cate un model 3D, si am creat placa 3d (*Push to 3D*)
* Am creat modelele pentru Display si Baterie dupa specificatiile din datasheeturi

## Diagrama bloc
![image](https://github.com/user-attachments/assets/5903e4ba-dd43-4d0f-bf3c-feaa2c6ee449)


## Utilizarea Pinilor ESP32-C6

Următorul tabel detaliază pinii GPIO specifici utilizați pe modulul ESP32-C6-WROOM-1 și funcțiile acestora în cadrul acestui proiect:

| Pin ESP32-C6 | Funcție                           | Component(e) Conectat(e)               | Interfață/Motiv                                              |
| :----------- | :-------------------------------- | :------------------------------------- | :----------------------------------------------------------- |
| GPIO0        | Selecție Boot / Intrare Buton     | SW_BOOT (Buton Boot), R_BOOT         | Pin standard pentru selecția modului boot; Intrare utilizator |
| GPIO1        | Ceas I2C                          | U4 (MAX17048), U3 (DS3231), SENSOR2 (BME688), J5 (Qwiic) | Magistrală I2C comună (SCL) pentru multipli senzori/CI        |
| GPIO2        | Date I2C                          | U4 (MAX17048), U3 (DS3231), SENSOR2 (BME688), J5 (Qwiic) | Magistrală I2C comună (SDA) pentru multipli senzori/CI        |
| GPIO3        | Selecție Cip E-Paper              | J1 (Header EPD)                        | Selecție Cip SPI (CS) pentru Afișajul E-Paper              |
| GPIO4        | Dată/Comandă E-Paper              | J1 (Header EPD)                        | Control Dată/Comandă (DC) pentru Afișajul E-Paper          |
| GPIO5        | Reset E-Paper                     | J1 (Header EPD)                        | Semnal Reset (RST) pentru Afișajul E-Paper               |
| GPIO6        | Stare Ocupat E-Paper              | J1 (Header EPD)                        | Semnal intrare Ocupat (Busy) de la Afișajul E-Paper          |
| GPIO7        | Hold Flash Extern                 | U1 (W25Q64JV), TP17                    | Semnal SPI Hold (/HOLD) pentru memoria flash externă         |
| GPIO8        | Dată 0 Card SD                    | J2 (Slot Card SD), TP6                 | Linie Date SDIO 0 / SPI MISO                               |
| GPIO9        | Dată 1 Card SD                    | J2 (Slot Card SD), TP7                 | Linie Date SDIO 1                                          |
| GPIO10       | Comandă Card SD                   | J2 (Slot Card SD), TP5                 | Linie Comandă SDIO / SPI MOSI                              |
| GPIO11       | Ceas Card SD                      | J2 (Slot Card SD), TP4                 | Linie Ceas SDIO / SPI SCK                                  |
| GPIO12       | Protecție Scriere Flash Extern    | U1 (W25Q64JV), TP16                    | Semnal SPI Protecție Scriere (/WP) pentru flash extern      |
| GPIO13       | MISO Flash Extern                 | U1 (W25Q64JV), TP15                    | SPI Master In Slave Out (MISO) pentru flash extern         |
| GPIO14       | MOSI Flash Extern                 | U1 (W25Q64JV), TP14                    | SPI Master Out Slave In (MOSI) pentru flash extern         |
| GPIO15       | Selecție Cip Flash Extern         | U1 (W25Q64JV), TP13                    | Selecție Cip SPI (/CS) pentru memoria flash externă        |
| GPIO16       | UART TXD                          | TP12                                   | Date Transmise UART pentru depanare/programare             |
| GPIO17       | UART RXD                          | TP11                                   | Date Recepționate UART pentru depanare/programare          |
| GPIO18       | Ceas Flash Extern                 | U1 (W25Q64JV), TP3                     | Ceas SPI (SCK) pentru memoria flash externă                |
| GPIO19       | Dată 2 Card SD                    | J2 (Slot Card SD), TP8                 | Linie Date SDIO 2                                          |
| GPIO20       | Dată 3 Card SD / Detectare Card   | J2 (Slot Card SD), TP9                 | Linie Date SDIO 3 / Intrare Detectare Card                 |
| GPIO21       | Intrare Buton Change              | SW_CHANGE (Buton Change)               | Intrare de Uz General pentru interacțiune utilizator         |
| USB_D-       | Date Negative USB                 | J4 (Conector USB-C)                    | Semnal Nativ USB D-                                        |
| USB_D+       | Date Pozitive USB                 | J4 (Conector USB-C)                    | Semnal Nativ USB D+                                        |
| EN           | Activare Sistem / Reset           | U5 (BD5229G), SW_RESET (Buton Reset)   | Activare Cip, controlat de supervizor tensiune/buton       |

## Energy management

| Component                   | Power Supply (V) | Estimated Current (mA) | Estimated Power (W) | Notes                                                                  |
|-----------------------------|-------------------|------------------------|----------------------|------------------------------------------------------------------------|
| ESP32-C6-WROOM-1-N8         | 3.3               | 150 (average)          | 0.5                  | Active mode; varies significantly based on usage.                       |
| SD Card Module              | 3.3               | 10 (average)           | 0.033                | Intermittent read/write operations.                                     |
| E-Paper Display             | 3.3               | 1(average)             | 0.0033               | Power used only during updates, very low average.                   |
| RTC Module - DS3231SN       | 3.3               | 1                      | 0.0033               |                                                                        |
| LiPo Fuel Gauger (MAX17048) | 3.3               | 0.05                   | 0.000165             | Very low power consumption.                                            |
| BME688 (Environmental Sensor) | 3.3               | 1 (average)            | 0.0033               | Intermittent readings.                                                  |
| NORFlash64MB (External Flash)| 3.3               | 2 (average)            | 0.0066               | Intermittent read/write operations.                                     |
| LDO Voltage Regulator       | 5 (input) / 3.3 (output) | Variable             | Variable               | Efficiency depends on load; quiescent current is very low.             |
| USB-C Connector + ESD       | 5 (input)         | Variable               | Variable               | Power depends on charging current; ESD protection negligible.          |
| Reset/Boot Buttons          | N/A               | Negligible             | Negligible           |                                                                        |
| **Total (Approximate)** |                   |          150-200              | **~0.55** | This is a rough estimate; actual power depends on duty cycles and usage. |


## Bill Of Materials (BOM)
| Componenta | Achizitie | Datasheet |
| ---------- | --------- | --------- |
| PFMF.050.1 | [Cumpara](https://ro.mouser.com/ProductDetail/Schurter/PFMF.050.2?qs=1auRipcfynCums5v1iucSA%3D%3D) |  [Datasheet](https://ro.mouser.com/datasheet/2/358/typ_PFMF-1275918.pdf) |
| USB4110-GF-A | [Cumpara](https://ro.mouser.com/ProductDetail/GCT/USB4110-GF-A?qs=KUoIvG%2F9IlYiZvIXQjyJeA%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/837/GCT_USB4110_Product_Drawing___20k_cycles-3455479.pdf) |
| USBLC6-2SC6Y | [Cumpara](https://ro.mouser.com/ProductDetail/STMicroelectronics/USBLC6-2SC6Y?qs=gNDSiZmRJS%2FOgDexvXkdow%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/389/usblc6_2sc6y-1852505.pdf) |
| SD0805S020S1R0 | [Cumpara](https://ro.mouser.com/ProductDetail/KYOCERA-AVX/SD0805S020S1R0?qs=jCA%252BPfw4LHbpkAoSnwrdjw%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/schottky-3165252.pdf) |
| DMG2305UX-7 | [Cumpara](https://ro.mouser.com/ProductDetail/Diodes-Incorporated/DMG2305UX-7?qs=L1DZKBg7t5F%2FNBHrjfxC%252Bg%3D%3D) | [Datasheet](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf) |
| XC6220A331MR-G | [Cumpara](https://ro.mouser.com/ProductDetail/Torex-Semiconductor/XC6220A331MR-G?qs=AsjdqWjXhJ8ZSWznL1J0gg%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/760/xc6220-3371556.pdf) |
| Condensator 100 uF TANT | [Cumpara](https://ro.mouser.com/ProductDetail/KYOCERA-AVX/TAJW107M010RNJ?qs=Wtp%252Bf%2FAeVqIH8v1VxV%252B1Rg%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/TAJ-3165264.pdf) |
| 112A-TAAR-R03_ATTEND | [Cumpara](https://www.digikey.ro/en/products/detail/attend-technology/112A-TAAR-R03/17633923) | [Datasheet](https://www.attend.com.tw/data/download/file/112A-TAAR-R03_Spec.pdf)
| ESP32-C6-WROOM-1-N8 | [Cumpara](https://ro.mouser.com/ProductDetail/Espressif-Systems/ESP32-C6-WROOM-1-N8?qs=8Wlm6%252BaMh8ST02Gmwp74cw%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/891/Espressif_ESP32_C6_WROOM_1__Datasheet_V0_1_PRELIMI-3239987.pdf) |
| MCP73831 | [Cumpara](https://ro.mouser.com/ProductDetail/Microchip-Technology/MCP73831T-2ACI-OT?qs=yUQqVecv4qvbBQBGbHx0Mw%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/268/MCP73831_Family_Data_Sheet_DS20001984H-3441711.pdf) |
| CHG_LED | [Cumpara](https://store.comet.srl.ro/Catalogue/Product/40478/) | [Datasheet](https://www.snapeda.com/parts/KP-1608SURCK/Kingbright/datasheet/) |
| SI1308EDL-T1-GE3 | [Cumpara](https://ro.mouser.com/ProductDetail/Vishay-Semiconductors/SI1308EDL-T1-GE3?qs=bX1%252BNvsK%2FBramh9tgpOaEw%3D%3D) | [Datasheet](https://www.vishay.com/doc?63399) |
| MBR0530 | [Cumpara](https://ro.mouser.com/ProductDetail/Micro-Commercial-Components-MCC/MBR0530-T?qs=9VyI4qLX4NTSXkb9ynzJnA%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/258/mcc_mbr0520~mbr0560sod123-1179695.pdf) |
| Bobina | [Cumpara](https://ro.mouser.com/ProductDetail/Wurth-Elektronik/744043680?qs=PGXP4M47uW6VkZq%252BkzjrHA%3D%3D) | [Datasheet](https://www.we-online.com/components/products/datasheet/744043680.pdf) |
| FH34SRJ-24S-0.5SH_99_ | [Cumpara](https://ro.mouser.com/ProductDetail/Hirose-Connector/FH34SRJ-24S-0.5SH99?qs=vcbW%252B4%252BSTIpKBl5ap9J8Fw%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/185/FH34SRJ_24S_0_5SH_99__CL0580_1255_6_99_2DDrawing_0-1615044.pdf) |
| BME688 | [Cumpara](https://ro.mouser.com/ProductDetail/Bosch-Sensortec/BME688?qs=IS%252B4QmGtzzqQoVDscqwx3A%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/783/bst_bme688_fl000-2307034.pdf) |
| BD5229G-TR | [Cumpara](https://ro.mouser.com/ProductDetail/ROHM-Semiconductor/BD5229G-TR?qs=4kLU8WoGk0vvnhrrYwdszw%3D%3D) | [Datasheet](https://fscdn.rohm.com/en/products/databook/datasheet/ic/power/voltage_detector/bd52xxg-e.pdf) |
| Buton adaptat | [Cumpara](https://ro.mouser.com/ProductDetail/CK/KMR221GULCLFS?qs=u2NJ%252B70r0goBXaNk7IrU0Q%3D%3D) | [Datasheet](https://www.ckswitches.com/media/1479/kmr2.pdf) |
| MAX17048G+T10 | [Cumpara](https://ro.mouser.com/ProductDetail/Analog-Devices-Maxim-Integrated/MAX17048G%2bT10?qs=D7PJwyCwLAoGnnn8jEPRBQ%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/609/MAX17048_MAX17049-3469099.pdf) |
| W25Q512JVEIQ | [Cumpara](https://ro.mouser.com/ProductDetail/Winbond/W25Q512JVEIQ?qs=l7cgNqFNU1jw6svr3at6tA%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/949/Winbond_W25Q512JV_Datasheet-3240039.pdf) |
| PGB1010603MR | [Cumpara](https://ro.mouser.com/ProductDetail/Littelfuse/PGB1010603MRHF?qs=KvZd0dN2Zg%2FuIq6icj%252BGKA%3D%3D) | [Datasheet](https://www.littelfuse.com/media?resourcetype=datasheets&itemid=8a337998-d54d-466b-be4e-dc5bcd1f9321&filename=littelfuse_pulseguard_pgb1_datasheet.pdf) |
| QWIIC_RIGHT_ANGLE | [Cumpara](https://ro.mouser.com/ProductDetail/GCT/USB4110-GF-A?qs=KUoIvG%2F9IlYiZvIXQjyJeA%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/837/GCT_USB4110_Product_Drawing___20k_cycles-3455479.pdf) |
| DS3231SN# | [Cumpara](https://ro.mouser.com/ProductDetail/Analog-Devices-Maxim-Integrated/DS3231SN?qs=ffX8NcjNb2RmKAb9wAk9Ug%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/609/DS3231-3421123.pdf) |
| CPH3225A C10_SUPERCAP | [Cumpara](https://ro.mouser.com/ProductDetail/Seiko-Semiconductors/CPH3225A?qs=3etwrb1wR%252BhUOph6lAO7eg%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/360/Seiko_Instruments_MicroBattery_E_20230330_2024Jan_-3561061.pdf) |
| Condensator 100nF | [Cumpara](https://ro.mouser.com/ProductDetail/KYOCERA-AVX/06033G104ZAT2A?qs=NXubJDmysXJMPmHfVo6Z%252BA%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/KGM_Y5V-3223189.pdf) |
| Condensator 4.7uF | [Cumpara](https://ro.mouser.com/ProductDetail/KYOCERA-AVX/0402ZD475MAT2A?qs=NBFAU1oqP4W4U2PCPHI0sg%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/cx5r_KGM-3223198.pdf) |
| Condensator 10uF | [Cumpara](https://ro.mouser.com/ProductDetail/Samsung-Electro-Mechanics/CL10A106KQ8NNNL?qs=xZ%2FP%252Ba9zWqaes9JKSsob2Q%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/585/MLCC-1837944.pdf) |
| Condensator 1uF/50V | [Cumpara](https://ro.mouser.com/ProductDetail/KYOCERA-AVX/06035D105MAT2A?qs=k4kUdCzLgS5%252BURKe1SOIeQ%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/40/cx5r_KGM-3223198.pdf) |
| Rezistenta 10K | [Cumpara](https://ro.mouser.com/ProductDetail/Vishay-Beyschlag/MCS0402MD1002BE000?qs=17u8i%2FzlE8%2F9BrAaXkMk1w%3D%3D) | [Datasheet](https://www.vishay.com/doc?28952) |
| Rezistenta 200 | [Cumpara](https://ro.mouser.com/ProductDetail/Vishay-Beyschlag/MCS0402MD2000BE100?qs=3SvaY9RawMJNVte4F12%252BZQ%3D%3D) | [Datasheet](https://www.vishay.com/doc?28952) |
| Rezistenta 100K | [Cumpara](https://ro.mouser.com/ProductDetail/Vishay-Beyschlag/MCT0603PD1003DP500?qs=5aG0NVq1C4wWAn8Ei3OpZA%3D%3D) | [Datasheet](https://www.vishay.com/doc?28916) |
| Rezistenta 2.2 | [Cumpara](https://ro.mouser.com/ProductDetail/SEI-Stackpole/RMCF0402FT2R20?qs=IPgv5n7u5QbBgyl0jwhwsA%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/385/SEI_RMCF_RMCP-3077565.pdf) |
| Rezistenta 5K1| [Cumpara](https://www.venkel.com/part/TFCR0603-16W-K-5690FT) | [Datasheet](https://data.venkel.com/documents/tfcr-series?_gl=1*tdo8m*_ga*NTA1MTc5MTcyLjE3NDM4OTMzNjc.*_ga_JRKGBZNVM8*MTc0Mzg5MzM2Ni4xLjAuMTc0Mzg5MzM2OC41OC4wLjA.) |
| Rezistenta 2K | [Cumpara](https://ro.mouser.com/ProductDetail/Vishay-Beyschlag/MCS04020C2001FE000?qs=wTZ%2FFzl837YG0wIkZJJOwQ%3D%3D) | [Datasheet](https://www.vishay.com/doc?28705) |
| Rezistenta 0.47 | [Cumpara](https://ro.mouser.com/ProductDetail/SEI-Stackpole/CSR0402JKR470?qs=IPgv5n7u5QawIB6nBEt8wA%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/385/SEI_CSR_CSRN-3077593.pdf) |
| Rezistenta 15 | [Cumpara](https://ro.mouser.com/ProductDetail/YAGEO/RT0402FRE0715RL?qs=BXCcY9r%252B08DFFpLSkPOIqQ%3D%3D) | [Datasheet](https://ro.mouser.com/datasheet/2/447/PYu_RT_1_to_0_01_RoHS_L_15-3461507.pdf) |
