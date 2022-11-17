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
- [x] IPOPT
- [x] CBC
- [x] CLP
- [ ] Bonmin
- [ ] Couenne
- [ ] Scip
- [ ] Zimpl
- [ ] Papilo
- [ ] Soplex
<br>
Following additional libraries for capability enhancement
- Metis
- Mumps
- HSL
- ASL

### Usage
Following packages are available in `termux` default repo or [TUR](https://github.com/termux-user-repository/tur) repowhich can be added easily 
```
pkg i zlib gcc-11 clang libcoinor-utils coinor-clp eigen glpk openblas gsl boost boost-headers p7zip mpich
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
