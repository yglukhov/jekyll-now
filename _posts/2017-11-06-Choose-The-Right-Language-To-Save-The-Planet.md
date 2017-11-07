---
layout: post
title: Choose The Right Language To Save The Planet
---

So here's a trendy statement that a lot of people would agree with:

> Programming language performance doesn't matter in most cases because computers
are so powerful the users will barely notice any difference.

And here's another one, even more people should agree with:

> We should do whatever is possible to preserve the resources of Earth to keep it
clean.

So I decided to try and calculate how those two statements violate each other.
For all the impatient here's the code in [Nim](https://nim-lang.org) that
translates your programming language performance to the coal burnt:

```nim
# Inputs
# Number of users that run your code:
let numUsers = 100000
# Average CPU load (0.0 - zero, 1.0 - 100% CPU load):
let averageCPULoad = 0.1
# Average user daily session in minutes:
let numMinutes = 10
# Average CPU power consumption (laptops and desktops are roughly 30 - 100 Watts):
let cpuPowerDrainWatts = 50

let cpuPowerDrainKW = cpuPowerDrainWatts / 1000
let numHoursPerUser = numMinutes / 60

let kwh = cpuPowerDrainKW * (numUsers.float * numHoursPerUser) * averageCPULoad
let coalKg = kwh * 0.125 # 1 kWh is equivalent of 0.125 Kg coal
echo "kwh per day: ", kwh
echo "coal burnt per day (kg): ", coalKg
echo "coal burnt per month (kg): ", coalKg * 30
```

I'm sure you can translate this program to your favorite language and run it in
less than a couple of minutes ;)

Sample usage.
-------------
Let's say we're doing a very cool and popular text editor. Developers love it
so much they keep it open 8 hours a day. Our user base is pretty big, maybe
around 30000, not to mention those who use the editor from time to time. The
editor is really optimized, in idle mode the background tasks consume roughly
1.5% CPU. The numbers are actually taken from a real world example. Lets enter
those figures into the program.

```nim
let numUsers = 30000
let numMinutes = 8 * 60
let averageCPULoad = 0.015 # 1.5%
let cpuPowerDrainWatts = 50
```
Run the program and see the output:
```
kwh per day: 180.0
coal burnt per day (kg): 22.5
coal burnt per month (kg): 675.0
```

675 Kg per month. Or 1488 lbs.
If there's a programming language that is 20% more efficient, that would
be 135 Kg difference, or 297 lbs per month. Does it matter? Its for you to decide.

References
----------
The numbers were looked up on the internet.
- kWh to coal conversion - [European Nuclear Society](https://www.euronuclear.org/info/encyclopedia/f/fuelcomparison.htm)
- CPU energy consumption - [Wikipedia](https://en.wikipedia.org/wiki/List_of_CPU_power_dissipation_figures)
