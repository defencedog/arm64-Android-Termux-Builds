### Background
For **chemical engineering** I use following `python` libraries via `jupyter notebook` on my Samsung Galaxy Tab S7. These require Linear & or nonlinear solvers.
- [IDAES-pse](https://github.com/IDAES/idaes-pse)
- [Pyomo](https://github.com/Pyomo/pyomo)
- [Pulp](https://github.com/coin-or/pulp)
- [feos](https://github.com/feos-org/feos)
- [sgtpy](https://github.com/gustavochm/sgtpy)
- [Cantera](https://github.com/Cantera/cantera)

I am particularly thankful to [TUR](https://github.com/termux-user-repository/tur) repo maintainer [Uchiha Kakashi](https://github.com/licy183) for helping me out<br>
This folder contains all necessary stuff (libraries, binaries, header files) to run following solvers on ARM64 Android via Termux:
- [x] IPOPT 3.14.9
- [x] CBC 2.10.5
- [x] CLP 1.17.5
- [x] Bonmin (git 2022.11.15)
- [x] Couenne (git 2022.11.15)
- [ ] Scip
- [ ] Papilo
- [ ] Soplex
- [x] Petsc 3.18.1 (located [here](https://github.com/defencedog/arm64-Android-Termux-Builds/tree/main/idaes) alongwith `idaes` python wrapper)

Following additional libraries for capability enhancement

- [x] Metis 5.1.0
- [x] Mumps 3.0.4
- [x] HSL (2021.05.05)
- [x] ASL (2019.06.05)
- [x] Scalapack 2.2.1
- [ ] Zimpl

### Usage
Following packages are available in `termux` default repo or [TUR](https://github.com/termux-user-repository/tur) repo which can be added easily 
```
pkg i zlib gcc-11 clang libcoinor-utils coinor-clp eigen glpk openblas gsl boost boost-headers p7zip mpich libtbb libgmp libgomp-11 libyaml-cpp libyaml
pkg i gcc-default-11 (a small size symlinking tool. Will be intermittently installed & uninstalled)
```
<br>All builds are to be placed in `$PREFIX/local`<br>
Use `tree` command to know the directory structure once unzipping archives using `7z x <file>.7z`. There will be some empty fodlers & required further action will be mentioned in `README.txt`For instance:
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
