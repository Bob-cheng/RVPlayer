# RVPlayer: Robotic Vehicle Forensics by Replaywith What-if Reasoning

## Introduction

**RVPlayer** is a robotic vehicle (RV) forensic tool that supports replay with what-if reasoning inside simulator to replace expensive field test based forensics. It features an efficient demand-driven adaptive logging method, a novel replay tehcnique supporting various replay policies that selectively enable/disable information during replay. 

This provides a prototype of our tool and data set to play, including system identification functions, a adaptive logging module, a replayer in a simulator, flight data for real and simulated vehicles.


## Setup

### Software Requirements
1. Matlab R2017B or above
2. Ubuntu 18.04 LTS
3. ArduCopter V4.01
4. Customized Mavproxy 1.8.5
5. Ground Control Systems (QGroundControl and MissionPlanner)
 

### Target Systems
1. SimQuad
2. 3DR Solo
3. SimRover
4. Erle-Rover

## Directory Structure

* Dataset
* System Identification
* Adaptive logging (Disturbance Capturing)
* Replayer and Simulation

## Dataset

* `Data/Drone/Test3`: SimQuad
* `Data/Drone/Test5`: 3DR Solo
* `Data/Rover/Test6`: SimRover
* `Data/Rover/Test8`: Erle-Brain


## Install

For matlab code, install Matlab R2017B or above.

For controller part and SITL simulation, follow the installing instructions of [Ardupilot](https://ardupilot.org/dev/docs/where-to-get-the-code.html) and [OpenSolo](https://github.com/OpenSolo/OpenSolo/wiki). 

## How to run

1. Run `systemid.m` or `rover_systemid.m` to do system identification and learn model parameters.
2. Run `disturbance_restore.m` or `disturbance_restore_rover.m` to capture disturbance of trace and have adaptively logged disturbance data and synchronization data with name `XXX_disturb_lin.csv`, `XXX_disturb_rot.csv` and `XXX_syn.csv`.
3. Put the disturbance and sync data to folder `Controllers/ardupilot/libraries/SITL/sim_rerun/MultiCopter/` (for Drones) or `Controllers/ardupilot/libraries/SITL/sim_rerun/Rover/` (for Rovers).
4. Assign the trace number (the `XXX`) to variable `fileNo` in `Controllers/ardupilot/libraries/SITL/SIM_Multicopter.cpp` or `Controllers/ardupilot/libraries/SITL/SIM_Rover.cpp`
5. Change the config file `Controllers/ardupilot/libraries/SITL/sim_rerun/config.csv` to disable `origin_model`, enable `add_disturb` and enable `sync_states`. Leave other configs as `0`. 
6. Build the controller to SITL target (follow building instructions of [Ardupilot](https://ardupilot.org/dev/docs/where-to-get-the-code.html)) and run with following command in `Arducopter` or `APMRover2` folder. (For rovers, `-f X` option is not required).
   
    ```
    sim_vehicle.py -L <place> -f X --map --console
    ```
    
     Make sure replacing `<place>` as the start place in mission plan and upload the same mission plan to vehicle before starting replaying.