# Video Camera System
The purpose of this tutorial is to provide information about the video camera system (VCS) and how to use. It consists of the following subsections:

- [What is the VCS?](#what-is-the-vcs)
- [How to set up?](#how-to-set-up)
- [How to record a video?](#how-to-record-a-video)
- [How to save a recorded video?](#how-to-save-a-recorded-video)
- [How to adjust camera settings?](#how-to-adjust-camera-settings)
- [Troubleshooting](#troubleshooting)

> In case you have any questions, please first consult the corresponding user manual, then contact Dennis Benders (mailto:d.benders@tudelft.nl).


## What is the VCS?
The VCS is a video camera system that makes it easy to display video streams and record advanced experiment videos by simply pressing one button. It consists of the following main components:
- 5 [Panasonic DC-BGH1E](https://www.panasonic.com/nl/consumer/cameras-camcorder/lumix-box-camera/dc-bgh1.html) cameras. The IDs, names, location and lens type for each of the cameras are given in the table below.
- 1 [Netgear GSM4212UX AV-line M4250](https://www.netgear.com/business/wired/switches/fully-managed/gsm4212ux/) network switch.
- 1 [Blackmagic ATEM SDI Extreme ISO](https://www.blackmagicdesign.com/products/atemsdi/techspecs/W-APS-36) video mixer.
- 1 [Samsung T7 Portable 2TB](https://www.samsung.com/nl/memory-storage/portable-ssd/portable-ssd-t7-2tb-black-mu-pc2t0k-ww/) SSD.
- 1 [LG 43UD79 43 inch UHD](https://www.lg.com/nl/monitor/lg-43UD79-B) monitor.
- 2 [Benro A1573FS2PRO](https://www.benro.nl/ida-Video_VideoKits_VideoKits_A1573FS2PRO_Video-Statief-Kit) tripods.

| ID | Name | Location | Lens type |
| - | - | - | - |
| 1 | BGH1-7698D6 | Top-left | Olympus EZM1240 Zoom Zuiko Pro 12-40mm F/2.8 PRO II |
| 2 | BGH1-778C5F | Top-center | Olympus M.Zuiko Digital 7-14mm F/2.8 PRO |
| 3 | BGH1-773113 | Top-right | Olympus EZM1240 Zoom Zuiko Pro 12-40mm F/2.8 PRO II |
| 4 | BGH1-378B1D | Separate | Olympus EZM1240 Zoom Zuiko Pro 12-40mm F/2.8 PRO II |
| 5 | BGH1-71C506 | Separate | Olympus EZM1240 Zoom Zuiko Pro 12-40mm F/2.8 PRO II |

The top-left camera is the one closest to the lab workplaces, the top-center camera is the top-down camera at the center of the ceiling, and the top-right camera is the one furthest away from the lab workplaces.

You can find the separate cameras, tripods and SDI and UTP cables in the AMR2 cabinet next to the lab. There is one SDI-UTP cable set already laid down around the lab, so you can easily record a video using a camera at the opposite side of the lab with respect to the workplaces.

The cameras are connected to the network switch, and the switch to the MRL desktop, via UTP cables. These cables:
- provide the cameras with power using Power over Ethernet (PoE);
- allow to adjust camera settings via the MRL desktop (see [How to adjust camera settings?](#how-to-adjust-camera-settings));
- allow to download on-camera recorded videos (for example, videos recorded at 240 FPS).

Next to UTP cables, the cameras have SDI cables attached to them. These cables carry the data of the live video streams and should be connected to the video mixer (see [How to set up?](#how-to-set-up)).

The video mixer is capable of recording all streams simultaneously on an external SSD and displaying them in real-time on a monitor. This is what we need the SSD and the LG monitor for. Without the SSD connected, the mixer won't be able to record a video.


## How to set up?
Setting up the system is very simple:
1. Get the box of the video mixer out of the AMR2 cabinet. This box should include the SSD.
2. Connect all cables to the video mixer (see the [manual of the video mixer](https://documents.blackmagicdesign.com/UserManuals/ATEM_SDI_Manual.pdf?_v=1719298810000) for more information):
   - Connect the desired camera SDI cables to the first input ports.
   - Connect the LG SDI cable to the first output port.
   - Connect the USB-C cable of the SSD to the USB A port.
   - Connect the power cable to the corresponding port.
3. Turn on the power of:
   - the network switch;
   - the LG monitor;
   - the video mixer.
4. Turn on the LG monitor by pressing the button in the middle below the screen.

You should now be able to see one (or multiple) camera screens on the monitor. If not, maybe the [troubleshooting](#troubleshooting) section has something to offer for you ;)


## How to record a video?
To record a video, just press the `REC` button in the top-right corner of the video mixer (`RECORD` section). It is recommended to display the multi-view on the monitor by pressing the `M/V` button in the bottom-right part of the mixer (`VIDEO OUT` section). This way, you can easily see what is recorded and if it is actually recorded (would be a pitty if your experiment was successful, but you accidentally didn't record it :no_good:). Maybe not so surprisingly: to stop the video press `STOP`, just below `REC`.

> [!important]
> You can only record a video when the SSD connected to the video mixer. If properly connected, the `DISK` light, just above `REC`, will turn green.

The video mixer records all camera streams separately, plus the *program* view. This is the top-right view in multi-view mode. While recording a video you can place camera streams in the *preview* view by pressing one of the stream numbers in the bottom-left part of the mixer. After pressing `CUT` in the bottom-right part of the mixer, the *program* and *preview* are interchanged. This allows you to create a recording that switches between video streams while performing the experiment. Isn't that exciting? :smiley:

In practice, though, it is probably more useful to record the separate streams and edit them afterwards in the free and cross-platform video editing software of Blackmagic: [DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve).

You can also add videos or pictures in the *program* view (`PICTURE-IN-PICTURE` section), add program change effects (`EFFECTS` section), or add macros that automatically perform a series of mixer actions (`MACRO` section), but this is a bit advanced for this tutorial. For more information, please check the [video mixer manual](https://documents.blackmagicdesign.com/UserManuals/ATEM_SDI_Manual.pdf?_v=1719298810000).

Of course, you don't have to use the multi-view mode. To show individual views of the multi-view mode, press either a number, `CLEAN`, `PVW` or `PGM` in the right panel on the mixer (`VIDEO OUT` section).

Finally, press `FTB` (below the `VIDEO OUT` section) if you want to fade out the *program* stream. Press `FTB` again if you want to fade the selected stream in again.


## How to save a recorded video?
After pressing `STOP` the videos streams are automatically saved on the SSD. You can unplug the SSD without turning off the mixer and plug it into your own device. The files will be stored in a directory called `TEST #` (sorry, this name was configured when setting up and never changed afterwards :sweat_smile:). A new `TEST #` directory is created for every set of videos that was recorded in a limited time interval.

In each `TEST #` directory, you can find `Audio Source Files` (not used), `Video ISO Files` (containing the separate video streams), a `.drp` file and a `.mp4` file. The `.mp4` file contains the *program* view recording.

You can copy the desired files to your own device.

> Please only remove your own videos from the SSD, not those of others. In case of doubt, just leave the video on the SSD. The SSD has sufficient storage capacity, so it is not wise to risk removing a video that might turn out to be crucial later ...


## How to adjust camera settings?
The camera settings can be adjusted using a program called *LUMIX Tether*. This program is installed on the MRL desktop. When launching the program, it will automatically search for all cameras in the network. This takes a little bit of time. The cameras that are found will be listed in the *CAMERA* window. If this window does not open automatically, click on the *CAMERA* button in the top of the window. The names correspond to the ones listed in the table at the top of this tutorial.

> It might happen that not all cameras are listed. In that case, just click *Update* in the *CAMERA* window, or close and open the program again.

To go to the settings of a specific camera, click on the name of the camera. This will open the *Recording Panel*. You can use this panel to record an on-camera video with specific recording settings. This, however, goes beyond the scope of this tutorial, so please check the [program manual](https://eww.pavc.panasonic.co.jp/dscoi/lumixtether/OperationGuide(en).pdf) if you want to do this. It might be helpful if you want to record a video in 240 FPS.

To go further to the camera settings, click on the `LV` (Live View) button in the top-left corner of the *Recording Panel*. This will open a new window. In this window, click on `MENU` (in the top-right corner). Now, you are able to adjust anything you want. Please check the [camera manual](https://eww.pavc.panasonic.co.jp/dscoi/DC-BGH1/E/DC-BGH1_DVQP2279_eng/index.htm) for more information.

> [!important]
> Make sure to write down all settings changes that you do. Revert those changes at the end of the your lab session, so the next lab user can use the system out of the box.


## Troubleshooting
**Why do I see a level bar and the camera information in my video stream? Is this also recorded?**

If you see the camera information being displayed on the LG monitor, then it means this information is part of the SDI stream and will thus be recorded if you press `REC`. We have already experienced that one of the video streams suddenly includes this information. To solve the problem, go to the camera settings (see [How to adjust camera settings?](#how-to-adjust-camera-settings)), then Setup menu, then IN/OUT, then `Info Display` (see the [list of menus in the manual](https://eww.pavc.panasonic.co.jp/dscoi/DC-BGH1/E/DC-BGH1_DVQP2279_eng/chapter12_01.htm)). In the appearing menu choose either `HDMI` or `OFF`. After clicking on the desired option, the video stream should be clean again. If you want to see this information, but not record it, you can attach a small, portable monitor (stored in the AMR2 cabinet) to the camera via HDMI and display this information on the HMDI output.
