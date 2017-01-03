This is a set of build scripts that will build Delft3d and all dependencies from source on Linux.

Before you begin, you will need to install and configure:

* A modules tool ([Lmod](https://www.tacc.utexas.edu/research-development/tacc-projects/lmod) is recommended).
* EasyBuild

And get some OS-level dependencies, namely...

On CentOS/RHEL: `yum install readline-devel libuuid-devel expat-devel openssl-devel ruby byacc flex`

Compile instructions (in this directory):

Checkout the source code:

```{bash}
svn checkout https://svn.oss.deltares.nl/repos/delft3d/tags/6686 delft3d_repository
# easybuild requires a single source file
tar -czvf delft3d_repository/src.tar.gz delft3d_repository/src
```

Build all of Delft3d's dependencies from scratch. This step builds absolutely everything including compilers and MPI. Understandably, this takes a *very* long time, so be patient. The more cores your computer has, the faster this will go.

```{bash}
module load EasyBuild
eb netCDF-Fortran-4.4.4-gmpich-2016a.eb --robot=$(pwd)
```

Build Delft3d from source. Due to the current state of the Delft3d source code, parallel builds do not work (this is why this is a separate step from building the dependencies).

```{bash}
eb Delft3d-6686-gmpich-2016a.eb --robot=$(pwd) --parallel=1
```

Load and start using Delft3d and/or the built-in tests:

```
username@host$ module load Delft3d
username@host$ module list

Currently Loaded Modules:
  1) EasyBuild/3.0.1             8) netCDF-Fortran/4.4.4-gmpich-2016a
  2) zlib/1.2.8-gmpich-2016a     9) GCCcore/4.9.3
  3) HDF5/1.8.13-gmpich-2016a   10) binutils/2.25-GCCcore-4.9.3
  4) cURL/7.49.1-gmpich-2016a   11) GCC/4.9.3-2.25
  5) gmpich/2016a               12) MPICH/3.2-GCC-4.9.3-2.25
  6) Szip/2.1-gmpich-2016a      13) Delft3d/6686-gmpich-2016a
  7) netCDF/4.4.1-gmpich-2016a

username@host$ test_00

 -----------------------------------------------
 Version: Deltares, NEFIS Version 5.09.00.6847 (Linux64), Dec 13 2016, 12:55:57
 -----------------------------------------------
 Test open/close some files w.r.t read and write
 -----------------------------------------------
 Filenames: C data_c.dat data_c.def

# omitted further output for brevity
```

