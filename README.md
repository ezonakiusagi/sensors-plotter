# DESCRIPTION
sensor-plotter is a tiny ksh93 script to simplify visualizing sensor data in a terminal / CLI environment.

# REQUIREMENTS
i wrote sensor-plotter on a Linux system with ksh93, so it requires the following to run:

- ksh93
- sensors (usually part of "lm_sensors" package)
- gnuplot

along with other standard Unix utilities like: rm, grep, awk, tail , head, sort, mktemp. make sure these are all installed first. check they are in your PATH by running 'which' followed by the program name.

# HOW TO USE
pick a sensor you want to visualize from the listing shown by 'sensors -u' command. here's a snippet of such output:
```
$ sensors -u
coretemp-isa-0000
Adapter: ISA adapter
Package id 0:
  temp1_input: 38.000
  temp1_max: 87.000
  temp1_crit: 105.000
  temp1_crit_alarm: 0.000
Core 0:
  temp2_input: 34.000
  temp2_max: 87.000
  temp2_crit: 105.000
  temp2_crit_alarm: 0.000
Core 1:
  temp3_input: 29.000
  temp3_max: 87.000
  temp3_crit: 105.000
  temp3_crit_alarm: 0.000
(remainder of output cut off)
```

In the above, we can see "coretemp-isa-0000" as a chip that has sensors, and there are several sensors listed, for example "temp1_input" is the package temperature sensor. If we want to visualize this sensor, run sensor-plotter with the following options:

$ ./sensor-plotter --chip=coretemp-isa-0000 --sensor=temp1_input

You should then see a plot of the sensor data in the terminal in ASCII art form. You can pick any "chip" and "sensor" to visualize, it can be a temperature sensor, fan speed sensor, voltage sensor, etc., but only 1 chip and 1 sensor at a time.

# OTHER OPTIONS
