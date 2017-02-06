# CTR Project Documentation
###This file contains the documentation for the CTR project code.
####[run_optimization.m](index1)
(index1)Loads all the data
```
anatomy.filename = ['../../data/' scenario '/anatomy.mat'];
....
```
define the optimization variables and optimization method i.e. fminsearchbnd
```
mopts.variables = {'lengths_curved', 'curvatures', 'base'};
mopts.method = 'fminsearchbnd';
```

_TBC - what does it do?_
targets = targets([2 3 4 5 6 7 1 8 9 10 11 12 13], :);

What is the significance of H? It is end-effector _but don’t understand how it has been defined._

Creates concentric tube robots signature of **Balance** type.Balanced pair is 2 curved sgments and 2 straight segments
```
ctr_ = [];
i = 1;
ctr_(i).type      = 'balanced';
```
Creates concentric tube robots signature of **Fixed** type. Fixed curvature tube is a straight segment and a curved segment.
```
i = 2;
ctr_(i).type      = 'fixed';

ctr_(i).u         = 45;
ctr_(i).u_min     = 0;
```
Attributes of CTR:
* Id
* U		curvature
* Length	arclength
* K		stiffness
* Diameter	
* Theta		need to write
* Phi		need to write
* Start		arc start
* End		arc end
* Curve		holds the curve





