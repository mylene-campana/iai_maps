# iai_maps
The semantic and ground lab and environment maps for projects in the IAI group in the University of Bremen

This package containts a kitchen environment for motion planning. No robot nor scripts are implemented here . 
For example if you want to use the PR2 robot in this environment, you can refer to github.com/humanoid-path-planner/hpp_tutorial 
scripts and URDF/SRDF/Class files for PR2.

The environment can be vizualised with HPP-gepetto-viewer (github.com/humanoid-path-planner) or with RViz (must create .launch files).

To install the package with cmake, simply:

  - Create a 'build' directory in the source package,

  - in the created /build, configure the package - particularly the 'install path variable' - and install it with 'ccmake ..' and 'make install'.
