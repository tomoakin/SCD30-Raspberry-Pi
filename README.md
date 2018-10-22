# SCD30-Raspberry-Pi
Python library for Raspberry pi and the SCD30 sensor.
More information about the sensor : https://www.sensirion.com/en/environmental-sensors/carbon-dioxide-sensors-co2/

# Hardware Bug on RPI
While SCD30 requires clock stretching for I2C bus communication
(this is written in bold face in the Interface Description by Sensirion),
the standard i2c port of raspberry pi has a hardware bug 
in clock stretching.  
See https://github.com/raspberrypi/linux/issues/254

Use i2c-gpio overlay instead of the Raspberry pi Hardware implementation of i2c.
## Hardware setup
*Pull-up GPIO pins #23 and #24 to 3V3 (3.3 V) with 3 kohm resister.
*Connect #23 to SDA and #24 to SCL.
## Software setup
*Add a line in /boot/config.txt as follows
```
dtoverlay=i2c-gpio,i2c_gpio_sda=23,i2c_gpio_scl=24,i2c_gpio_delay_us=2
```
See https://raspberrypi.stackexchange.com/questions/37796/how-to-use-i2c-gpio-with-raspberry-pi

*Reboot

*Confirm to detect the SCD30 at 0x61 in the i2c-3 bus with `i2cdetect -y 3`
```
pi@raspberrypi:~ $ i2cdetect -y 3 
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- 61 -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- --                         
```

Some of the function of this library are not fully implemented or buggy.

# Licence

GNU AGPL v3
