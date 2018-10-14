# Camera-Calibration
The goal of camera calibration is to find the intrinsic and extrinsic parameters of a camera. 

**Overview**

Inorder to calibrate the camera we image a 3D object such as a patterned cube and use the 3D-2D point correspondences between the 3D object and its 2d image to find the camera parameters.

There are two set of parameters that we need to find: intrinsic parameters and extrinsic parameters. Intrinsic parameters are those parameters which are internal to the camera such as focal length, principal points, etc., whereas extrinsic parameters are those parameters that prescribe the location t (translation vector) and orientation R (rotation matrix) of the camera with respect to an external coordinate system (usually called the world coordinate system). In the first part, we will compute only the intrinsic parameters (assuming that the extrinsic parameters are known) and in the second part we will jointly compute the intrinsic and extrinsic parameters. 


**Intrinsic parameter computation**

The calibrating object that we use is a Rubik’s cube. We image the cube as shown in figure below. We then obtain many 3D-2D point correspondences. In this part, we have already computed the point correspondences and all you have to do is to compute the intrinsic parameters from them.  The 3D-2D correspondences are given in the data file “pt_corres.mat”. This file contains “pts_2D”,  the 2D points and “cam_pts_3D”,  all the corresponding 3D points. Obtain the intrinsic parameter matrix K and enter in your report. Given: K has the following form:
