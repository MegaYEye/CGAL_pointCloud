# CGAL_pointCloud

Example pipeline for process point clouds with CGAL library
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Chapter_Point_Set_Processing

Code developed by Sharan Sankar ([@sharansankar](https://github.com/sharansankar)) as high school summer project, and exploited further for these sort of examples.

## Prerequisites

* CGAL, http://doc.cgal.org/latest/Manual/installation.html

```
sudo apt-get install libcgal-dev libcgal-demo
sudo apt-get libqglviewer-dev libmpfi-dev libipe-dev
```

You might need Eigen3 (http://eigen.tuxfamily.org/), and OpenMesh (http://www.openmesh.org/). This need to be checked still


## Install

Dummy install for Linux shell (will fail of course if you don't have the dependencies handled)

### 1) Clone the repo

### 2) Build all individual folders

```
cd CGAL_pointCloud
cd CGAL
cd bilateralSmoothing && mkdir build && cd build && cmake ../src && make && cd .. && cd ..
cd EAR && mkdir build && cd build && cmake ../src && make  && cd .. && cd ..
cd normalEstimation && mkdir build && cd build && cmake ../src && make  && cd .. && cd ..
cd removeOutliers && mkdir build && cd build && cmake ../src && make  && cd .. && cd ..
cd WLOP && mkdir build && cd build && cmake ../src && make   && cd .. && cd ..
```

### 3) Using then the different "filters" with the example indoor point cloud scan provided with the repository

The provided algorithms are not the most adaptive that there are as they require bunch of user-set parameters, and in the end You get a ton of different possible parameter combinations and user frustration when trying to get these to work in robust way.

#### 3.1 Remove outliers
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3OutlierRemoval

This is rather fast to compute:

_argv[1]= % of point clouds to remove [now we remove 10%]_
_argv[2]= the number of neighbouring points that are taken into consideration [def 24]_

```
./removeOutliers/build/removeOutliers 10 24 ../data/cloud_subsetSmall.xyz ../outputPath/cloud_outlierRemoved.xyz
```

#### 3.2a Simplify the point cloud with WLOP
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3Simplification

More time-consuming to compute, but this can be accelerated with *multiple cores* with [Intel TBB](http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3Example_wlop) (in other words buying a lot of cloud computing power)

_argv[1]=  % of point clouds to keep [now we remove 90%, *keep 10%*]_

_argv[2]= the number of neighbouring points that are taken into consideration [def 24]_

```
./WLOP/build/WLOP 10 0.5 ../outputPath/cloud_outlierRemoved.xyz ../outputPath/cloud_WLOPed.xyz
```

#### 3.3a Smooth the simplified point cloud with bilateral smoothing
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3Smoothing

_argv[1]= the control sharpness of the result [def 20]_

_argv[2]= the number of times the projection is applied  [def 2]_

_argv[3]= neighboorhood size [def 75]_

```
./bilateralSmoothing/build/bilateralSmoothing 20 2 75 ../outputPath/cloud_WLOPed.xyz ../outputPath/cloud_bilateralSmoothed.xyz
```

*Segmentation fault (core dumped)? Does not work now*

#### 3.2-3B EAR (Combines EAR + Bilateral smoothing + Edge-Aware resampling on one go)
http://vcc.szu.edu.cn/research/2013/EAR/
http://doc.cgal.org/latest/Point_set_processing_3/

Rather time-consuming to compute again:

_argv[1]= control sharpness of the result, e.g. 25_

_argv[2]= higher values will sample more points near the edges, e.g. 0 for uniform sampling, 0.5 for edge-weighed_

_argv[3]= initial size of neighborhood, e.g. 0.25_

_argv[4]= upsampling factor, e.g. with 1, the number of points same constant_

```
./EAR/build/EAR 25 0.5 0.25 2 ../outputPath/cloud_outlierRemoved.xyz ../outputPath/cloud_EARed.xyz
```

#### 3.4 Estimate the normals (from WLOP and from EAR)
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3NormalEstimation

_argv[1]= K-nearest neighbors = 3 rings [def 18]_

```
./normalEstimation/build/normalEstimation 18 ../outputPath/cloud_WLOPed.xyz ../outputPath/cloud_WLOP_normalEstimated.xyz
./normalEstimation/build/normalEstimation 18 ../outputPath/cloud_EARd.xyz ../outputPath/cloud_EAR_normalEstimated.xyz
```

#### NOTES!

* It might be that your point cloud is so good quality that you might not need to do any smoothing or other filtering. Test with your point clouds and do sensitivity analysis?
* These algorithms are not necessarily not the most computationally efficient. Plus there is I/O overhead having to read/write the results between the blocks
* Filenames are hard-coded so you need to do this now in order.

