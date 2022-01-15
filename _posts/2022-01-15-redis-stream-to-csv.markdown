---
layout: post
title:  "Converting Redis Streams output to CSV"
date:   2022-01-15 12:00:00 +0100
category: redis
tags: redis programming shell
---

To convert sensor readings in a Redis Streams into CSV.

First, let's define some handy variables:

```sh
FROM_TS=1642161986000
TO_TS=1642248369000

REDIS_URL=<redis-url>
STREAM=<stream-name>
```

Let's start with a single data point.
Each sensor reading has the same structure, in example:

```sh
$ redis-cli --no-raw -h $REDIS_URL xrange $STREAM - + count 1

1) 1) "1639676389878-0"
    2)  1) "sensorid"
        2) "4119"
        3) "value"
        4) "18.7"
        5) "ts"
        6) "2021-12-16T17:38:52.350Z"
        7) "hub-type"
        8) "develco"
        9) "sensor-type"
       10) "Motion Sensor"
       11) "hub-id"
       12) "549"
       13) "location"
       14) "Bathroom/Wall/Motion"
       15) "event-type"
       16) "temperature_average"
       17) "latency-seconds"
       18) "57"
```

Let's print them in "raw" (flattened) format:

```sh
$ redis-cli --raw -h $REDIS_URL xrange $STREAM $FROM_TS $TO_TS count 1 2>&1

1639676389878-0
sensorid
4119
value
18.7
ts
2021-12-16T17:38:52.350Z
hub-type
develco
sensor-type
Motion Sensor Mini
hub-id
549
location
Bathroom/Wall/Motion
event-type
temperature_average
latency-seconds
57
```

We can remove the first line (it's the Redis Streams timestamp) and then will join every second row
to the preceding one, in a key-value pair fashion:

```sh
$ redis-cli --raw -h $REDIS_URL xrange $STREAM $FROM_TS $TO_TS count 1 2>&1 | egrep -v '[0-9]{13}-' | paste -d= - -

sensorid=4119
value=18.7
ts=2021-12-16T17:38:52.350Z
hub-type=develco
sensor-type=Motion Sensor Mini
hub-id=549
location=Bathroom/Wall/Motion
event-type=temperature_average
latency-seconds=57
```

Now we just need to join those lines with a comma:

```sh
$ redis-cli --raw -h $REDIS_URL xrange $STREAM $FROM_TS $TO_TS count 1 2>&1 | egrep -v '[0-9]{13}-' | paste -d= - - | paste -d, - - - - - - - - -

sensorid=4119,value=18.7,ts=2021-12-16T17:38:52.350Z,hub-type=develco,sensor-type=Motion Sensor Mini,hub-id=549,location=Bathroom/Wall/Motion,event-type=temperature_average,latency-seconds=57
```

This works for a single datapoint. To apply it to the full time range, just remove the `count 1`
argument.:

```sh
$ redis-cli --raw -h $REDIS_URL xrange $STREAM $FROM_TS $TO_TS 2>&1 | egrep -v '[0-9]{13}-' | paste -d= - - | paste -d, - - - - - - - - - | tee events.csv

sensorid=4119,value=18.7,ts=2021-12-16T17:38:52.350Z,hub-type=develco,sensor-type=Motion Sensor Mini,hub-id=549,location=Bathroom/Wall/Motion,event-type=temperature_average,latency-seconds=57
sensorid=4323,value=23.0,ts=2021-12-16T17:38:52.669Z,hub-type=develco,sensor-type=Vibration Sensor,hub-id=685,location=Bedroom/Bed/Vibration,event-type=temperature_average,latency-seconds=57
sensorid=3980,value=20.8,ts=2021-12-16T17:39:02.880Z,hub-type=develco,sensor-type=Window Sensor,hub-id=640,location=Kitchen/Fridge/Door,event-type=temperature_average,latency-seconds=47
sensorid=3748,value=1,ts=2021-12-16T17:39:04.974846Z,hub-type=develco,sensor-type=Motion Sensor Mini,hub-id=569,location=Hall/Wall/Motion,event-type=motion_triggered,latency-seconds=45
...
```
The output file can now be sorted (in this case, to group events by `sensorid`), or you can extract
the keys into a "headers" first row, or... whatever you want.

Thank you shell!

