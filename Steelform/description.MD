# STEELFORM

<!-- TOC -->

* [STEELFORM](#steelform)
* [Obecné informace](#obecné-informace)
* [Hardware](#hardware)
    * [Seznam strojů](#seznam-strojů)
    * [Dodatečné informace](#dodatečné-informace)
    * [Dotykové terminály](#dotykové-terminály)
    * [Seznam strojů](#seznam-strojů-1)
    * [Součtová tabulka](#součtová-tabulka)
* [Software](#software)
    * [Komunikace Vario](#komunikace-vario)
    * [Logika](#logika)

<!-- TOC -->

# Obecné informace

1. IT správa serveru Pavel Štěpánek, tel. 774 108 100, email pavel.stepanek@steelform.cz
2. U strojů budou převážně zapojeny proudové transformátory
3. Digitální signály, které bude možnost připojit budou přijeny (např. maják)
4. Terminály budou osazeny na pracovních stolech na vesa ramenu (vesa držáky budou přidělány z boční strany stolu)

# Hardware

## Seznam strojů

1. fanuc C400
2. fanuc C600
3. fanuc C800
4. Hloubička penta
5. hurco vmx84i
6. hurco vm10
7. akira sv1350
8. hermle c12
9. hermle c400
10. hermle c650
11. hurco tm8i
12. buaj bruska
13. pila cosen
14. kompresory

## Dodatečné informace

- Celkem bude připojeno 13 strojů (13 malých jednotek, 1 velká jednotka)
- Při analýze bylo zjištěno, že bude potřeba:
    - **12 kusů** 25A proudových transformátorů
    - **24 kusů** 50A proudových transformátorů
    - **12 kusů** 100A proudových transformátorů
    - **4 kusy relé** pro připojení majáku na určité stroje.
    - **1 kus** externí krabice (jednotka se nevleze do stroje)

## Dotykové terminály

- Dotykových terminálů 8 kusů, k dotykovým terminálům je potřeba přivést ethernetový kabel (nemají wifi)
- Dotykové terminály budou připevněny na vesa držák
    - **7 kusů** kloubových stolník
    - **1 kus** nástěnný.

## Seznam strojů

![Součtová tabulka](seznam_stroju.png)

## Součtová tabulka
| Název            | Počet |
|------------------|-------|
| Jedotka malá     | 13    |
| Jednotka velká   | 1     |
| Trafo 25A        | 12    |
| Trafo 50A        | 24    |
| Trafo 100A       | 12    |
| Trafo 300A       | 0     |
| Extwerní krabice | 1     |
| Kabeláž          | 180   |
| Licence          | 14    |
| Shuttle 15''     | 8     |
| VESA kloubový    | 7     |
| VESA nástěnný    | 1     |
| Zdroj 24VDC      | 1     |
| Jistič 4A        | 1     |
| Jistič 2A        | 1     |

# Software

U zákazníka je instalován Debian x64 na dodaném PC.
Vzdálený přístup screenconnect.
Přidán virtuální monitor:

- `sudo apt install xserver-xorg-video-dummy`
- `sudo nano /etc/X11/xorg.conf`
- Editace:

```Section "Device"
    Identifier "DummyDevice"
    Driver "dummy"
    VideoRam 256000
EndSection

Section "Screen"
    Identifier "DummyScreen"
    Device "DummyDevice"
    Monitor "DummyMonitor"
    DefaultDepth 24
    SubSection "Display"
        Depth 24
        Modes "1920x1080_60.0"
    EndSubSection
EndSection

Section "Monitor"
    Identifier "DummyMonitor"
    HorizSync 30-70
    VertRefresh 50-75
    ModeLine "1920x1080" 148.50 1920 2448 2492 2640 1080 1084 1089 1125 +Hsync +Vsync
EndSection

```

## Komunikace Vario

- Služba steelform_communication_service
- Připojení skrze API.
- Import zakázek, operací a napárovaných pracovišť.
  -Importní služba vytváří databázovou tabulku steelform_orders a do ní v intervalu 60 vteřin importuje/aktualizuje
  data.

## Logika

- Služba steelform_terminal_webservice
- Prototyp
  zde: https://www.sketch.com/s/265c10c5-4f11-49fc-974d-fd1071bb4d76/prototype/6A10AA96-2904-4151-9FEE-90BBB3AF9323/a/6A10AA96-2904-4151-9FEE-90BBB3AF9323
- Logika využívá importovaná data a při provozu vytváří standardní data xMostu (zakázky, prostoje, ...)