-----------------------------------------------------------
This software is provided "as is", without any warranties,
under the GNU public license.
-----------------------------------------------------------

This version calculates the DRT \gamma(f), where f is frequency (Hz).
\gamma(tau) satisfies to equation (Latex notations are used)

    Z(\omega) = R_{HFR}
         + R_{pol} \int_0^{\infty} \gamma(\tau) d \tau / (1 + i \omega \tau)

The method employs non--negative least squares algorithm "nnls" from
scipy.optimize library. The code seeks for the vector x minimizing

argmin_x ||(A^T * A + lambda * I) * x - A^T * b||, x >= 0

where A^T is transposed matrix A, b is the right side vector,
I is the identity matrix, lambda is the regularization parameter,
and x >= 0 means that all elements of vector x are non--negative. Details
of matrix A and vector b calculation are described in doi:10.1039/D0CP02094J


1. To install the package, simply unpack the zip-file into a new
directory (folder).

2. You need python 3.6+ installed in your machine, and
the following python packages: numpy 1.15+, scipy 1.2+,
matplotlib 3.1.3+.

3. To run the test variant (DRT of Warburg finite--length impedance)
open a terminal window (cmd in Windows machine) and type

    > python3 Run.py

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

   omg, zre, zim = user_data(fdata)

   Example of 'MyImpedance.dat' is supplied.

In addition, an initial value of regularization parameters lamT
must be given (see the respective lines in the file 'TPRrun.py'),
and the mode of DRT calculation ('real' or 'imag'), see the line

mode = 'real'

in file 'Run.py'. By default, real part of impedance is used for DRT
calculation (mode = 'real') . Specify

mode = 'imag'

to use the imaginary part instead.

4. The initial value of the regularization parameter lamT can be found
as following. Run the code with arbitrary lamT. The first plot shows the
so-called L-curve for the problem to be solved. Each point on this plot
corresponds to a certain value of lamT indicated at the point.
Note that at some lamT, the points on the plot either start to overlap, or
rapidly rise along the y--axis. Take minimal lamT, at which this change happens.
Break the code execution and set the selected lamT in the file 'TPRrun.py'.
Note that for the test problem (DRT of Warburg impedance), optimal lamT = 1e-7.

5. Finally, run

    > python3 Run.py

The following plots will be shown during the code execution:
   a) The L--curve and the values of lamT for the problem
      to be solved (see item 4 above)
   b) Nyquist spectrum of the"experimental" impedance,
   c) DRT gamma(f)
   d) DRT G(f) = gamma(f) / (2*pi*f)
   d) Frequency dependence of the real or imaginary part of experimental
      impedance (points) and the values reconstructed from the DRT
      (open circles).

6. Output files and plots are placed in the sub-folder './results/'.
The text file 'Warburg_gamma.dat' contains 3 colums: f / Hz,
gamma(f), and G(f) = gamma / (2*pi*f). The text file 'Warburg_zre.dat'
contains 3 columns: f / Hz, experimental Re(z) supplied by the user,
and Re(Z) reconstructed from the DRT.

7. To change the prefix 'Warburg_' for output files, edit the line

fname = './results/Warburg_'

in file 'Run.py'.

8. The algorithm of calculation of peak positions and area under each peak
is in beta-version state and the users are encouraged to use their codes
for these calculations. Please, use the numerical data for gamma.
