## Rebuttal Material: Spectral Reconstruction from Dispersive Blur:Approaching Full Light Throughput Spectral Imager

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

Step 4, we further perform white balance by correcting the integral curves instead of the RGB curves measured by the monochromator through solving the equation as following (taken the R channel as an example):

$ RR_cS = b $

where, R means the integral curve of red channel in RGB sensors measured by the monochromator, $R_c$ is a diagonal matrix the elements on the diagonal are correction factors for different channels which may be caused by spectral transmission curve of lens. $b$ means the captures value of red channel. 

since it is difficult to accurately calibrate the spectral transmission of optical lens and the spectral curves of RGB sensor in practice, it is normal that there are some color artifacts in the synthesized rgb images.
Since the prism disperser which leads to nonlinear dispersion is used in our system, it is impossible to keep the wavelength interval
per pixel equal. Instead, we precalibrate the non-linear dispersion of different wavelengths, and correct the reconstructed hyperspectral curves with the calibration results.  

#### <span id="parameters">Parameters setting(#R1)</span>

Different coefficients are used to represent the strength of different constraints which determines convergence speed of algorithm.
Different parameters are choosen empirally in a coarse-to-fine tuning method.
It should be noted that these parameters are almost system-independent and fixed in both synthetic and real experiments. 


\begin{algorithm}[t]
  \label{alg: optimization}
  \caption{Optimization Algorithm}
  \KwIn{Dispersive blurred image ${\bf D}$ and sharp grey image ${\bf G}$}
  \KwOut{Reconstructed multispectral image cube ${\bf S}$}
  {\em //*Initialization*//}\\
  ${\bf S}_\lambda = {\bf G} / N_\lambda$; ~~~ {\em //*$N_\lambda$ is the number of channels.*//} \\
  $\beta_{cs} = 1$; ~~~ $\lambda_{side} = 100$ \;
  \While{$OutIter < MaxOuterIter$}{
    \For{$MidIter = 1 \to MaxMidIter$}{
      {\em //*Lookup table for solving ${\bf Q}$-subproblem*//} \\
      ${\bf Q}_{cs}$ = lookup2Dtable($\nabla {\bf G}$, $\nabla {\bf S}$)\;
      {\em //*CG iteration for solving ${\bf S}$-subproblem*//} \\
      \While{$InnerIter \le MaxInnerIter$}{
        Conjugate Gradient Optimaization for Eq.~\ref{equ: Ssub}. \;
        \If{$ ||\Delta {\bf S}||_2 \le 10^{-7} $}{Break\;}
        InnerIter = InnerIter + 1\;
      }
    }
    $\beta_{cs} = \sqrt{2}{\beta_{cs}}$; ~~ $\lambda_{side} = \sqrt{2}{\lambda_{side}}$\;
  }
\end{algorithm}


For experimental reproducibility, hardware parameters are listed in the table below.

|  Sensor part number    | Aperture(F) |Exposure time(ms)|
| -------------------    | -------| ------------|
| FLIR GS3-u3-32S4C-C    |    1/8 | 30        |
| FLIR GS3-u3-32S4M-C    |    1/8 | 30        |


As for running time, the iterative method takes about 2 hours to converge on the Intel(R)Core(TM)i7-6700K CPU @4.00GHz hardware platform with 32.0G RAM.
