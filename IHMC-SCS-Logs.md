### Downloading Logs
* Use Webdavs to connect to IHMC file server:
  * Click on the gear icon in Nautilus -> 'Connect to Server'
  * davs://samson.ihmc.us:8081/LogData
  * user: edinburgh
  * password: ask Maurice

### Running LogVisualizer
To make the Log Visualizer run in SCS from Eclipse, you need to increase Java memory
* Run -> Run Configurations
* In VM Arguments add: -Xmx8192m
* Select the log file and the LogVisualizer will launch playback

### Example File Size of IHMC Logs
Atlas example:
- 2015-00-00-scs-30s_drill_circles_cropped
- 160 seconds. 10GB in total
  - 4.9 GB of data (robotData.bsz). 30MB/sec
  - 4.4 GB of movies of about 1GB: UI video and 3 videos from cameras

Valkyrie example:
- 2014-12-20-Schebang
- 143 seconds. 4.4GB in total
  - 2.6 GB of data (robotData.bsz). 18MB/sec
  - 1.8 GB of movies of about 1GB: 2 videos from cameras

Valkrie LCM log example:
- Typical walking log from NASA training: 4.5 MB/sec
- Of which 2 MB/sec is from CAMERA at 4Hz

### Playing SCS Logs to LCM (Work in progress)
Modifications to LogVisualizer.java to transmit LCM traffic
* I could modify it to transmit the test message from the LCM tutorial

First add jars to project:
* Project -> properties
* Java Build Path -> Libraries
* 'Add External JARs' -> added examples_types.jar and lcm.jar

At top of LogVisualizer.java add:

```
    import java.io.*;
    import lcm.lcm.*;
```

in main() add:

```
LCM lcm = LCM.getSingleton();

exlcm.example_t msg = new exlcm.example_t();
msg.timestamp = System.nanoTime();
msg.position = new double[] { 1, 2, 3 };
msg.orientation = new double[] { 1, 0, 0, 0 };
msg.ranges = new short[] {
    0, 1, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14
};
msg.num_ranges = msg.ranges.length;
msg.name = "example string";
msg.enabled = true;

lcm.publish("EXAMPLE", msg);
```

... this transmits the EXAMPLE message which you can see with the bot-spy