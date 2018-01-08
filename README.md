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

## JSBSim:
* Building JSBSim using CMAKE:
**    See Section 3 in the INSTALL file.
* Run with:

    ./linux-build/src/JSBSim --script=scripts/c310_plans_racetrack.xml --realtime --logdirectivefile=data_output/flightgear.xml --logdirectivefile=data_output/matlabSocket.xml

Control in realtime from a separate shell with:
[jmcanana@jmpc ~]$ telnet localhost 1137
Connected to localhost.
Escape character is '^]'.
Connected to JSBSim server
JSBSim> set ap/active-waypoint 10
JSBSim> set ap/active-waypoint 11
JSBSim> set ap/active-waypoint 3

As defined in scripts/c310_plans_racetrack.xml
Waypoint 10 fly North.
Waypoint 11 fly East.
Waypoint 3 rejoins the race track.

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
