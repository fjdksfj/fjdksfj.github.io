##  Spectral Reconstruction from Dispersive Blur:Approaching Full Light Throughput Spectral Imager

### Experiment :

* [More results](#res)
* [Experimental details](#para)
   
    * [Calibration details](#calib)
    * [Parameters setting](#parameters)


### <span id="res">Experimental Results on Real Data (#R3 & #R4)</span>

![image](https://github.com/fjdksfj/fjdksfj.github.io/blob/master/more_res.png)

> Fig. 1: Experimental results on captured image data. The synthetic RGB images, details and the spectral response of scene points are evaluated.

As shown in Fig. 1, the first column is the  captured dispersive image and the second column is captured sharp gray image. Based on the reconstructed multispectral images from these two measurements, the synthetic images by integrating with the camera response curves are shown in the third column. Physically captured RGB images by an additional RGB camera are in the forth column. Our hybrid system and pixel-wise reconstruction algorithm could recover the spatial details, as shown in the fifth and sixth columns. Futhermore, the spectral response curves  at golden points are shown in the last column to directly verify the effectiveness of our method. 
Scenes with different colors are captured especially blue to validate our algorithm.

### <span id="para">Experimental details (#R1)</span>
#### <span id="calib">Calibration details (#R1)</span>

Step 1, in order to align two cameras, checkerboard calibration method is used and we further correct pincushion distortion caused by lens. 

Step 2, due to the non-uniformity of the prism dispersion, we use a monochromator to calibrate the distribution of different wavelengths and check the dispersion width.

Step 3, the dispersion nonlinearity results in uneven energy distribution along wavelength, and here we use nonlinear interpolation according by the non-uniform wavelength distribution we have already calabrated in step 2.

Step 4, we further perform white balance by correcting the integral curves instead of the RGB curves measured by the monochromator through solving the equation (1) as following (taken the R channel as an example):
$$ RR_cS = b \tag{1} $$
where, R means the integral curve of red channel in RGB sensors measured by the monochromator, $R_c$ is a diagonal matrix the elements on the diagonal are correction factors for different channels which may be caused by spectral transmission curve of lens. $b$ means the captures value of red channel. If the number of points where the spectra are known is greater than the number of channels, then $R_c$  can be solved.

since it is difficult to accurately calibrate the spectral transmission of optical lens and the spectral curves of RGB sensor in practice, it is normal that there are some color artifacts in the synthesized rgb images.
Since the prism disperser which leads to nonlinear dispersion is used in our system, it is impossible to keep the wavelength interval
per pixel equal. Instead, we precalibrate the non-linear dispersion of different wavelengths, and correct the reconstructed hyperspectral curves with the calibration results.  

#### <span id="parameters">Parameters setting(#R1)</span>

Different coefficients are used to represent the strength of different constraints which determines convergence speed of algorithm.
Different parameters are choosen empirally in a coarse-to-fine tuning method.
It should be noted that these parameters are almost system-independent and fixed in both synthetic and real experiments. 

<div align="center">
<img src="https://github.com/fjdksfj/fjdksfj.github.io/blob/master/alg.jpg"  height="50%" width="50%">
</div>
<div align="center">
 > Alg. 1: The main steps of our reconstruction algorithm.
</div>

Alg. 1 demonstrate the main steps of our reconstruction algorithm. Three loops are applied to make the algorithm converge gradually. The outer loop is used to iteratively increase the weight parameters $\beta_{cs}$ and $\lambda_{side}$. The middle loop is utilized for alternatively optimize the Q-subproblem and S-subproblem. The inner loop is applied to solving the S-problem using CG algorithm iteratively.  In this paper, we empirically set $\lambda_{DoB} = 10$, $\lambda_{cs} = 1e-5$, and $\lambda_{side}^0 = 10$ initially and $\lambda_{side}^{t+1} = \sqrt{2}\lambda_{side}^{t}$ during outer iterations, $t$ denotes the number of outer iteration.
To balance the performance and running time, we set set $MaxOuterIter = 6$, $MaxMidIter = 5$, and $MaxInnerIter = 100$ to terminate the algorithm, in case the algorithm does not converge into the $10^{-7}$ error bands in time.




For experimental reproducibility, hardware parameters are listed in the table below.

|  Sensor part number    | Aperture(F) |Exposure time(ms)|
| -------------------    | -------| ------------|
| FLIR GS3-u3-32S4C-C    |    1/8 | 30        |
| FLIR GS3-u3-32S4M-C    |    1/8 | 30        |


As for running time, the iterative method takes about 2 hours to converge on the Intel(R)Core(TM)i7-6700K CPU @4.00GHz hardware platform with 32.0G RAM.

As shown in Fig. 6, our system is based on prism.
The prism can introduce additional anisotropy aberrations, which may deteriorate the quality of our imaging system. This is a common problem of prism-based spectrometers, and it can be greatly alleviated by adopting Amici prism (exactly what we use). The Amici prism is a completely symmetrical structure with three prisms. Due to its special materials and structures, it can improve the nonlinearity of dispersion, line bending and band bending. 
