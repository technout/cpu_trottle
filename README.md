# cpu_trottle.py
Trottles CPU when getting hot - Python3 cli tool which can be run as a daemon systemd service.

Script for throttling system CPU frequency based on a desired maximum temperature (celsius).

CPU Trottle is released under the terms of the GNU GPLv3 License.

Set a maximum temperature for your system using this tool. If the maximum temperature is exceeded, the script will limit the speed of all your CPU cores until the system is again below your desired maximum temperature. (If your system remains above maximum temperature after completely limiting your CPU cores, it will simply stay limited until temperatures drop below the maximum desired.)

```
Usage: cpu_trottle.py [-h] [--time TIME] [--crit_temp CRIT_TEMP] [--debug]

optional arguments:
  -h, --help              show this help message and exit
  --time TIME             Seconds to cooldown cpu before next check, default is 30 seconds.
  --crit_temp CRIT_TEMP   Temp for cpu to trottle down (temperature in celcius degrees), default is 64.
  --debug                 Output more information when set to True.

Example: sudo cpu_trottle.py --time=15 --crit_temp=45 --debug

This will keep cpu temp below 45 degrees celcius (most of the time) and let ik cool down at a lower speed for 15 seconds. Also it prints all debug messages to screen (and to logfile).
```

Works on modern AMD and Intel CPU's with Linux Kernel 5.4 and newer.

Required packages
-----------------
This script requires package: cpufrequtils
Install command for Ubuntu based distro's: `sudo apt install cpufrequtils`

Python version 3.7 or higher

How to stress test the CPU
--------------------------
Stress testing cpu with command:
`stress -c 4 -t 120s`
With c the number of threads and t time in seconds to stress test.

Install helpful Gnome extensions
--------------------------------
You can install Gnome extensions on https://extensions.gnome.org/
Very helpful tools are: cpufreq and Freon for temperature and cpu speed measurements

Make this script run as a systemd service
----------------------------------------
Instructions for running the script as a systemd service daemon:
```
Step 1.
sudo nano /etc/systemd/system/cpu_trottle.service

[Unit]
Description=Trottles cpu speed at defined temperature
After=multi-user.target

[Service]
Type=simple
Restart=always
ExecStart=/usr/bin/python3 /home/<username>/cpu_trottle.py

[Install]
WantedBy=multi-user.target

Step 2.
sudo systemctl daemon-reload

Step 3.
sudo systemctl enable cpu_trottle.service
sudo systemctl start cpu_trottle.service
```
