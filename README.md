# Lüftermonster

![Photo of the Lüftermonster PCB](assets/pcb-photo.jpg?raw=true "Title")

## TL;DR:
You want to control up to 15 PC Fans by PWM, read the RPM of every single one, attach up to 4 DS18b20 Temperature Probes, and have all this exposed by API, Web-interface or into Home-Assistant? This is for you!

## Description
Many water based central heating systems that use radiators benefit from a reduced system temperature. Whether it´s fuel-value gas or oil, or a heat-pump, reducing the systems primary temperature results in energy savings.

Radiators can only radiate a certain amount of heat defined by their construction and size - unless we force some air through them! This can be done passively by building a "chimney" around them and using the chimney-effect, and/or by attaching fans. Many commercially available fan-retrofitting kits or even radiators with fans included use noisy low quality fans, don´t allow for a lot of control, and are often very expensive. So you might want to add your own.

Lüftermonster is a project that provides you with a hardware PCB and software to control up to 15 PC Fans, allowing you to make them noiseless by PWM. The tacho signals (RPM) of all fans are monitored so you can monitor them and detect broken fans. Furthermore, you can add up to 4 DS18b20 temperature sensors in order to measure water- and air-temperature to finely control your fan-speed depending on your heating and room situation. Lüftermonster is WIFI-enabled by relying on a trusty ESP32.

By implementing Lüftermonster using [ESPHome](https://esphome.io/), we make it maintainable and easily customizable. It integrates seamlessly into [Home-Assistant](https://www.home-assistant.io/) and other home-automation solutions. If you don´t have or want one, you can use the local API or web-based interface to control and monitor your Lüftermonster.

The hardware is designed so that it can be easily and affordably ordered off [JLCPCB](https://jlcpcb.com/) ready-made with components soldered by them!

# Building one
## What you need
- Lüftermonster PCB(s).
- A USB-to-Serial adapter, like the ubiquitous "FTDI" ones. Find them off your local maker store, EBay, AliExpress, Amazon or similar.
- "Dupont" female-to-female cables to connect Lüftermonster to the USB-to-Serial adapter.
- 20x5mm 2A slow fuses.
- Up to 15 PC Fans per Lüftermonster: 12V, 4-pin.
- (Optional: up to 4 DS18b20 temperature probes per Lüftermonster, including 3-pin cables)


## Hardware
TODO: somethingsomething gerber
TODO: somethingsomething JLCPCB BOM & CPL Excel files

## Software
### Preparation
Install ESPHome, please refer to their excellent ["Getting Started" Sections](https://esphome.io/). Note: You only need to get esphome itself running, not write any YAML-Files yourself.

Copy `firmware/secrets.yaml.template` to `firmware/secrets.yaml`, and edit according to your environment.

Open and edit `firmware/lueftermonster.yaml` according to your environment. Note: If you have multiple Lüftermonsters, please assign unique and meaningful names like "lueftermonster-livingroom-left" or similar.

### Programming
Connect the USB-to-Serial adapter to your Lüftermonster, as follows:
  - VCC -> 3v3
  - GND -> GND
  - RX -> TX
  - TX -> RX
  - EN -> CTS
  - IO0 -> DTR

Attach your USB-to-Serial adapter to your computer

Follow the ESPHome Getting-Started guide you´ve chosen to "run" or "upload" `firmware/lueftermonster.yaml`.

Note: `esptool`, which is internally used by ESPHome to program the ESP32 tries to reset the ESP32 and bring it into programming mode. If this does not work, it may be that "Automatic bootloader mode" does not work. Please [refer to the documentation](https://docs.espressif.com/projects/esptool/en/latest/esp32/advanced-topics/boot-mode-selection.html#automatic-bootloader).

### Usage
After a brief period, you should be able to open `http://<the-name-you-set-in-the-yaml>.local/`, for example http://lueftermonster-livingroom-left.local/ .

Now, you can control the fan speed, and check the fans current RPMs. Note: RPM are not updated often, so it make take a while for all of them to display, and for speed changes to become visible. This is intentional since the information is most useful for outage detection, and does not need to be reported often.

To connect to Home-Assistant refer to the [ESPHome documentation](https://esphome.io/guides/getting_started_hassio#connecting-your-device-to-home-assistant)

### Connecting DS18b20 Temperature Sensors
TODO: Section about DS18b20 address finding and such.