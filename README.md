Download Link: https://assignmentchef.com/product/solved-computer-vision-lab-exercise-5
<br>
<h1> Epipolar Geometry</h1>

Students should work on this lab exercise in groups of two people. In this assignment you will write a function that takes two images as input and computes fundamental matrix by Normalized Eight-point Algorithm with RANSAC 1.3. You will work with supplied teddy bear (<em>obj02 001.jpg</em>, <em>obj02</em>

<em>002.jpg</em>).

<h2>1         Fundamental Matrix Estimation</h2>

Please consult the lecture slides for the definition of the fundamental matrix and Epipolar geometry and the following papers [1, 2]. The overall scheme can be summarized as follows:

<ol>

 <li>Detect interest points in each image.</li>

 <li>Describe the local appearance of the regions around interest points.</li>

 <li>Get a set of supposed matches between region descriptors in each image and select 20 correspondences from them. Remember that matched points are needed to be well distributed on the images for an accurate estimation of the fundamental matrix.</li>

 <li>Estimate the fundamental matrix for the given two images.</li>

</ol>

The first two steps can be performed using Harris/Hessian Affine implementation which can be downloaded from <a href="http://www.robots.ox.ac.uk/~vgg/research/affine/detectors.html">http://www.robots.ox.ac.uk/ </a><a href="http://www.robots.ox.ac.uk/~vgg/research/affine/detectors.html"><sub>~</sub></a><a href="http://www.robots.ox.ac.uk/~vgg/research/affine/detectors.html">vgg/research/affine/detectors.html</a><a href="http://www.robots.ox.ac.uk/~vgg/research/affine/detectors.html">.</a> Third step can be performed using vl feat functions:

<table width="527">

 <tbody>

  <tr>

   <td width="27">1</td>

   <td width="500">[x, y, a, b, c, desc ] = ./ extractfeatures -haraff -i …img.png -sift</td>

  </tr>

  <tr>

   <td width="27">2</td>

   <td width="500">[matches, scores] = vlubcmatch (desc1 , desc2)</td>

  </tr>

 </tbody>

</table>

<strong>Note: </strong>Eliminate detected interest points on background. In the next stage, for given <em>n &lt; </em>8 known corresponding points pairs in stereo images, we can formulate a homogenous linear equation as follows:

<em>,                              </em>(1)

where <em>x<sub>i </sub></em>and <em>y<sub>i </sub></em>denote <em>x </em>and <em>y </em>coordinates of the <em>i<sup>th </sup></em>point <em>p<sub>i</sub></em>, respectively.




Equation 1 can also be written as

<em>f</em><sub>11</sub>

<table width="460">

 <tbody>

  <tr>

   <td width="57"></td>

   <td width="45"><em>x</em>1<em>y</em>10…<em>x</em><em>ny</em><em>n</em>0</td>

   <td width="30"><em>x</em><sub>1</sub>…<em>x<sub>n</sub></em></td>

   <td width="45"><em>y</em>1<em>x</em>01…<em>y</em><em>nx</em>0<em>n</em></td>

   <td width="44"><em>y</em>1<em>y</em>10…<em>y</em><em>ny</em><em>n</em>0</td>

   <td width="28"><em>y</em><sub>1</sub>…<em>y<sub>n</sub></em></td>

   <td width="30"><em>x</em>0<sub>1</sub>…<em>x</em>0<em><sub>n</sub></em></td>

   <td width="28"><em>y</em><sub>1</sub>0…<em>y<sub>n</sub></em>0</td>

   <td width="134"><em>,</em></td>

   <td width="20">(2)</td>

  </tr>

 </tbody>

</table>

<em>f</em>21

 




|                                      {z                                      }<em>f</em>13

A                                                                                      

<em>f</em>23 <em>f</em>33

where F denotes the fundamental matrix.

<h2>2        Eight-point Algorithm</h2>

<ul>

 <li>Construct the <em>n </em>× 9 matrix A</li>

 <li>Find the SVD of <em>A </em>: <em>A </em>= <em>UDV <sup>T</sup></em></li>

 <li>The entries of <em>F </em>are the components of the column of <em>V </em>corresponding to the smallest singular value.</li>

</ul>

Estimated fundamental matrix F is almost always non-singular (read pages 215 221 from Forsyth &amp; Ponce book for further details). The singularity of fundamental matrix is enforced by adjusting the entries of estimated <em>F</em>:

