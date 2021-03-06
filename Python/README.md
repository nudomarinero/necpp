# PyNEC is a Python module wrapped from NEC-2 

This module was contributed by Remi Sassolas. It wraps the C++ interface to nec2++. There is a more recent python module
that wraps the C-style interface. It is in the directory ../python.

## How one can build it ?

To build it one must have Swig ver 1.3.25 installed
(http://www.swig.org/). SWIG is the tool used for the wrapping of NEC-2.
- Get the NEC-2 src. 
<NEC-2 directory path> should be something like ".../necppYYYYMMDD/".
- Compile NEC-2.
- Copy the files from the "interface_files/" directory in "<NEC-2
directory path>/src/". These are the interface description files that will be used
by Swig
- Move to "<NEC-2 directory path>/src/".
- Have Swig generate the wrapper code :

swig -c++ -python PyNEC.i

When Swig generates the wrapper code, it also generates a "shadow class" python file (PyNEC.py here) to have a more user friendly module.
This file has been modifed (and shared in several parts) to further improve the module. The resulting python files can be found in the
"python_module/" directory. 

- Compile the wrapper code :

g++ -c nec_context.cpp PyNEC_wrap.cxx -I/usr/local/include/python2.4 -I/usr/local/lib/python2.4/config -DHAVE_CONFIG_H

- Link the compiled wrapper code with the other ".o" files

g++ -shared -lstdc++ nec_context.o nec_output.o c_plot_card.o c_geometry.o misc.o nec_exception.o nec_ground.o c_ggrid.o matrix_algebra.o nec_radiation_pattern.o c_evlcom.o PyNEC_wrap.o -o _PyNEC.so

- Get _PyNEC.so (the shared library) and copy it to the "python_module/" directory (in order to take advantage of the improved python files
mentioned above).
- You can now load the module.  


## How one can load it ?

To load PyNEC move to the "python_module/" directory, and import the
module from a python interpreter :

from PyNEC import * 

Warning : to use PyNEC one must have the numarray module installed
(http://www.stsci.edu/resources/software_hardware/numarray). The version used
for the tests is ver 1.3.3.

Then you can start using the module.



## List of directories

- The directory "interface_files/" contains the different interface files. "PyNEC.i" is the "master" interface file.

- The directory "python_module/" contains the module itself (_PyNEC.so), and the remaining python files are the "shadow class" files
(actually the "shadow class" file generated by Swig have been shared in several parts and some methods have been added).

- The directory "swig_output/" contains different files generated by Swig, that are not much usefull for the end-user.

- The directory "test_scripts/" contains a test script for each result type defined in "nec_results.h".
You should use the python command line, and copy/paste the scripts in it (so they're not genuine "scripts"...).
They create an object corresponding to one result type, and then it's up to you to try and use the different methods available for this object.

- The directory "test_nec_files" contains the nec files corresponding to the different scripts.
