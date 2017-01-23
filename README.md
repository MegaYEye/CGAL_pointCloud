# CGAL_pointCloud

Example pipeline for process point clouds with CGAL library
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Chapter_Point_Set_Processing

Code developed by Sharan Sankar ([@sharansankar](https://github.com/sharansankar)) as high school summer project, and exploited further for these sort of examples.

## Prerequisites


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

#### Remove outliers
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3OutlierRemoval

This is rather fast to compute:
_argv[1]= % of point clouds to remove [now we remove 10%]_
_argv[2]= the number of neighbouring points that are taken into consideration [def 24]_

```
./removeOutliers/build/removeOutliers 10 24 ../data/cloud_subsetSmall.xyz ../outputPath/cloud_outlierRemoved.xyz
```

#### Simplify the point cloud with WLOP
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3Simplification

More time-consuming to compute
_argv[1]=  % of point clouds to keep [now we remove 90%, *keep 10%*]_
_argv[2]= the number of neighbouring points that are taken into consideration [def 24]_

```
./WLOP/build/WLOP 10 0.5 ../outputPath/cloud_outlierRemoved.xyz ../outputPath/cloud_WLOPed.xyz
```

#### Smooth the simplified point cloud with bilateral smoothing
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3Smoothing

_argv[1]= the control sharpness of the result_
_argv[2]= the number of times the projection is applied_
_argv[3]= neighboorhood size_

```
./normalEstimation/build/bilateralSmoothing 20 2 75 ../outputPath/cloud_bilateralSmoothed.xyz ../outputPath/cloud_normalEstimated.xyz
```

#### Estimate the normals
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3NormalEstimation



#### NOTES!

* It might be that your point cloud is so good quality that you might not need to do any smoothing or other filtering. Test with your point clouds and do sensitivity analysis?
* These algorithms are not necessarily not the most computationally efficient. Plus there is I/O overhead having to read/write the results between the blocks
* Filenames are hard-coded so you need to do this now in order.

