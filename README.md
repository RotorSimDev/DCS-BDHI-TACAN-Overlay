# DCS-BDHI-TACAN-Overlay
A gauge overlay that can be used to simulate a TACAN station on any vessel in a DCS mission.

Background
----------

**TACAN** (Tactical Air Navigation) is a military radio navigation system that provides aircraft with bearing and distance information relative to a ground-based or shipboard navigation station. It allows pilots to determine both the direction to a station and their distance from it without relying on GPS or visual references.

TACAN operates in the UHF frequency band and allows for 252 unique station identifiers: 126 "X" channels and 126 "Y" channels.  It provides two primary types of navigation information:

1. Bearing: The magnetic direction from the aircraft to the TACAN station, expressed in degrees. This allows the pilot to determine the aircraft's position relative to the station and navigate toward or away from it.

2. Distance: The slant-range distance between the aircraft and the station, displayed in nautical miles (NM). Unlike horizontal ground distance, slant range includes the aircraft's altitude relative to the station.

In aircraft equipped with a Bearing Distance Heading Indicator (**BDHI**), TACAN information is presented on a compass-style instrument. The compass card rotates to indicate the aircraft's magnetic heading, while a bearing pointer indicates the direction to the selected TACAN station. A separate digital readout displays the distance to that station.

For naval aviation, TACAN is particularly valuable because ships equipped with TACAN transmitters provide a continuously updated navigation reference, allowing helicopters and fixed-wing aircraft to locate and return to their ships even when operating beyond visual range.

BDHI Overlay for DCS World
--------------------------
<img width="657" height="382" alt="gauge" src="https://github.com/user-attachments/assets/bdbab764-8b2d-463d-9bdf-cce1dafa7655" />


The BDHI Overlay recreates this navigation capability for aircraft and ships in DCS World that don't have TACAN functionality.

By calculating the aircraft's position relative to a designated navigation reference (i.e. ship), the overlay presents a simulated BDHI with a rotating compass card, bearing pointer, and distance readout. Additionally, the overlay contains a working course deviation indicator (**CDI**) that is adjusted by a rotating CRS knob.  There is also a TACAN control area where you can tune in to different TACAN stations.  This provides pilots with a familiar instrument for navigating to ships or other reference points without relying on the F10 map or external navigation aids.

Up-Front Note
-------------
This overlay is designed to be used by ships and aircraft that don't have TACAN capability.  This BDHI will NOT lock in on the actual TACAN channel that is used by the carrier in the DCS Super Carrier addon.  Nor will aircraft like the F-18 that actually have a TACAN be able to lock in on the "signal" put out by the ships using this overlay - you'll see why in the "How It Works" section below.

Installation
------------
Download and extract the .zip file from this site.  Place the files/folders as described below into your Scripts folder located in your DCS Saved Games folder.  On a Windows machine, this is typically located at C:\Users\<WindowsUsername>\Saved Games\DCS\Scripts:
1. Place the file BDHIOverlayHook.lua into the Hooks folder
2. Place the folder BDHIOVerlay in the Scripts folder.

Your final installation layout should look something like this:

<img width="460" height="190" alt="dir-tree" src="https://github.com/user-attachments/assets/71bbd26e-9e2b-4f42-861f-42548a5ea3ab" />
  
The BDHIOverlay folder contains all of the logic and graphical elements for overlay.

How It Works
------------
When the overlay loads or when the TACAN channel in the TCN CONTROL section is changed, the simulation environment is scanned for a unit named "Naval-*tacan_channel_X*" or "Naval-*tacan_channel_Y*", e.g. Naval-055X or Naval-121Y.  If there is a ship with the unit name whose suffix matches the value in the TCN CONTROL window on the overlay, then the aircraft's position and distance relative to that ship is calculated and displayed on the overlay.   

Instructions
------------
Since the overlay looks for ships with specific unit names containing the desired TACAN channel, you're going to have to open up the the DCS Mission Editor and either create a mission where you apply the appropriate unit name OR you can edit a mission that already exists and just change the unit name of a ship that you'd like to fly to/from.

As an example, we will take the first mission from YoYo's very excellent free Kiowa campaign, Through the Desert Dust (https://files.digitalcombatsimulator.com/en/files/3345522/), and edit the "unit name" of the LHA-class ship you start and end the mission on so that the BDHI Overlay will find a valid TACAN channel.
1. Start DCS World
2. Open the Mission Editor:
    1. If you're presented with two folders, select the one that says "Open"
    2. If you're not presented with two folders, select FILE...OPEN from the menu bar
3. Find the file "Mission 1 - Delivery man.miz" - this is typically located in "DCS World installation\Mods\campaigns\OH-58D - Through the desert dust\" or, if you're a Steam user, "C:\Program Files (x86)\Steam\steamapps\common\DCSWorld\Mods\campaigns\OH-58D - Through the desert dust\"
4. When you open the .miz file, you should see something like this:
<img width="959" height="524" alt="mission-ed0" src="https://github.com/user-attachments/assets/4aee9ebb-f2f6-48f4-89d7-e8a6e80fab57" />

5. Click on the blue icon in the center of the screen - that's the USS Tarawa - it should open the editing panel on the right like this:
<img width="959" height="524" alt="mission-ed" src="https://github.com/user-attachments/assets/7c6b6813-0182-4bdd-802a-f44c1b4f68c7" />

