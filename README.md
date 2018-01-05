# JGM_PLANS_2018
Codebase for the paper: An Open Source Flight Dynamics Model and IMU Signal Simulator (IEEE/ION Position Location and Navigation Symposium 2018)

## The following submodules make up this demo:
* JSBSim: An open source flight dynamics model written in c++. Read jsbsim/COPYING for license details prior to using.
* MATLAB-Groves: INS/GNSS simulation source code written for Matlab, released under a Modified BSD License by Paul Groves, author of Principles of GNSS, Inertial, and Multisensor Integrated Navigation Systems (Sencod Edition). Read MATLAB-Groves/License.txt prior to using.
* tcp_udp_ip: A UDP to matlab utility, copyright (c) 2015, Peter Rydesäter. Read tcp_udp_ip/license.txt prior to using.
* flightgear: An open source flight simulator.  Read flightgear/COPYING for license details prior to using.

## JSBSim:
* Building JSBSim using CMAKE:
**    See Section 3 in the INSTALL file.
* Run with:

    ./linux-build/src/JSBSim --script=scripts/c310_plans_racetrack.xml --realtime --logdirectivefile=data_output/flightgear.xml --logdirectivefile=data_output/matlabSocket.xml

## tcp_udp_ip:
* See header of pnet.c for mex compilation instructions.
* With jsbsim running as above, you can view the output using tcp_udp_ip/udp_plotter_jsb(1138) within Matlab.

## Flight Gear:
* Optionally, install flight gear to visualize the output of jsbSim. With jsbsim running as above, you can view the aircraft and map info with:

    fgfs --fg-root="{your path}" --aircraft=c310 --native-fdm=socket,in,60,,5550,udp --fdm=external --airport=KMRY --httpd=8050

    Within flight gear, you can view both the aircraft model simulation as well as a moving map if you click the menu, Equipment -> Map (opens in browser).
