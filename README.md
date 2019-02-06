# Rebuttal Material: Spectral Reconstruction from Dispersive Blur:Approaching Full Light Throughput Spectral Imager

# Experiment :

* [More results](#res)
* [Experimental details](#para)
   
    * [Calibration steps](#calib)
    * [Parameters setting](#parameters)


## <span id="res">Experimental Results on Real Data (#R3 & #R4)</span>

![image](https://github.com/fjdksfj/fjdksfj.github.io/blob/master/more_res.jpg)

> Fig. 1: Experimental results on captured image data. The synthetic RGB images, details and the spectral response of scene points are evaluated.

As shown in Fig. 1, The first column is the captured sharp gray image and the second column is the captured dispersive image. Based on the reconstructed multispectral images from these two measurements, the synthetic images by integrating with the camera response curve are shown in the third column. Real captured RGB images are in the forth column. Our hybrid system and pixel-wise reconstruction algorithm could cover the spatial details as in the fifth and sixth columns. Futhermore, the spectral response curves are shown in the last column to directly verify effectiveness of our method. 
Scenes with different colors are captured especially blue to prove the robustness of our algorithm.

## <span id="para">Experimental details (#R1)</span>
### <span id="calib">Calibration steps (#R1)</span>

First, in order to align two cameras, checkerboard calibration method is used and we further correct pincushion distortion caused by lens. Secondly, due to the non-uniformity of the prism dispersion, we use a monochromator to calibrate the distribution of different wavelengths and check the dispersion width; By fine-tuning the position of the prism in hybrid system to ensure that the acquired images is in accordance with the imaging model in Equation 5 and the delta x is 1 pixel apart.  

### <span id="parameters">Parameters setting(#R1)</span>

Different coefficients are used to represent the strength of different constraints which determines convergence speed of algorithm. Empirically, by monitoring the convergence direction of temporary variables, and different parameters are set by large-step method followed by small step. It should be noted that these parameters are almost system-independent and fixed in both synthetic and real experiments.

For experimental reproducibility, hardware parameters are detailed in the table below.

|  Sensor part number    | Aperture(F) |Exposure time(ms)|
| -------------------    | -------| ------------|
| FLIR GS3-u3-32S4C-C    |    1/8 | 30        |
| FLIR GS3-u3-32S4M-C    |    1/8 | 30        |


As for running time, the iterative method takes about 2 hours on the Intel(R)Core(TM)i7-6700K CPU @4.00GHz hardware platform with 32.0G RAM.
