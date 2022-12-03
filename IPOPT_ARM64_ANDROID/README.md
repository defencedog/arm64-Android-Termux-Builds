### Background
For **chemical engineering** I use following `python` libraries via `jupyter notebook` on my Samsung Galaxy Tab S7. These require Linear & or nonlinear solvers.
- [IDAES-pse](https://github.com/IDAES/idaes-pse)
- [Pyomo](https://github.com/Pyomo/pyomo)
- [Pulp](https://github.com/coin-or/pulp)
- [feos](https://github.com/feos-org/feos)
- [sgtpy](https://github.com/gustavochm/sgtpy)
- [Cantera](https://github.com/Cantera/cantera)

I am particularly thankful to [TUR](https://github.com/termux-user-repository/tur) repo maintainer [Uchiha Kakashi](https://github.com/licy183) for helping me out<br>
### Files may be redundant copy/replace these no issue!
This folder contains all necessary stuff (libraries, binaries, header files) to run following solvers on ARM64 Android via Termux:
- [x] IPOPT 3.14.9
- [x] CBC 2.10.5
- [x] CLP 1.17.5
- [x] Bonmin (git 2022.11.15)
- [x] Couenne (git 2022.11.15)
- [x] Scip 8.0.2.4
- [x] Papilo 2.1.1
- [x] Soplex 6.0.1.3
- [x] Petsc 3.18.1 (located [here](https://github.com/defencedog/arm64-Android-Termux-Builds/tree/main/idaes) alongwith `idaes` python wrapper)

Following additional libraries for capability enhancement

- [x] Metis 5.1.0
- [x] Mumps 3.0.4
- [x] HSL (2021.05.05)
- [x] ASL (2019.06.05)
- [x] CGL 
- [x] Scalapack 2.2.1
- [x] Zimpl 3.5.2
- [x] Bliss 0.77

### [SCIPOPT Suite](https://www.scipopt.org/) now is completely working
It's highly recommended to install python interface. It has many examples
https://github.com/scipopt/PySCIPOpt/tree/master/examples

Procedure to install in Python @ version 4.2

```
export SCIPOPTDIR=$PREFIX/local
export LDFLAGS=-lpython3.9 && MATHLIB=m pip install pyscipopt --verbose
```
Version / Build information
```
SCIP version 8.0.2.4 [precision: 8 byte] [memory: block] [mode: debug] [LP solver: Soplex 6.0.1.3] [GitHash: 1929dc2868]
Copyright (C) 2002-2022 Konrad-Zuse-Zentrum fuer Informationstechnik Berlin (ZIB)

External libraries:
  Readline 8.1         GNU library for command line editing (gnu.org/s/readline)
  Soplex 6.0.1.3       Linear Programming Solver developed at Zuse Institute Berlin (soplex.zib.de) [GitHash: 713be0fe]
  CppAD 20180000.0     Algorithmic Differentiation of C++ algorithms developed by B. Bell (github.com/coin-or/CppAD)
  ZLIB 1.2.12          General purpose compression library by J. Gailly and M. Adler (zlib.net)
  GMP 6.2.1            GNU Multiple Precision Arithmetic Library developed by T. Granlund (gmplib.org)
  ZIMPL 3.5.2          Zuse Institute Mathematical Programming Language developed by T. Koch (zimpl.zib.de)
  AMPL/MP 4e2d45c4     AMPL .nl file reader library (github.com/ampl/mp)
  PaPILO 2.1.1         parallel presolve for integer and linear optimization (github.com/scipopt/papilo) (built with TBB) [GitHash: dea16d4]
  bliss 0.77           Computing Graph Automorphism Groups by T. Junttila and P. Kaski (www.tcs.hut.fi/Software/bliss/)
  Ipopt 3.14.9         Interior Point Optimizer developed by A. Waechter et.al. (github.com/coin-or/Ipopt)

Compiler: clang 15.0.4

Build options:
 ARCH=aarch64
 OSTYPE=Android-4.19.113-24856341
 COMP=Clang 15.0.4
 BUILD=RelWithDebInfo
 DEBUGSOL=OFF
 EXPRINT=cppad
 SYM=bliss
 GMP=ON
 IPOPT=ON
 WORHP=OFF
 LPS=spx
 LPSCHECK=OFF
 NOBLKBUFMEM=OFF
 NOBLKMEM=OFF
 NOBUFMEM=OFF
 THREADSAFE=ON
 READLINE=ON
 SANITIZE_ADDRESS=OFF
 SANITIZE_MEMORY=OFF
 SANITIZE_UNDEFINED=OFF
 SANITIZE_THREAD=OFF
 SHARED=ON
 VERSION=8.0.2.4
 API_VERSION=108
 ZIMPL=ON
 ZLIB=ON
```

### Usage
Following packages are available in `termux` default repo or [TUR](https://github.com/termux-user-repository/tur) repo which can be added easily 
```
pkg i zlib gcc-11 clang libcoinor-utils coinor-clp eigen glpk openblas gsl boost boost-headers p7zip mpich libtbb libgmp libyaml-cpp libyaml
pkg i gcc-default-11 (a small size symlinking tool. Will be intermittently installed & uninstalled)
```
<br>All builds are to be placed in `$PREFIX/local`<br>
Use `tree` command to know the directory structure once unzipping archives using `7z x <file>.7z`. There will be some empty fodlers & required further action will be mentioned in `README.txt`<br>
For instance a `tee` output:
```
~ $ tree metis_aarch64-linux-android/
metis_aarch64-linux-android/
├── README.txt
├── bin
│   ├── gpmetis
│   ├── m2gmetis
│   ├── mpmetis
│   └── ndmetis
├── include
│   ├── coin
│   │   └── ThirdParty
│   ├── metis.h
│   └── metis.h.gch
└── lib
    └── libmetis.so

5 directories, 8 files
```
Each `.7z` include small information of what needs to be further done once extracting respective archive
