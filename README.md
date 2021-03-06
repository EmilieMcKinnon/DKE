# Diffusional Kurtosis Estimator
The Diffusional Kurtosis Estimator (DKE) is software for processing 
Diffusional Kurtosis Imaging (DKI) data. 

## Useful Links for DKE
[Introduction to DKI](https://education.musc.edu/colleges/medicine/departments/centers/cbi/dki)

[DKI Protocols & Data Processing](https://education.musc.edu/colleges/medicine/departments/centers/cbi/dki/protocols)

[DKE Results](https://education.musc.edu/colleges/medicine/departments/centers/cbi/dki/results)

[DKE FAQs](https://education.musc.edu/colleges/medicine/departments/centers/cbi/dki/faqs)

[DKI References](https://education.musc.edu/colleges/medicine/departments/centers/cbi/dki/references)

[DKE Forum on NITRC](https://www.nitrc.org/forum/?group_id=652)

## Downloading DKE
DKE is written primarily in MATLAB. It can be run either from executables 
or from source code. An advantage of running DKE from executables is that 
it can be run under the free MATLAB Compiler Runtime (MCR) R2012a (or R2013b 
for OS X), whereas MATLAB is needed for running DKE from source code. 
An advantage of running DKE from source code is that you can modify your 
copy of the source code to suit your own purposes.

### Executables
The DKE executables for 32- and 64-bit Windows, 64-bit Linux, and 64-bit macOS 
are available from [our DKE project pages on NITRC](https://www.nitrc.org/projects/dke/).

### Source Code
The source code can be downloaded from this site using either the Downloads 
menu or via "git clone" (if you have a git client installed). If you download
DKE via "git clone", you can later update to the latest version on this site
with "git pull".

## Prerequisites
In order to run DKE from source code, you need to have MATLAB. The files
dke_preprocess_bruker.m, dke_preprocess_dicom.m, and map_interpolate.m
make calls to [SPM](http://www.fil.ion.ucl.ac.uk/spm/). So if your image
files are in Bruker format or DICOM format, or if you interpolate the
parametric maps, you will need SPM to be installed and in your MATLAB
path.

## Running DKE from source code in MATLAB
Most of the source code for DKE is in the mfiles subdirectory, which should be 
added to your MATLAB path. The subdirectory that has the gradient vectors
file that you use should be added to your MATLAB path, or else the full path 
name of the gradient vectors file used should be specified in the fn_gradients 
parameter in the input parameter file.

DKE makes use of MEX files (MATLAB executable files) for speed. 
Previously-compiled MEX files are in the subdirectories that end in 
"*mex". To use these files, add the appropriate subdirectory to your MATLAB 
path. For example, if you want to run DKE in MATLAB on macOS:
```
>> addpath /path/to/mfiles
>> addpath /path/to/gradientVectors
>> addpath /path/to/mac64mex
>> dke /path/to/DKEParameters.dat
```

### Creating new MEX files
Note that the previously-compiled MEX files might not work on your computer, 
depending on the specific version of the operating system that you are using. 
In that case, you need to recompile the MEX files yourself with MATLAB Coder.

You can create MEX files for augment_constraints, compute_rd, and 
compute_rf by opening the corresponding .prj files in MATLAB. These files are 
in separate directories alongside the mfiles directory. MATLAB Coder will 
convert the .m files into C code and compile the C code into MEX files.

You can also create the MEX file for mlsei using MATLAB Coder. The source 
code is in C (converted from Fortran). Instructions are in mexlsei_setup.m in 
the mlsei/mfiles directory, but some of the steps have been done. Make the 
f2c library, move it to the "c" directory, and then create the MEX file. 
For example, on a 64-bit Linux computer (computer type = glnxa64), in MATLAB:
```
>> pwd

ans =

/home/username/src/dke/mlsei/c/libf2c

>> !make -f makefile.lx64
cc -c f77vers.c
cc -c i77vers.c
cc -c -DSkip_f2c_Undefs -O -DNON_UNIX_STDIO -fPIC main.c
...
ar: creating libf2cx64.a
ranlib libf2cx64.a

>> !mv libf2cx64.a ..

>> cd ../..

>> pwd

ans =

/home/username/src/dke/mlsei

>> mex -L "./c" -lf2cx64 -outdir . -v ./c/mexlsei.c ./c/dlsei.c
...
```

The MEX files can then be moved to the appropriate directory ending in "mex", 
for example linux64mex if you are using 64-bit Linux.

## Changelog

### DKE v2.6.1

**Added**:

* DTI fib file output. If the flag output DTI tracks is on it will
  automatically output the DTI.fib file to. The gaussian dODF is
  used to visualize the ODFs but there is commented code in there to
  change it to the elipsoid. The directions in the fib file for
  tractography is the first eigenvector.

**Changed**:

* Updated DKE version and date-time strings

**Removed**:
* None
