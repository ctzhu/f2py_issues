# Steps to get `f2py` to work in `Anaconda` `Python 3.6` in `win-64`

* 1, (Optional) Create a batch file in `<PYTHON>\scripts\`, to pass args to `f2py`(i.e. `f2py36`)
* 2, install `libpython`, `msvc_runtime` and `mingw` using `conda install`
* 3, Fix some of the file names:
   `libmsvcr140.dll.a` to `libmsvcr140.a`
   `libpython36.dll.a` to `libpython36.a`
* 4, create `libvcruntime140.a` file
   - Download `pexports`
   - In `<PYTHON>` folder: `pexports vcruntime140.dll >libs\vcruntime140.def`
   - Create the file with `dlltool`: 
      `dlltool -dllname vcruntime140.dll --def libs\vcruntime140.def --output-lib libs\libvcruntime140.a`
* 5, Create a `distutils.cfg` in `<PYTHON>\Lib\distutils`
* 6, Bug fixes these files: 
   - `<PYTHON>\Lib\distutils\cygwinccompiler.py`
   - `<PYTHON>\Lib\site-packages\numpy\distutils\mingw32ccompiler.py`
   - `<PYTHON>\Lib\site-packages\numpy\distutils\misc_util.py`
* 7, If the `Fortran` code contains `interface` one must generate a `pyf` before compiling. Otherwise there will be an error.
   - `sign2map: Confused: THE NAME of INTERFACE is not in lcb_map[]`
   - Must first run `f2py36 test.f90 -m test -h test.pyf`
   - Then `f2py36 -c test.pyf test.f90 --fcompiler=gnu95 --compiler=mingw32` to get around it
