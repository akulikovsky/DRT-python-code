-----------------------------------------------------------
This software is provided "as is", without any warranties,
under the GNU public license.
-----------------------------------------------------------
What's new on 21 April, 2025:

- Faster algorithm of DRT calculation.
- New, simpler plotting subroutines
-----------------------------------------------------------

The code calculates the dimensionless DRT G(f), where f is 
the frequency (Hz). G(tau) satisfies to the equation (Latex notations are used)

    Z(\omega) = R_{HFR}
         + R_{pol} \int_{-\infty}^{\infty} G(\tau) d \ln(\tau) / (1 + i \omega \tau)

The function G(f) is related to the DRT gamma(f) as G(f) = gamma(f) /(2*pi*f).
The method employs the Tikhonov regularization in combination with 
the non--negative least squares algorithm "nnls" from
the scipy.optimize library. The code seeks for the vector x minimizing

argmin_x ||(A^T * A + lambT * I) * x - A^T * b||, x >= 0

where A^T is the transposed matrix A, b is the right side vector,
I is the identity matrix, lamT is the regularization parameter,
and x >= 0 means that all elements of the vector x are non--negative. Details
of the matrix A and vector b calculation are described in doi:10.1039/D0CP02094J

1. To install the package, simply unpack the zip-file into a new directory (folder).

2. You need python 3.10+ installed in your machine, and the following python 
packages: numpy 2.20+, scipy 1.3+, matplotlib 3.1.3+.

3. To run the test variant (DRT of the Warburg finite--length impedance)
open a terminal window (cmd in Windows machine) and type

    > python3 Run.py

4. To calculate the DRT from your data, you have two options.

   (a) Supply python procedure which returns 3 numpy arrays:
   omega, zre and zim. Omega must be in the descending order and
   zim must be positive. This procedure must be called instead
   of 'ZWarburg()' in the file 'TPRrun.py'.

   (b) Create a text file 'MyImpedance.dat' with the three columns
   separated by commas:

                frequency(Hz),   zre,   zim

   Frequencies must be in the descending order and zim must be positive.
   Comment on the line

   omg, zre, zim = ZWarburg()

   by inserting the symbol '#' at the first position in the line,
   and uncomment the line

   omg, zre, zim = user_data(fdata)

   Example of 'MyImpedance.dat' is supplied.

The mode of DRT calculation should be set ('real' or 'imag'), see the line

mode = 'real'

in file 'Run.py'. By default, the real part of impedance is used for the DRT
calculation (mode = 'real') . Specify

mode = 'imag'

to use the imaginary part instead.

4. Finally, run

    > python3 Run.py

The regularization parameters lamT will be calculated using the L-curve 
method. You could enter your lamT, if necessary. 

The following plots will be shown during the code execution:
   a) The L--curve and the values of lamT for the problem
      to be solved (see item 4 above)
   b) The Nyquist spectrum of the "experimental" impedance.
   c) The DRT G(f)
   d) The frequency dependence of the real or imaginary part of the experimental
      impedance (points) and the values reconstructed from the DRT (open circles).

6. The output files and plots are placed in the sub-folder 'results'.
The text file 'Warburg_Gfun.dat' contains 2 colums: f / Hz,
and G(f). The text file 'Warburg_zre.dat' contains 3 columns:
f / Hz, experimental Re(z) supplied by the user, and Re(Z) reconstructed
from DRT.

7. To change the prefix 'Warburg_' for output files, edit the line

fname = './results/Warburg_'

in the file 'Run.py'.

8. The algorithm of calculation of peak positions and the area under each peak
is still in the beta-version state and the users are encouraged to use their codes
for these calculations. Please, use the numerical data for G(f).