6. In the top right section, you'll see several editable fields.  For this example, we want to assign the TACAN channel 55x to this particular ship.  To do that, change the UNIT NAME field to Naval-055X as shown below:
<img width="579" height="346" alt="mission-ed2" src="https://github.com/user-attachments/assets/388b2399-fddb-4c8c-abab-a8a646cdc450" />

7. In the menu bar, click FILE...SAVE.
8. When you fly the mission in DCS, you'll be able to bring up the BDHI Overlay by pressing CTRL-SHIFT-F12.

Using the BDHI
--------------
<img width="1584" height="677" alt="bdhi-labeled" src="https://github.com/user-attachments/assets/9e24b6b9-1752-4262-ad8f-9f9bf249fbea" />

1. DME window:  DME stands for "distance measuring equipment." This window tells how far the aircraft is from the selected TACAN station in nautical miles.
2. Course window: This number mirrors the position of the course deviation card which is dialed in by the course selector knob.
3. Compass card: The compass card shows the current aircraft heading at the top and is used to reference the TACAN needle bearing and the selected course.
4. TACAN needle (head): Points toward the selected TACAN station, i.e. shows the magnetic bearing to the selected TACAN station.
5. Course deviation card: Set to a desired course by the course selector knob.  The two white dots on either side of the center of the card represent the amount of deviation from the selected course.  Moving outward from the center of the card, each dot represents 5 degrees of deviation.
6. To/From flag: Indicates whether the current course is headed towards or away from the selected TACAN station.
7. TACAN needle (tail): Can be thought of as representing the aircraft's location from the TACAN station.  In the above image, the aircraft would be said to be on the 115 bearing from the station.  This is also referred to as a "radial."  If the ship radios you for your position, a common phrasing might be "I'm on your 115 radial at 11 point 3 DME" or shortened to "I'm on your 115 at 11 point 3."
8. Course deviation indicator: A bar that slides left or right of center commensurate with the number of degrees off course you are.
9. TACAN channel selector: Left or right clicking any of the four numbers will increment or decrement their values, thus changing the currently selected TACAN station.  If there are no units with that channel in the simulation, then a red bar will appear over the DME window indicating no reception on that TACAN channel.
10. Course selector knob: Using the mouse scroll wheel will increment or decrement the desire course accordingly.  The course deviation card will turn and point at the value shown in the course window.

Sample Kiowa Mission
--------------
If you're not interested in editing or creating a mission from scratch, I've included a sample mission in the package's zip file called BeachAssault.miz.  You must have the Polychop OH-58D Kiowa in order to run this right out of the box, otherwise you could edit the mission and change to a different helicopter.  Generally, user-created missions like this are stored in the C:\Users\<WindowsUserName>\Saved Games\DCS\Missions\ directory.

In this mission, you're tasked with cold starting your armed helo on the back of an Oliver Hazard Perry class frigate, attacking a single shore target vehicle about 5 miles away, and then returning to the frigate.  Labels are turned off, so you'll have to use the (very) simple intel provided in the brief.  This means you'll need to tune your TACAN to the correct channel as the default channel is not the one the ship is using.  Then you can use the bearing provided from the ship to get to the search area.  After you successfully engage the target, the TACAN will help you get within visual range of the ship so you can land.

**Here's what this mission might look like...**

After startup, you pull up the BDHI Overlay with CTRL-SHIFT-F12 and you get this:
<img width="905" height="546" alt="init-error" src="https://github.com/user-attachments/assets/94b7b27d-2c0b-4441-ab0d-e07bbcff772f" />
Notice the red bar through the DME window!  What did the brief say?  Oh yeah, 45X was the TACAN channel...

<img width="792" height="511" alt="corrected" src="https://github.com/user-attachments/assets/6df65a0c-2b50-4d8f-bee7-896c2a2340b8" />

Now the red bar is gone, the CDI has deflected and the yellow TACAN needle moved towards mom (i.e. your ship) a little bit.  You're good to go.  

So you take off, dial your course to 120 (because that's what the intel said to do), and start centering up your CDI so you can begin searching the shoreline for the target.
<img width="960" height="540" alt="Screen_261123_195128" src="https://github.com/user-attachments/assets/0bd78902-9b00-4ca4-bc49-05ec8eb57426" />

Hopefully you find your target and engage it before you get too close...
<img width="960" height="540" alt="Screen_261123_195403" src="https://github.com/user-attachments/assets/fb5c7bb0-718e-4bab-b002-9a0533ef9bab" />
Rifle!  Splash one.  Time to RTB.
<img width="960" height="540" alt="Screen_261123_195712" src="https://github.com/user-attachments/assets/62fb24f9-2d5a-42c4-b300-b742864cd52c" />
That little dot is your home...
<img width="960" height="540" alt="Screen_261123_195849" src="https://github.com/user-attachments/assets/0db89337-3695-4fb3-8382-590b534cfcd5" />
Always feels good when you get closer...
<img width="960" height="540" alt="Screen_261123_195916" src="https://github.com/user-attachments/assets/ef0a4733-e046-489c-bd3c-ad7ebb70a356" />
Land now. Down, down, down.
<img width="1920" height="1080" alt="Screen_261123_195950" src="https://github.com/user-attachments/assets/527affae-9e75-49e9-aacd-9238d1ebd79a" />
In the trap, trapped.  Center light.  Ok, not this exact aircraft, but you get the idea.


Download the package here
-------------------------
[![Download Latest Release](https://img.shields.io/badge/Download-Latest%20Release-blue)](../../releases/latest)















    

