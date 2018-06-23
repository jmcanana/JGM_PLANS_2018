# JGM_PLANS_2018
Codebase for the paper: An Open Source Flight Dynamics Model and IMU Signal Simulator (IEEE/ION Position Location and Navigation Symposium 2018)

[This is what everything should look like once setup to run.](https://www.dropbox.com/s/yxhjo08jwrrx8m1/insSimulation.MOV?dl=0)

## The following submodules make up this demo:
* JSBSim: An open source flight dynamics model written in c++. Read jsbsim/COPYING for license details prior to using.
* MATLAB-Groves: INS/GNSS simulation source code written for Matlab, released under a Modified BSD License by Paul Groves,
author of Principles of GNSS, Inertial, and Multisensor Integrated Navigation Systems (Second Edition).
Read MATLAB-Groves/License.txt prior to using.
* tcp_udp_ip: A UDP to matlab utility, copyright (c) 2015, Peter Rydesäter. Read tcp_udp_ip/license.txt prior to using.
* flightgear: An open source flight simulator.  Read flightgear/COPYING for license details prior to using.

## Cloning Locally:

* Do this at your command line:
```sh
git clone https://github.com/jmcanana/JGM_PLANS_2018.git
cd JGM_PLANS_2018/
git submodule init
git submodule update
```
* You should see this:
```console
[jmcanana@jmpc ~]$ git clone https://github.com/jmcanana/JGM_PLANS_2018.git
Cloning into 'JGM_PLANS_2018'...
remote: Counting objects: 23, done.
remote: Total 23 (delta 0), reused 0 (delta 0), pack-reused 23
Unpacking objects: 100% (23/23), done.
[jmcanana@jmpc ~]$ cd JGM_PLANS_2018/
[jmcanana@jmpc JGM_PLANS_2018](master=)$ git submodule init
Submodule 'MATLAB-Groves' (https://github.com/jmcanana/MATLAB-Groves.git) registered for path 'MATLAB-Groves'
Submodule 'flightgear' (git@github.com:jmcanana/flightgear.git) registered for path 'flightgear'
Submodule 'jsbsim' (https://github.com/jmcanana/jsbsim.git) registered for path 'jsbsim'
Submodule 'simgear' (git@github.com:jmcanana/simgear.git) registered for path 'simgear'
Submodule 'tcp_udp_ip' (https://github.com/jmcanana/tcp_udp_ip.git) registered for path 'tcp_udp_ip'
[jmcanana@jmpc JGM_PLANS_2018](master=)$ git submodule update
Submodule path 'MATLAB-Groves': checked out '7e969a7f49ea318bcca5bac1347870fe6c1f2da0'
Submodule path 'flightgear': checked out '8653db1b23c41710de1548d69893686491fdd43b'
Submodule path 'jsbsim': checked out 'd2d9b75655df24ea65b0e83db307782c5ba04569'
Submodule path 'simgear': checked out '9ae949006123247253bfaf36b6a3d6147ffb7583'
Submodule path 'tcp_udp_ip': checked out '4afd01196d32a0ce1517970ea81c2d0dd47929b7'
```

## JSBSim:
* Building JSBSim using CMAKE:
    * See Section 3 in the INSTALL file.
    * Something like this:
    ```console
    [jmcanana@jmpc JGM_PLANS_2018](master=)$ cd jsbsim/
    jmcanana@jmpc jsbsim]((d2d9b75...))$ mkdir build_linux
    [jmcanana@jmpc jsbsim]((d2d9b75...))$ cd build_linux/
    [jmcanana@jmpc build_linux]((d2d9b75...))$ cmake ..
    -- The C compiler identification is GNU 7.2.1
    .
    .
    .
    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/jmcanana/JGM_PLANS_2018/jsbsim/build_linux
    [jmcanana@jmpc build_linux]((d2d9b75...))$ make
    Scanning dependencies of target libJSBSim
    [  1%] Building CXX object src/CMakeFiles/libJSBSim.dir/FGFDMExec.cpp.o
    [  2%] Building CXX object src/CMakeFiles/libJSBSim.dir/FGJSBBase.cpp.o
    .
    .
    .
    [ 99%] Building CXX object utils/aeromatic++/CMakeFiles/aeromatic.dir/aeromatic.cpp.o
    [100%] Linking CXX executable aeromatic
    [100%] Built target aeromatic
    [jmcanana@jmpc build_linux]((d2d9b75...))$ cd ..
    [jmcanana@jmpc jsbsim]((d2d9b75...))$  ./build_linux/src/JSBSim --script=scripts/c310_plans_racetrack.xml  --realtime --logdirectivefile=data_output/matlabSocket.xml
    ```
    * The above steps should have JSBSim taking off from MRY to fly a racetrack pattern above the PLANS conference in a Cessna C310. The data described in jsbsim/data_output/matlabSocket.xml will be streamed to a UDP port on localhost:1138 at 50Hz.
    * Complete the MATLAB-Groves and tcp_udp_ip steps to parse this data and run an INS using MATLAB.
    * If you want to just see the data at the UDP port (and you are using linux), you may use netcat in a new terminal:
    ```console
    [jmcanana@jmpc Remarkable](master=)$ nc -l -u 1138
       90.58,  36.5939752, -121.878947,   1505.2931, -11.6425513, -210.457783,-0.392873515,0.0110301989,-0.0472051443,  4.65417476
        90.6,  36.5939745, -121.878962,  1505.30094, -11.6399452, -210.468563,-0.391465376,0.0110343134,-0.0472214922,  4.65419072
       90.62,  36.5939739, -121.878976,  1505.30875, -11.6373351, -210.479335,-0.390060735,0.0110382698,-0.0472378113,  4.65420671
       90.64,  36.5939733,  -121.87899,  1505.31654,  -11.634721, -210.490098,-0.388659614,0.0110420693,-0.0472541014,  4.65422272
    ```
    * If you do not have any listeners at port 1138, JSBSim will spew "send: Connection refused" errors until a listening port is opened.


    * You can connect to JSBSim via telnet to control in realtime from a separate shell with:
    ```console
    [jmcanana@jmpc ~]$ telnet localhost 1137
    Connected to localhost.
    Escape character is '^]'.
    Connected to JSBSim server
    JSBSim> set ap/active-waypoint 10
    JSBSim> set ap/active-waypoint 11
    JSBSim> set ap/active-waypoint 3
    ```
    * As defined in jsbsim/scripts/c310_plans_racetrack.xml:
        +  Waypoint 10 fly North.
        +  Waypoint 11 fly East.
        +  Waypoint 3 rejoins the race track.

## MATLAB-Groves:
* This codebase has been extended by us with the following functions:
** JsbSim_tightly_coupled_INS_GNSS.m
** JsbSim_loosely_coupled_INS_GNSS.m

These functions run an INS on the jsbSim data.  They are dependent on the tcp_udp_ip package.
You can inject a lever arm error into JsbSim_loosely_coupled_INS_GNSS.m by setting  the lever_arm vector.
You can inject GPS update delay of one epoch in JsbSim_tightly_coupled_INS_GNSS.m by setting the
use_old_GNSS_data to true.



## tcp_udp_ip:
* See header of pnet.c for mex compilation instructions.
* With jsbsim running as above, you can view the output using tcp_udp_ip/udp_plotter_jsb(1138) within Matlab.

## Flight Gear:
* Optionally, install flight gear to visualize the output of jsbSim. With jsbsim running as above, you can view the aircraft and map info with:

    fgfs --fg-root="{your path}" --aircraft=c310 --native-fdm=socket,in,60,,5550,udp --fdm=external --airport=KMRY --httpd=8050

    Within flight gear, you can view both the aircraft model simulation as well as a moving map if you click the menu, Equipment -> Map (opens in browser).
* Run with:
    ```console
    [jmcanana@jmpc jsbsim]((d2d9b75...))$ ./build-linux/src/JSBSim --script=scripts/c310_plans_racetrack.xml --realtime --logdirectivefile=data_output/flightgear.xml --logdirectivefile=data_output/matlabSocket.xml
    ```