<ul>

 <li>Find the SVD of <em>F </em>: <em>F </em>= <em>U<sub>f</sub>D<sub>f</sub>V<sub>f</sub><sup>T</sup></em></li>

 <li>Set the smallest singular value in the diagonal matrix <em>D<sub>f </sub></em>to zero in order to obtain the corrected matrix <em>D<sub>f</sub></em></li>

 <li>Recompute</li>

</ul>

<h2>3         Normalized Eight-point Algorithm</h2>

It turns out that a careful normalization of the input data (the point correspondences) leads to an enormous improvement in the conditioning of the problem, and hence in the stability of the result. The added complexity necessary for this transformation is insignificant.

<strong>3.1       Normalization:</strong>

We want to apply a similarity transformation to the set of points√        <em>p<sub>i </sub></em>so that their mean is 0 and the average distance to the mean is  2.

Let,

and  , where <em>x<sub>i </sub></em>and <em>y<sub>i </sub></em>denote x and y

coordinates of a point <em>p<sub>i</sub></em>, respectively.

Then ˆ<em>p<sub>i </sub></em>= <em>Tp<sub>i</sub></em>. Check and show that the set of points ˆ<em>p<sub>i </sub></em>satisfies our criteria.

Similarly, define a transformation <em>T </em>using the set {<em>p</em>ˆ<em><sub>i</sub></em>}, and let ˆ   .

<strong>3.2         Find a fundamental matrix:</strong>

<ul>

 <li>Construct a matrix <em>A </em>from the matches ˆ<em>p<sub>i </sub></em>&#x2194; <em>p</em>ˆ<em><sub>i</sub></em><sup>0 </sup>and get <em>F</em>ˆ from the last column of <em>V </em>in the SVD of <em>A</em></li>

 <li>Find the SVD of</li>

 <li>Set the smallest singular value in the diagonal matrix <em>D<sub>F</sub></em>ˆ to zero in order to obtain the corrected matrix</li>

 <li>Recompute</li>

</ul>

<strong>3.3       Denormalization:</strong>

Finally, let <em>F </em>= <em>T<sup>T</sup>FT</em><sup>ˆ </sup>.

<h2>4                 Normalized Eight-point Algorithm with RANSAC</h2>

Fundamental matrix estimation step given in Section 3.2 can also be performed via a RANSAC-based approach. First pick 8 point correspondences randomly from the set {<em>p</em>ˆ<em><sub>i </sub></em>&#x2194; <em>p</em>ˆ<em><sub>i</sub></em><sup>0</sup>}, then, calculate a fundamental matrix <em>F</em><sup>ˆ0</sup>, and count the number of inliers (the other correspondences that agree with this fundamental matrix). Repeat this process many times, pick the largest set of inliers obtained, and apply fundamental matrix estimation step given in Section 3.2 to the set of all inliers.

In order to determine whether a match ˆ<em>p<sub>i </sub></em>&#x2194; <em>p</em>ˆ<em><sub>i</sub></em><sup>0 </sup>agrees with a fundamental matrix <em>F</em>, we typically use the Sampson distance as follows:

<em>,                         </em>(3)

where (<em>Fp</em>)<sup>2</sup><em><sub>j </sub></em>is the square of the <em>j<sup>th </sup></em>entry of the vector <em>Fp</em>. If <em>d<sub>i </sub></em>is smaller than some threshold, the match is said to be an inlier.

<h2>5      Outputs</h2>

The example of the images that you can plot for these assignment are provided in Fig. 1.

Figure 1: Feature matching and Epipolar lines for the teddy bear image example

Moreover, since the rest of the project needs many point matches on 20 images, you can start to find more point correspondences on different image pairs of the teddy-bear.

<h2>References</h2>

<ul>

 <li>Richard I. Hartley. ”In Defence of the Eight-Point Algorithm”. IEEE Transactions on Pattern Analysis and Machine Intelligence, June 1997.</li>

 <li>Fred Rothganger, Svetlana Lazebnik, Cordelia Schmid, Jean Ponce.”3D Object Modeling and Recognition Using Local Affine-Invariant Image Descriptors and Multi-View Spatial Constraints”. International Journal of Computer Vision, March 2006.</li>

</ul>