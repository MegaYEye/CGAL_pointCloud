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

#### Remove outliers
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3OutlierRemoval

#### Simplify the point cloud with WLOP
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3Simplification

#### Smooth the simplified point cloud with bilateral smoothing
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3Smoothing

#### Estimate the normals
http://doc.cgal.org/latest/Point_set_processing_3/index.html#Point_set_processing_3NormalEstimation



#### NOTES!

* It might be that your point cloud is so good quality that you might not need to do any smoothing or other filtering. Test with your point clouds and do sensitivity analysis?
* These algorithms are not necessarily not the most computationally efficient. Plus there is I/O overhead having to read/write the results between the blocks
