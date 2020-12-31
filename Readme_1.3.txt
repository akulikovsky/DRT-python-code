-----------------------------------------------------------
This software is provided "as is", without any warranties,
under the GNU public license.
-----------------------------------------------------------

This version of the TPG algorithm calculates the dimensionless
DRT G(f), where f is frequency (Hz). The function G(f) is related
to the DRT gamma(f) as G(f) = gamma(f) /(2*pi*f). G(tau) satisfies 
to equation (Latex notations are used)

    Z(\omega) = R_{HFR} + R_{pol}*\int_{-\infty}^{\infty} G(\tau) d \ln(\tau) / (1 + i*\omega*\tau) 

1. To install the package, simply unpack the zip-file into a new
directory (folder).

2. You need python 3.6+ installed in your machine, and
the following python packages: numpy 1.15+, scipy 1.2+,
matplotlib 3.1.3+.

3. To run the test variant (DRT of Warburg finite--length impedance)
open a terminal window (cmd in Windows machine) and type

    > python3 TPGrun.py

4. To calculate DRT from your data, you have two options.

   (a) Supply python procedure which returns 3 numpy arrays:
   omega, zre and zim. Omega must be in descending order and
   zim must be positive. This procedure must be called instead
   of 'ZWarburg()' in the file 'TPRrun.py'.

   (b) Create a text file 'MyImpedance.dat' with the three
   columns separated by commas:

                frequency(Hz),   zre,   zim

   Frequencies must be in descending order and zim must be positive.
   Comment on the line

   omg, zre, zim = ZWarburg()

   by inserting the symbol '#' at the first position in the line,
   and uncomment the line

   omg, zre, zim = user_data()

   Example of 'MyImpedance.dat' is supplied.

In addition, initial values of regularization parameters lamT0 and lampg0
must be given (see the respective lines in the file 'TPRrun.py'), and the mode
of DRT calculation ('real' or 'imag'), see the line

mode = 'real'

in file 'TPGrun.py'. By default, real part of impedance is used for DRT
calculation (mode = 'real') . Specify

mode = 'imag'

to use the imaginary part instead.

4a. (added in version 1.1)
The initial value of the regularization parameter lamT0 can be found as following.
Run the code with arbitrary lamT0 and lampg0. The first plot shows the
so-called L-curve for the problem to be solved. Each point on this plot
corresponds to a certain value of lamT0 indicated at the point. Take lamT0
corresponding to the lower left corner of the plot (the point
of the maximal plot curvature). Break the code execution and set the selected
lamT0 in the file 'TPRrun.py'. Note that some problems require taken
higher values of lamT0.

4b. (added in version 1.2)
The initial guess for lampg0 can be taken from the second plot
shown during the code execution. Just take the value shown on the second
plot at the point of maximal curvature of the L-curve, as in the section 4a.

5. By default, SciPy least_squares constrained minimizer with 'trf'
option is called. This minimizer uses bounds [lambda / 10, lambda * 10] for
both regularization parameters. If one of the parameters reaches
the boundary value, change the initial guess for this parameter.

6. Finally, run

    > python3 TPGrun.py

The following plots will be shown during the code execution:
   a) The L--curve and the values of lamT0 for the problem
      to be solved (see item 4a above)
   b) The L--curve and the values of lampg0 for the problem
      to be solved (see item 4b above)
   c) Nyquist spectrum. Points correspond to the"experimental" impedance,
      open circles show impedance reconstructed from pure Tikhonov
      regularization solution.
   d) DRT gamma(f) calculated using the Tikhonov regularization. Note
      negative non--physical oscillations of the curve.
   e) Frequency dependence of the real or imaginary part of impedance
      (points) and the values reconstructed from the Tikhonov regularization
      (open circles). The plots (b)--(c) allow to estimate the choice
      of lamT0. Typically, correct selection of lamT0 gives good quality
      of fitting the curve in the plot (d).
   f) Final DRT G(f) = gamma / (2 * pi * f)
   g) Final frequency dependence of the real or imaginary part of impedance
      (points) and the values reconstructed from the TRPG-solution
      (open circles).

   All the plots above will be stored in the sub--folder './results/'

7. Output files and plots are placed in the sub-folder 'results'.
The text file 'Warburg_Gfun.dat' contains 2 colums: f / Hz and G(f).
The text file 'Warburg_zim.dat' contains 3 columns: f / Hz,
experimental Im(z) supplied by the user, and Im(Z) reconstructed from DRT.

8. To change the prefix 'Warburg_' for output files, edit the line

fname = './results/Warburg_'

in file 'TPGrun.py'.

Changes in version 1.3:

- The real and imaginary parts of experimental impedance are normalized to 
R_{pol} right from the code start. The Tikhonov matrix is, thus, dimensionless,
which in some cases improves convergence. 

- The algorithm of calculation of peak positions and area under each peak 
has changed. Yet, however, this algorithm is in beta-version state and 
the users are encouraged to use their codes for these calculations. Please,
use the numerical data for G(f).

   



