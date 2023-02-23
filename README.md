# pikvm-gpio-atx-multiport
This is a Pi-KVM GPIO implementation for ATX multiport support up to controlling 4 PC ports with the [XH-HK4401 4-port HDMI USB KVM Switch](https://docs.pikvm.org/xh_hk4401/) and a custom breakoutboard for ATX switch. The project is based on Pi-KVM V2. If you are using Pi-KVM V3, you would need to make adaptations for GPIO pins based on the Pin Mapping and Reference sections.

The implementation is accomplished through adding the addtional GPIO pins to the Pi-KVM Web GUI under /etc/kvmd/override.yaml.

<p>
<img src="https://raw.githubusercontent.com/shiecldk/pikvm-gpio-atx-multiport/main/images/kvm-atx-menu.png" alt="kvm-atx-menu" class="center"></p>

## Hardware Requirements
Make sure you have the 3 hardwares to make the KVM switch work:

1. Pi-KVM V2 with RPi 4B model (Pi-KVM V3 requires adaptations)
2. A HDMI USB KVM Switch
3. A Custom ATX Switch

## Installation
### HDMI USB KVM Switch
Follow the guide to setup the hardware for HDMI USB KVM Switch. In this project's instance, the [XH-HK4401 KVM Switch](https://docs.pikvm.org/xh_hk4401/) is used.

### kvdm files
Set the Web GUI for ATX Switch using the following guide:

In the Pi-KVM terminal:<br>
Obtain the root permission:<br>
`su -`<br>

Set to read-write mode:<br>
`rw`<br>

Go to the /etc/kvmd directory:<br>
`cd /etc/kvmd`<br>

Make backup of override.yaml and web.css:<br>
`cp override.yaml override.yaml.bak`<br>
`cp web.css web.css.bak`<br>

Copy override.yaml and web.css in this repo to your Pi-KVM OS, e.g., through SFTP or through `nano` to edit the files:<br>
`nano override.yaml`<br>
`nano web.css`<br>

Set the OS back to read-only mode:<br>
`ro`<br>

Restart the kvmd service:<br>
`systemctl restart kvmd`<br>

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
PC3 | GPIO #16 | GPIO #19 | GPIO #13
PC4 | GPIO #20 | GPIO #26 | GPIO #21

## Reference
### Raspberry Pi 4 Pins
<p>
<img src="https://raw.githubusercontent.com/shiecldk/pikvm-gpio-atx-multiport/main/images/rpi4b-pins.png" alt="rpi4b-pins" class="center"></p>

Source: [https://linuxhint.com/gpio-pinout-raspberry-pi/](https://linuxhint.com/gpio-pinout-raspberry-pi/)

### GPIO Pinout in Pi-KVM V3 
This reference is derived from the [Pi-KVM V3 doc](https://docs.pikvm.org/v3/#io-ports-and-jumpers). However, this project is based on Pi-KVM V2; only take this as a reference to check the available GPIO pins and disable some pikvm service if needed.

<details class="note">
<summary>GPIO pinout</summary>
<div class="admonition warning">
<p class="admonition-title">Before proceeding, make sure that the mb you are using has normal ATX headers</p>
</div>
<ul>
<li><strong>ATX control</strong></li>
<li><code>power led = GPIO 24</code> - Used for reading the host power state.</li>
<li><code>hdd led = 22</code> - Same for the HDD activity.</li>
<li><code>power switch = 23</code> - Used for pressing the power button of the host.</li>
<li><code>reset switch = 27</code> - Same for the reset button.</li>
</ul>
<p>These pins can't be used for any other purposes even if ATX function is disabled.</p>
<ul>
<li>
<p><strong>I2C bus</strong> - <code>GPIO 2, 3</code> - Can be used as I2C ONLY (OLED/RTC).</p>
</li>
<li>
<p><strong>1-Wire [19]</strong> - <code>GPIO 4</code> - Also available under ATX RJ-45 port (point [19] on the above) as bi-directional buffered open-drain 5V for regular 1-Wire usage.</p>
</li>
<li>
<p><strong>UART</strong> - <code>GPIO 14, 15</code> - Can be used as UART only for the serial console. When jumpers [5] are removed, you can connect to pins 14 and 15 directly using GPIO header. Also you can remove jumper [5] and disable UART console in the <code>/boot/config.txt</code> and <code>/boot/cmdline.txt</code> to use this pins for any purpose. But it's not recommended.</p>
</li>
<li>
<p><strong>Red activity led on the front [8]</strong> - <code>GPIO 13</code> - Can be disabled in <code>/boot/config.txt</code> and available on the Neo-pixel port [19].</p>
</li>
<li>
<p><strong>PWM fan controller</strong> - <code>GPIO 12</code>. Can be used for custom purposes if the fan disconnected and <code>kvmd-fan</code> service is stopped.</p>
</li>
<li>
<p><strong>I2S HDMI sound</strong> - <code>GPIO 18, 19, 20, 21</code>. Can be used for custom purposes if the <code>tc358743-audio</code> overlay in <code>/boot/config.txt</code> is disabled <strong>AND</strong> jumpers [4] are removed.</p>
</li>
<li>
<p><strong>USB breaker</strong> - <code>GPIO 5</code> - Can't be used for any other purposes.</p>
</li>
</ul>
</details>

## Limitation
Due to the limitation with number of the available GPIO ports on Pi-KVM V2, HDD Leds are not available on the Web GUI. Following the Pin Mapping section, if you find out additional GPIO pins that can be used for PC2-PC4, feel free to let me know.

## Credits
* The [Pi-KVM](https://github.com/pikvm/pikvm) project.
* zappanaut's [pikvm-usb-atx-ctrl](https://github.com/zappanaut/pikvm-usb-atx-ctrl) repo that inspired this project.
