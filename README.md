# ZLeveler

Postprocess 3D printer G-Code output from Cura engine
adjusting hotbed uneveness in Z-axis.

Adjustment of z-axis can be done manually
(without automatic Z-leveling hardware). 

Written in python

# Calibration file

Negative Z-value describes 'valley' (nozzle too far from surface).
Positive Z-value describes 'hill' (nozzle too close to surface).
Units are in mm.

Normal file should consist of all negative Z-values.
Adjust hotbed mechanically so that without zlevler
nozzle at Z=0 doesn't touch the hotbed surface.

Surface sampling: Move the nozzle accross the hotbed and 
count how many papers can be inserted between the nozle and hotbed. 

A 2D interpolation method "RBF" is used which is adaptive
so the calibration points can be randomly sampled.
You dont neccessary need to sample e.g. complete 5x5 grid 
over the hotbed.

Probing example: at point X=10 Y=20 you can insert 3 papers.
Assuming each paper is about 0.1 mm then you'd enter
a line:

    X10 Y20 Z-0.3

Example of complete file:

    zlevel.xyz

    X0   Y0   Z-0.1
    X25  Y0   Z-0.2
    X50  Y0   Z0
    X75  Y0   Z-0.1
    X100 Y0   Z-0.2

    X0   Y25  Z-0.3
    X25  Y25  Z-0.1
    X50  Y25  Z-0.2
    X75  Y25  Z-0.2
    X100 Y25  Z-0.1

    X0   Y50  Z-0.1
    X25  Y50  Z-0.2
    X50  Y50  Z-0.3
    X75  Y50  Z-0.2
    X100 Y50  Z0

    X0   Y75  Z-0.3
    X25  Y75  Z-0.2
    X50  Y75  Z-0.2
    X75  Y75  Z-0.3
    X100 Y75  Z-0.4

    X0   Y100 Z-0.2
    X25  Y100 Z-0.1
    X50  Y100 Z0
    X75  Y100 Z-0.1
    X100 Y100 Z-0.2

Graphical view:

    ./zleveler.py -v1

![ZLEVEL](/pic/zlevel.png)

# Install dependencies

There are not many dependencies, and they are hopfully
easily obtainable:

    apt-get install python3-numpy python3-scipy python3-matplotlib

Works with Python2 or Python3.

# Disclaimer

This code may have bugs and produce g-codes which lead to hardware error.
Be extra careful and always graphically preview generated g-code in 
e.g. repetierhost slice by slice before sending them to the machine.

It currently supports only absolute G code.
Relative G-code will be converted wrong and can damage the machine.

# Todo

    [x] Split long g-code into shorter ones --xymax=10.0
    [x] Option for global z-offset --zoffset=-0.5
    [x] Plot surface with matplotlib -v1
