# Vicon Motion Capture

A [motion capture system](https://en.wikipedia.org/wiki/Motion_capture) is a set of external sensors (typically cameras) which allow you to capture the position and orientation (pose) of objects in a tracking volume. In the context of robotics, this allows us to conduct control and motion-planning experiments without having worry about perception and state estimation.
The [Vicon system](https://www.vicon.com/) is the motion capture system installed in the Multi-Robotics Lab at CoR, ME. The system is able to achieve submillimeter motion tracking, and it consists of 12 cameras hanging several meters above the ground. The information gathered here are the notes taken during the Vicon-training in November 2023.
**Email ProCare (support):**: rprinsen@procarebv.nl


> [! warning]
> TODO: There is a lot of information low-level info in this section, needs cleanup once the setup is converged.

## Components:
- **Cameras:**: Vicon Vantage V5. There are different cameras in the set-up. The field-of-view (FOV) depends on the type of camera. The cameras with 8.5mm lenses on the long sides of the dome have a shorter focal distance and a wider view. The cameras on the short side of the dome have a 12.5mm lens and therefore a longer focal distance. The cameras are hung clockwise from 1-12. The total set of cameras could reach an accuracy of 0.017mm (tested in Oxford), but the error per camera can be higher than the total accuracy. The latency of the camera is due to onboard processing.
If the camera has flickering lights, the camera is moved and might need to be calibrated. 
- **Switch**:The IP address of the switch is 192.168.10.1. 
- **Wand**: This is a T-shaped device that calibrates the cameras which should be used before any experiments! You can see the battery indication on top and the wand can be charged with a charger. Press the button to turn on "Strope" which gives brighter leds (at 100Hz) which are useful for a big space like our dome.
- **System**: The System Latency is the total system latency of around ~2.8ms.

## Tracking volume
The effective volume of mocap is about 13 (length) by 7.5 (width) by 6 (height) meters.

## Network:

See [this guide](network.md).

## Tracker software:
- The Vicon tracker software has four tabs that give you the most important information: System, Calibration, Objects, Recording.

## Daily usage: Settings in the System tab
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

## Daily usage: Set-up & Calibration before every experiment:
- TODO: ADD EXACT INSTRUCTIONS TO TURN ON THE SYSTEM.
- Turn on the system **at least 45 minutes!!** before doing any calibration or experiments, since the system needs to "heat up" first (as mentioned in the System settings). 
- Every day the system needs to be recalibrated with the **wand** (internal lens characteristics might change, and external factors like temperature and light).
- Step (1): Select the **wand** device for calibration.
- Step (2): In the settings, only having 1000 frames per camera can be low. Recommended is to have 1500 frames. An Automatic Stop is fine. 
- Step (3): Stop the wand. Start the calibration in the tracker software, then walk into the lab, start the Wand and make sure that you cover the complete lab by making continuous swings with the wand at different heights while moving through the lab. Do **NOT** stand in front of one camera and then continue to the next one, but really move through the entire space all the time.
- If the fields are green (in the software/cameras?) that is good.
- Step (4): By default the origin is set to be one camera and then all other cameras are tilted. Set the correct origin by clicking Start, then click Set Origin.
- If desired (in advanced): Make the floor reconstruction waterpass/leveled with the actual floor by letting markers cover the complete space. Click AUTO DETECT, click Start, and click Set.

## Daily usage: Objects:
- Orient your object in the . You have to pause before creating an object. Enter the name of the object, press ALT + drag, and select "to select markers". The track mode will track objects only to reduce latency.
- You can translate the origin of an object by clicking Cube, pressing "t" for shifting coordinate axes, and pressing "r" for rotating axes.
- There is no reachable limit for the number of obstacles that can be added. The distributor showed tracking of 100 different objects.
- use the [vicon_bridge](https://github.com/tud-amr/vicon_bridge) to get pose readings into ROS.
    - consider using a `robot_localization_ekf` to augment these pose estimates with velocity estimates (e.g. for MPC)

## Saving recording:
- In Live mode (press space to put it in Paused mode) it is always streaming, but you can also record it.
- The system saves camera data, plus object data. You can save it to a CSV-file.
- You can replay the recording on your own laptop if it has a Tracker Licence (which might not be that easy to obtain for everyone.)
- Next to Tabs, the view type can be saved (with different perspectives).

## Storing information:
- VST files to reproduce: system.vst and object(s).vsk, view types and options.
- File storage locations:
  - Public: C:\Users\Public\Documents\Vicon\Tracker3.x\{Configurations\Systems, Configurations\ViewTypes, Configurations\Options, Objects}
  -  Private: C:\Users\<UserName>\AppData\Roaming\Vicon\Tracker3.x\{Configurations\Systems, Configurations\ViewTypes, Configurations\Options, Objects}

## Connection and set-up of the cameras (once or when needed):
- The switch: the IP address of the switch is 192.168.10.1. You can connect a new Vicon camera to every possible port on the switch and it will automatically get a new IP address.
- To test the coverage of the lab area with all the cameras, place markers at the edges of the lab. Test the height coverage by putting the **wand** on a long stick, also don't forget to calibrate the corners.
- If the coverage at the corners is really bad, then you need to plan an appointment with Vicon to adjust the camera settings. 
- Cameras can be adjusted and afterward recalibrated, in case of lighting or etc. 

## New versions of the software:
Additional features are available in Vicon Tracker 4 (compatible with our cameras):
- Showing live meshes over marker objects.
- Indication how much a camera contributes to the tracking.
- More information on the following website: https://docs.vicon.com/display/Tracker40/Vicon+Tracker+4+Migration+Guide.
