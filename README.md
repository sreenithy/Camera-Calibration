## Camera Calibration


The goal of camera calibration is to find the intrinsic and extrinsic parameters of a camera. 

**Overview**


Inorder to calibrate the camera we image a 3D object such as a patterned cube and use the 3D-2D point correspondences between the 3D object and its 2d image to find the camera parameters.

There are two set of parameters that we need to find: intrinsic parameters and extrinsic parameters. Intrinsic parameters are those parameters which are internal to the camera such as focal length, principal points, etc., whereas extrinsic parameters are those parameters that prescribe the location t (translation vector) and orientation R (rotation matrix) of the camera with respect to an external coordinate system (usually called the world coordinate system). In the first part, we will compute only the intrinsic parameters (assuming that the extrinsic parameters are known) and in the second part we will jointly compute the intrinsic and extrinsic parameters. 


**Intrinsic parameter computation**


The calibrating object that we use is a Rubik’s cube.

We image the cube as shown in figure below. We then obtain many 3D-2D point correspondences. In this part, we have already computed the point correspondences and all you have to do is to compute the intrinsic parameters from them.  The 3D-2D correspondences are given in the data file “pt_corres.mat”. This file contains “pts_2D”,  the 2D points and “cam_pts_3D”,  all the corresponding 3D points.Now we have to find the K matrix
![alt text](https://github.com/sreenithy/Camera-Calibration/blob/master/misc/a.png)
**K Matrix**


The matrix K which relates the 3D to the 2D points is an upper triangular matrix with the following shape.
![alt text](https://github.com/sreenithy/Camera-Calibration/blob/master/misc/imageedit_1_9820914905.png "K Matrix")

where αx, αy represents the focal length in terms of the x and y pixel dimensions,px and py are the principal
points and s is the skew parameter.


The relation between the 2D point (x, y) and the corresponding 3D point (X, Y, Z) are given by

1. x = αx(X/Z) + s(Y/Z) + px 

2. y = αy(Y/Z) + py

**Intrinsic and extrinsic parameter computation**

In the previous part, we have computed the intrinsic parameter assuming that the extrinsic parameters are known, i.e., we assumed that we know the 3D point correspondences in the camera coordinate system.  But this is rarely the case; almost always we know the 3D point correspondences only in the world coordinate system and hence we need to estimate both the intrinsic and extrinsic parameters. But before that we need to obtain the 3D-2D point correspondences. The 3D points are described with respect to a world coordinate system as shown in the figure 1. The figure shows the x,y and z axes of the world coordinate system along with some sample 3D points, which are the corners of the squares. There are 28 such points.

1. The 3D points in the world coordinate system are provided in rubik_3D_pts.mat and the corresponding 2D points on the image are provided in rubik_2D_pts.mat

2. Next we want to compute the camera projection matrix P = K[R t], where K is the internal/intrinsic calibration matrix, R is the rotation matrix which specifies the orientation of the camera coordinate system w.r.t the world coordinate system and t is the translation vector which species the location of the camera center in the world coordinate system.

3. For computing P, the code is in the function “calibrate(x,X)” and is based on the “Direct Linear Transformation (DLT)"
The DLT is an important algorithm to understand and is detailed below.

**Discrete Linear Transform**

The Discrete Linear Transorm(DLT) is simple linear algorithm for estimating the camera projection matrix
**P** from corresponding 3-space and image entities. This computation of the camera matrix is known as resectioning.
The simplest such correspondence is that between a 3D point **X** and its image **x** under the unknown
camera mapping. Given sufficiently many such correspondences the camera matrix may be determined.

**Algorithm**

Let’s assume a number of point correspondences between 3D points and 2D image points are given. The
camera matrix is a 3x4 matrix which relates the points by, xi = P.Xi For each correspondence Xi ↔ xi
, we get three equations of which two are linearly independent and is described below


**Steps**

1. From a set of n point correspondences, we obtain a 2nx12 matrix A by stacking up the equations of the
above form for each correspondence

2. Obtain the SVD of A. The unit singular vector corresponding to the smallest singular value is the solution
p. Specifically, if A = UDVT with D diagonal with positive diagonal entries, arranged in descending order
down the diagonal, then p is the last column of V

3. Obtain p and write it in matrix form to get the matrix P

![alt text](https://github.com/sreenithy/Camera-Calibration/blob/master/misc/imageedit_5_5151063270.png )

The projection matrix P is computed by solving the set of equations Ap = 0, where p is the vector containing
the entries of the matrix P.


**Minimum number of point correspondences needed for computing P**

The 3x4 matrix P has 12 elements but the scale is arbitrary and therefore 11 degrees of freedom. Since each
point correspondence gives 2 equations, at a minimum 5.5 such correspondences are required to solve for
P. The 0.5 indicates that only one of the equations is used from the sixth point, that is we choose either the
x-coordinate or the y-coordinate of the sixth image point.


With this minimum number of correspondences, the solution is exact and is obtained by solving Ap = 0 where
A is an 11x12 matrix in this case.


If the data is not exact,the n ≥ 6 point correspondences are given, then there will not be an exact solution and
we solve by minimizing an algebraic or geometric error.

**Obtaining parameters K, R and t from the projection matrix P**

The decomposition of P into K,R,t is done as by RQ decomposition. It involves calculating the decomposition A
= R Q where Q is unitary/orthogonal and R upper triangular.

**Verify the accuracy of the computed parameters**

For this we will compute the re-projection error, which is a measure of the distance between the 2D points and the 2D points obtained by projecting the 3D points using the computed camera parameters.
![alt text](https://github.com/sreenithy/Camera-Calibration/blob/master/misc/c.png "Plot of the given 2D points and the re-projected 2D points in the original figure of the Rubik’s cube,
here the red circles denote the given 2D points and the green are the re-projected points")


**References**


[1] R. Hartley and A. Zissermann, Multiview geometry, 2nd edition, Cambridge University Press.


[2] Z. Zhang. A flexible new technique for camera calibration. IEEE Transactions on Pattern Analysis and Machine Intelligence, 22(11):1330-1334, 2000.
