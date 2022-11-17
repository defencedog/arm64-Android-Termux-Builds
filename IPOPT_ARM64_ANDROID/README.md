This folder contains all necessary stuff (libraries, binaries, header files) to run following solvers on ARM64 Android via Termux:
- IPOPT
- CBC
- CLP
- more will be added (Soplex, Papilo, Putsc...)

Following additional libraries for capability enhancement
- Metis
- Mumps
- HSL
- ASL

### Usage
All builds are to be placed in `$PREFIX/local`<br>
Use `tree` command to know the directory structure once unzipping archives using `7z x <file>.7z`. There will be some empty fodlers & reqquired further action will be mentioned in `README.txt`<br>
For instance:
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
Each `.7z` included small information of what needs to be further done once 
