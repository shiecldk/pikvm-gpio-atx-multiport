# pikvm-gpio-atx-multiport
This is a Pi-KVM GPIO implementation for ATX multiport support up to controlling 4 PC ports with the [XH-HK4401 4-port HDMI USB KVM Switch](https://docs.pikvm.org/xh_hk4401/) and a custom breakoutboard for ATX switch.

The implementation is accomplished through adding the addtional GPIO pins to the Pi-KVM Web GUI under /etc/kvmd/override.yaml.

<p>
<img src="https://raw.githubusercontent.com/shiecldk/pikvm-gpio-atx-multiport/main/images/kvm-atx-menu.png" alt="kvm-atx-menu" class="center"></p>

## Hardware Requirements
Make sure you have the 3 hardwares to make the KVM switch work:

1. Pi-KVM V2 with RPi 4B model
2. A HDMI USB KVM Switch
3. A Custom ATX Switch

## Installation
### HDMI USB KVM Switch
Follow the guide to setup the hardware for HDMI USB KVM Switch. In this project's instance, the [XH-HK4401 KVM Switch](https://docs.pikvm.org/xh_hk4401/) is used.

### kvdm files
Set the Web GUI for ATX Switch using the following guide:

In the Pi-KVM terminal:
Obtain the root permission:<br>
`su -`

Set to read-write mode:<br>
`rw`

Go to the /etc/kvdm directory:<br>
`cd /etc/kvdm`

Make backup of override.yaml and web.css:<br>
`cp override.yaml override.yaml.bak`
`cp web.css web.css.bak`

Copy override.yaml and web.css in this repo to your Pi-KVM OS, e.g., through SFTP or through `nano` to edit the files:<br>
`nano override.yaml`
`nano web.css`

Set the OS back to read-only mode:<br>
`ro`

Restart the kvmd service:<br>
`systemctl restart kvmd`


## Pin Mapping
### Pi-KVM V2 Diagram
Take a reference of the pin mapping for \_PWRSW\_, \_RESET\_, and \_PLED\_ from the Pi-KVM V2 guide to setup your own breakoutboard for ATX switch.

<p>
<img src="https://raw.githubusercontent.com/pikvm/pikvm/master/img/v2.png" alt="v2" class="center"></p>

### GPIO Mapping
The following GPIO pins are used for controlling 4 PCs:

PCs | \_PWRSW\_ Pin | \_RESET\_ Pin | \_PLED\_ Pin
--- | --- | --- | ---
PC1 | GPIO #23 | GPIO #27 | GPIO #24
PC2 | GPIO #18 | GPIO #17 | GPIO #25
PC3 | GPIO #16 | GPIO #19 | GPIO #12
PC4 | GPIO #20 | GPIO #26 | GPIO #21

## Reference
### Raspberry Pi 4 GPIO Pins
<p>
<img src="https://raw.githubusercontent.com/shiecldk/pikvm-gpio-atx-multiport/main/images/rpi4b-pins.png" alt="rpi4b-pins" class="center"></p>

Source: [https://linuxhint.com/gpio-pinout-raspberry-pi/](https://linuxhint.com/gpio-pinout-raspberry-pi/)

## Limitation
Due to the limitation with number of the available GPIO ports on Pi-KVM V2, HDD Leds are not available on the Web GUI. Following the Pin Mapping section, if you find out additional GPIO pins that can be used for PC2 - PC4, feel free to let me know.

## Credits
* The [Pi-KVM project](https://github.com/pikvm/pikvm).
* zappanaut's [pikvm-usb-atx-ctrl repo](https://github.com/zappanaut/pikvm-usb-atx-ctrl) that inspired this project.