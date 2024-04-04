# Motion Capture Infrastructure

A [motion capture system](https://en.wikipedia.org/wiki/Motion_capture) is a set of external sensors (typically cameras) which allow you to capture the position and orientation (pose) of objects in a tracking volume. In the context of robotics, this allows us to conduct control and motion-planning experiments without having worry about perception and state estimation.


## List of Systems at TUD

At TUD, we have several such systems. For the external systems, there is some initial "paperwork" to do before you can use it and the software/system used at each of them varies slightly.

**Important note**: Access to the external facility is a favor of Aerospace/DCSC. If you book a lot without showing up, this will negatively affect our relationship with them.

### Mobile Robotics Lab (Vicon)
- **Location:** CoR (34.F- ground floor)
- **Administration**: Javier Alonso-Mora / Thijs Niesten
- **Type/Brand:**:[Vicon](https://www.vicon.com/)
- **Email ProCare (support):**: rprinsen@procarebv.nl
The [Vicon system](https://www.vicon.com/) is the motion capture system installed in the Multi-Robotics Lab at CoR, 3ME. The system is able to achieve submillimeter motion tracking, and it consists of 12 cameras hanging several meters above the ground. The information gathered here are the notes taken during the Vicon-training in November 2023.

> [! warning]
> TODO: There is a lot of information low-level info in this section, needs cleanup once the setup is converged.

#### Components:
- **Cameras:**: There are different cameras in the set-up. The field-of-view (FOV) depends on the type of camera. The cameras with 8.5mm lenses on the long sides of the dome have a shorter focal distance and a wider view. The cameras on the short side of the dome have a 12.5mm lens and therefore a longer focal distance. The cameras are hung clockwise from 1-12. The total set of cameras could reach an accuracy of 0.017mm (tested in Oxford), but the error per camera can be higher than the total accuracy. The latency of the camera is due to onboard processing.
If the camera has flickering lights, the camera is moved and might need to be calibrated. 
- **Switch**:The IP address of the switch is 192.168.10.1. 
- **Wand**: This is a T-shaped device that calibrates the cameras which should be used before any experiments! You can see the battery indication on top and the wand can be charged with a charger. Press the button to turn on "Strope" which gives brighter leds (at 100Hz) which are useful for a big space like our dome.
- **System**: The System Latency is the total system latency of around ~2.8ms.

#### Network:

See [this guide](../infrastructure/network/mobile_robotics_hub/).

#### Tracker software:
- The Vicon tracker software has four tabs that give you the most important information: System, Calibration, Objects, Recording.

#### Daily usage: Settings in the System tab
- Settings can be saved to a configuration file. Private will only be saved to the current user. Our idea: admin account with standard settings, and user accounts to use the system. It can be nice to back up the file somewhere else to easily recover it later by copying it to the C-drive.
 Files are stored in C/users/public/public documents/vicon can be nice to back-up. Program Data is also a folder to take into account.
-  Local Vicon system (this includes configurations for the system):
    - Frame rate (can be checked under actual frame rate). Max rate = 2000Hz (however with subsampling => smaller FOV, might require a firmware update). Full frame: 420Hz. Max rate: 1060Hz (without some pixels).
    - Genlock: sync other hardware
    - Reconstruction: min. cameras for a marker (currently at 2, can be set higher to avoid vibrations in data). Min. object marker separation (currently at 10mm) => make sure for small drones.
- Vicon cameras (this gives the config per camera):
    -  Gain is adjusted (amplification, so markers can be detected at longer distances). Trade-off between amount of reflections (e.g., from truss) and tracking distance.
    -  LEDs/display (camera number)/accelerometer on/off
    -  Temperature sensor. The system should be warmed up before measuring. Warmed-up: ~52-55 degrees. 0:30-0:45 minutes before cameras are warmed-up => **When going to the lab, first start Vicon Tracker and the cameras, then set everything else up.**
- When hovering with your mouse over the cameras it gives info (e.g. "camera has been moved"). Unchecking "Bumped" in the Vicn tracker will stop the camera lights from flickering if you think it is annoying. However, you might want to recalibrate the cameras.
- Environmental drift tolerance: This might be an interesting setting to adapt if you want to track objects that are not as rigid (humans/etc) to allow more tolerance on the movements between the markers.

#### Daily usage: Set-up & Calibration before every experiment:
- TODO: ADD EXACT INSTRUCTIONS TO TURN ON THE SYSTEM.
- Turn on the system **at least 45 minutes!!** before doing any calibration or experiments, since the system needs to "heat up" first (as mentioned in the System settings). 
- Every day the system needs to be recalibrated with the **wand** (internal lens characteristics might change, and external factors like temperature and light).
- Step (1): Select the **wand** device for calibration.
- Step (2): In the settings, only having 1000 frames per camera can be low. Recommended is to have 1500 frames. An Automatic Stop is fine. 
- Step (3): Stop the wand. Start the calibration in the tracker software, then walk into the lab, start the Wand and make sure that you cover the complete lab by making continuous swings with the wand at different heights while moving through the lab. Do **NOT** stand in front of one camera and then continue to the next one, but really move through the entire space all the time.
- If the fields are green (in the software/cameras?) that is good.
- Step (4): By default the origin is set to be one camera and then all other cameras are tilted. Set the correct origin by clicking Start, then click Set Origin.
- If desired (in advanced): Make the floor reconstruction waterpass/leveled with the actual floor by letting markers cover the complete space. Click AUTO DETECT, click Start, and click Set.

#### Daily usage: Objects:
- Orient your object in the . You have to pause before creating an object. Enter the name of the object, press ALT + drag, and select "to select markers". The track mode will track objects only to reduce latency.
- You can translate the origin of an object by clicking Cube, pressing "t" for shifting coordinate axes, and pressing "r" for rotating axes.
- There is no reachable limit for the number of obstacles that can be added. The distributor showed tracking of 100 different objects.
- use the [vicon_bridge](https://github.com/tud-amr/vicon_bridge) to get pose readings into ROS.
    - consider using a `robot_localization_ekf` to augment these pose estimates with velocity estimates (e.g. for MPC)

#### Saving recording:
- In Live mode (press space to put it in Paused mode) it is always streaming, but you can also record it.
- The system saves camera data, plus object data. You can save it to a CSV-file.
- You can replay the recording on your own laptop if it has a Tracker Licence (which might not be that easy to obtain for everyone.)
- Next to Tabs, the view type can be saved (with different perspectives).

#### Storing information:
- VST files to reproduce: system.vst and object(s).vsk, view types and options.
- File storage locations:
  - Public: C:\Users\Public\Documents\Vicon\Tracker3.x\{Configurations\Systems, Configurations\ViewTypes, Configurations\Options, Objects}
  -  Private: C:\Users\<UserName>\AppData\Roaming\Vicon\Tracker3.x\{Configurations\Systems, Configurations\ViewTypes, Configurations\Options, Objects}

#### Connection and set-up of the cameras (once or when needed):
- The switch: the IP address of the switch is 192.168.10.1. You can connect a new Vicon camera to every possible port on the switch and it will automatically get a new IP address.
- To test the coverage of the lab area with all the cameras, place markers at the edges of the lab. Test the height coverage by putting the **wand** on a long stick, also don't forget to calibrate the corners.
- If the coverage at the corners is really bad, then you need to plan an appointment with Vicon to adjust the camera settings. 
- Cameras can be adjusted and afterward recalibrated, in case of lighting or etc. 

#### New versions of the software:
Additional features are available in Vicon Tracker 4 (compatible with our cameras):
- Showing live meshes over marker objects.
- Indication how much a camera contributes to the tracking.
- More information on the following website: https://docs.vicon.com/display/Tracker40/Vicon+Tracker+4+Migration+Guide.

### DCSC OptiTrack

- **Location:** DCSC (34-C-310)
- **Administration**: Wim Wien (w.h.a.wien@tudelft.nl)
- **Type/Brand:**: [OptiTrack](#OptiTrack)
- **Initial Paperwork**
    - Write an email to [André van der Kraan](https://www.tudelft.nl/staff/a.a.m.vanderkraan/) (CoR lab technicians) to ask for access.
    - Complete the safety quiz sent by André on [labservant](https://labservant.tudelft.nl)
    - Upon successful completion of the script, André will notify your supervisor.
    - You can then ask your supervisor to send a request to the Area Supervisor DCSC (Wim Wien, W.H.A.Wien@tudelft.nl) and Office Manager DCSC (Marieke Versloot-Bus, M.Versloot-Bus@tudelft.nl) with the following data:
    ```
    - Name, employee student
    - Mail address
    - Group
    - Supervisor
    - Employee or student number
    - Type of workspace that is needed
    - Time period
    ```
    - Update your employee card at the service desk.
- **Booking the System**:
    - [ClusterMarket](https://app.clustermarket.com)
    - Currently, you are not allowed to book more than 5h per day.

### Aerospace CyberZoo OptiTrack

- **Location** Aerospace
- **Administration**: Guido de Croon
- **Type/Bran**: [OptiTrack](#OptiTrack)
- **Initial Paperwork**
    - Send a photo of your student badge via email to Bertine (b.m.markus@tudelft.nl) with a kind request to access the CyberZoo and a start and end date. Important: add your supervisor (e.g. Javier) and Guido de Croon (g.c.h.e.decroon@tudelft.nl) in CC to give the email an aura of legitimacy.
    - Schedule a meeting for a safety briefing with Erik van der Horst (e.vanderhorst@tudelft.nl)
    - One you have a reply from Bertine, update your employee card at the service desk.
- **Booking the System**
    - Dennis Benders (d.benders@tudelft.nl) and Lasse Peters (l.peters@tudelft.nl) have access to the CyberZoo google calendar to make bookings for the group. Reach out to either of them to make a booking for you.
- **Additional information**
    - Additional information, including recording videos with the IP cameras, can be found on [this README](https://github.com/matteobarbera/mavlab_wiki_fork/blob/master/Cyberzoo.md), created by the people from MAVlab.
- **Recording a video**
    - In case recording on the optitrack computer is not possible, connect your own laptop via the LAN cable and stream the video cameras there. Open VLC media player, Media/Open Network Stream, enter URL             rtsp://192.168.209.168/0 (side view) or rtsp://192.168.209.169/0 (top view), and press play. Record video using e.g. kazam.

### CoR lab OptiTrack
**Note**: Building this lab is in the pipeline, but is not yet finished.

- **Location** CoR
- **Administration** TBD
- **Type/Bran**: TBD
- **Initial Paperwork** TBD
- **Booking the System** TBD

#### System Usage

All of the motion capture systems that we have work tracking objects in the infrared spectrum. For that purpose there are two different modi:

- Passive mode: The cameras emit infrared light and passive (reflective) markers are attached objects that you like to track.
- Active mode: The cameras don't emit any infrared light; instead, active (led) markers are attached to the objects that you would like to track.

The passive mode is the default and almost always what you want. If, however, your robot is very reflective in the infrared spectrum or one of your other sensors is blinded by the infrared light emitted by the cameras (e.g. infrared camera such as Intel RealSense), then active mode is your friend.

In any case, since this kind of sensor relies on infrared light you need to minimize the pollution of this spectrum through external light sources. Therefore, close the blinds if you have trouble with very noisy measurements.

### OptiTrack 

A complete documentation for the OptiTrack system can be found [here](https://v22.wiki.optitrack.com/index.php?title=OptiTrack_Documentation_Wiki). Here we just provide a quick summary:

#### TL;DR

- The system sends the motion capture data to the *Motive* software on a *Windows* host system.
- In Motive you can group individual points into *rigid bodies*. For these, Motive computes the 3D pose (position and orientation) automatically in the global reference frame so you don't have to stitch this information together in your software.
- Motive uses a [multicast messages](https://en.wikipedia.org/wiki/Multicast) to publish the rigid body poses using their own (not ROS compatible) message format
- Depending on your application you can either read these messages [], or, if you are using ROS, you can also use the [mocap_optitrack bridge](https://github.com/tud-amr/mocap_optitrack) to translate these to [TF ROS messages](https://www.google.com/search?hl=en&q=ROS%20TF).

#### TROUBLESHOOTING

- In general, if no OptiTrack data is received, it is good practice to first try the following:
    - Restart Motive and relaunch the package that receives the broadcasted OptiTrack data.
    - Ensure that the User ID field in Motive corresponds to the user ID set in the parameter file of the [mocap_optitrack](https://github.com/tud-amr/mocap_optitrack) package (e.g., [line 6 in mocap.yaml](https://github.com/tud-amr/mocap_optitrack/blob/bca2d9fffddde67552caf0554a12bbeca1967d9e/config/mocap.yaml#L6) should correspond with user ID equal to 1 in Motive).
    - Make sure that all other Motive settings are correct according to [this manual](https://github.com/tud-amr/mocap_optitrack/blob/dcsc_pose_stamped/README.md).
- If your laptop cannot connect to the switch via which the OptiTrack system is streaming data, change the ethernet settings of the desktop running the Motive software according to the settings in the following figure:

![Ethernet settings](./pictures/Motive_computer_ethernet_settings.png)

(it might be a good idea to first try the first option only, but this is not tested in the lab)
- If the cameras are not turned on (they don't light up in blue) restart the optitrack computer.

[TODO] Let's extend this section over time. Until that has happened: ask someone who has worked with this system in the past (Luzia, Dennis, Lorenzo, Lasse,...)
