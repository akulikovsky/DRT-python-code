-----------------------------------------------------------
This software is provided "as is", without any warranties,
under GNU public license.
-----------------------------------------------------------

1. To install the package, unpack the zip-file into a new
directory (folder).

2. You need python 3.6+ installed in your machine, and
the following python packages: numpy 1.15+, scipy 1.2+,
matplotlib 3.1.3.

3. To run the test variant (DRT of Warburg finite--length impedance)
open a terminal window (cmd in Windows machine) and type
    > python3 TPGsolver.py

4. To calculate DRT from your data, you have two options.

   (a) Supply python procedure which returns 3 numpy arrays:
   omega, zre and zim. Omega must be in descending order, and
   zim must be positive. This procedure must be called instead
   of 'DataWarburg()' in the file 'TPRrun.py'.

   (b) Create a text file 'MyImpedance.dat' with the three
   columns separated by space(s):

                frequency(Hz)   zre   zim

   Frequencies must be in descending order and zim must be positive.
   Comment on the line 'omg, zre, zim = ZWarburg()'
   and uncomment the line 'omg, zre, zim = user_data()'.
   Example of 'MyImpedance.dat' is supplied.

In addition, initial values of regularization parameters lamT0 and lampg0
must be given (see the respective lines in file 'TPRrun.py').

5. By default, SciPy least_squares constrained minimizer with 'trf'
option is called. This minimizer uses bounds [lambda / 10, lambda * 10] for
both regularization parameters. If one of the parameters reaches
the boundary value, change the initial guess for this parameter.

6. Finally, run
    > python3 TPGrun.py

7. Output files and plots are placed in the sub-folder 'results'.
The text file 'Warburg_gamma.dat' contains 3 colums: f / Hz,
gamma(f) (s^{-1}), and G(f) = gamma / (2 pi f).
The text file 'Warburg_zim.dat' contains 3 columns: f / Hz,
experimental Im(z) supplied by the user, and Im(Z) reconstructed from DRT.

8. To change the prefix 'Warburg_' for output files, edit
the line 'fname = ...' in file 'TPGrun.py'.

