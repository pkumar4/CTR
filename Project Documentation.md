# CTR Project Documentation
###This file contains the documentation for the CTR project code.
####run_optimization.m

Loads all the data
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

Creates an array of CTR
```
ctr_ = [];
```
And add the below CTRs of Balance and Fixed type to ctr_ array

Creates concentric tube robots signature of **Balance** type.Balanced pair is 2 curved sgments and 2 straight segments
```

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
**Attributes of CTR**:
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

The array ctr_ is passed to CTROptimize
```
[ctr_ err] = CTROptimize(ctr_, H, anatomy, targets, vector, base, mopts);
```

####CTRCreateStructure.m
This file will receive CTR signature structure and populates all the parameters based on relationships between the tubes
S: The potential curvature changing points that should be examined.**Need to clarify**

A balanced pair is created with 4 segments(2 curved segments and 2 straight segments). A fixed curvature tube is created with 2 segments(1 straight and 1 curved segment) and number of segment is set accordingly as below:
length(ctr_) = 3 as being passed from run_optimization.m
```
for i = 1:length(ctr_)
    if strcmp(ctr_(i).type, 'balanced')
      n_segments = n_segments + 4;
    elseif strcmp(ctr_(i).type, 'fixed')
      n_segments = n_segments + 2;
    end
  end
```
A struct is created with all the attributes of CTR
```
ctr = CTRCreateSegment();
```
i is used to iterate new structure and j for old structure
If n_segment = 4( 'Balanced' type) the below iterates 1 time and initialises the new struct as below:
3rd element in new struct = first element in the old struct
4th element in new struct = same as 3rd element in new struct except theta and k are overwritten
```
ctr(i+2).id       = i + 2;
      ctr(i+2).u        = ctr_(j).u;
      ctr(i+2).u_temp   = ctr_(j).u;
      ctr(i+2).length   = ctr_(j).c_len;
      ctr(i+2).k        = ctr_(j).k(1);
      ctr(i+2).k_temp   = ctr_(j).k(1);
      ctr(i+2).diameter = ctr_(j).diameter;
      ctr(i+2).theta    = ctr_(j).theta(1);
      ctr(i+2).phi      = max(0, min(ctr_(j).phi, ctr_(j).c_len));
      ctr(i+2).start    = max(0, ctr(i+2).phi + all_end - ctr(i+2).length);
      ctr(i+2).end      = ctr(i+2).phi + all_end;
      
      % Curved segment #2
      ctr(i+3)       = ctr(i+2);
      ctr(i+3).id    = i+3;
      ctr(i+3).theta = ctr_(j).theta(2);
      if length(ctr_(j).k) == 2
        ctr(i+3).k = ctr_(j).k(2);
      end
```
The first element in new strcut has the stiffness, diameter and theta same as first element in old struct
Length is set as diff between arc end and arc length for 3rd element
```
	ctr(i).id       = i;
	ctr(i).u        = 0;
	ctr(i).u_temp   = 0;
	ctr(i).length   = max(0, ctr(i+2).end - ctr(i+2).length);
	ctr(i).k        = ctr_(j).k(1);
	ctr(i).k_temp   = ctr_(j).k(1);
	ctr(i).diameter = ctr_(j).diameter;
	ctr(i).theta    = ctr_(j).theta(1);
	ctr(i).phi      = NaN;
	ctr(i).start    = 0;
	ctr(i).end      = ctr(i).length;
```
The second element is the same as first element except for the value of theta and k

** Why we are taking the unique s?
```
s = unique(s);
```








