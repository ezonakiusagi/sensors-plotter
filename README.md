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
```
$ ./sensor-plotter --chip=coretemp-isa-0000 --sensor=temp1_input
```
You should then see a plot of the sensor data in the terminal in ASCII art form. You can pick any "chip" and "sensor" to visualize, it can be a temperature sensor, fan speed sensor, voltage sensor, etc., but only 1 chip and 1 sensor at a time. The output will look like this:

```
     +---------------+---------------+----------------+---------------+---------------+---------------+   
  70 +-+             +               +                +               +               +             +-+   
     |                                                                                                |   
     |                                                                                                |   
     |                                                                                                |   
     |                                                                                                |   
  60 +-+                                                                                    ***     +-+   
     |                                                                                     *  *       |   
     |                                                                                    *   *       |   
     |                                                                                    *   *       |   
  50 +-+                                                                                  *   *     +-+   
     |                  *                                                                 *    *      |   
     |                  *                                                                 *    *      |   
     |                  *                                                                *     *      |   
     |                  **                                               * *             *     *      |   
  40 +-+                **                                               ***             *     **   +-+   
     ********* *********************************** ***** ******* **** *** ****************       *****|   
     |        *                 *            *    *     *       * *  *         *       *              |   
     |                                                                                                |   
     |                                                                                                |   
  30 +-+                                                                                            +-+   
     |                                                                                                |   
     |                                                                                                |   
     |                                                                                                |   
     |                                                                                                |   
  20 +-+                                                                                            +-+   
     |                                                                                                |   
     |                                                                                                |   
     |                                                                                                |   
  10 +-+                                                                                            +-+   
     |                                                                                                |   
     |                                                                                                |   
     |                                                                                                |   
     +               +               +                +               +               +               +   
   0 +-+-------------+---------------+----------------+---------------+---------------+-------------+-+   
     0               20              40               60              80             100             120  
```

# OTHER OPTIONS
There are a number of other options, which can be seen by running with the --help option:
```
$ ./sensors-plotter --help
Usage: ./sensors-plotter [ options ]
OPTIONS
  -c, --chip=sensor chip
                  Select the sensor chip to probe (required).
  -s, --sensor=sensor
                  Select the sensor to probe (required).
  -w, --width=width
                  The number of seconds of the width of plot (default=120).
  -t, --title=title
                  The title of the plot.
  -S, --stats     turn on statistics at bottom of screen.
  -C, --continue=file
                  continue from previous data stream file (default is to start new data stream).
```

### width: 
by default, the time window for visualization is 120 seconds of data. if there is less than 120 seconds of data available, it will visualize as many data points as is available. if there are more than 120 seconds of data, it will visualize the last 120 seconds. This time window can be adjusted with the --width= option by providing the desired time window when launching the program.

### title:
if you are running more than 1 instance of this script in multiple terminal windows and you want to distinguish them from each other, you can add a "title" to each visualization plot with the --title= option. the title will then be shown at the top of each plot.

### statistics:
if you would like to see some basic statistics of the plot, use the --stats option to show the "last", "maximum", "minimum", and "average" value at the bottom of the plot.

### continue:
by default, every run of this script will start a new data stream and collect data from the selected sensor. if instead, you would like to simply continue from a previous data stream and just add newly collected data, use the --continue=file option, where "file" is the path to the previous data stream file. when you terminate the program with ctrl-c, it will print the path to the data stream file used. you can use this file path on the next run with --continue to continue your visualization without starting over.
