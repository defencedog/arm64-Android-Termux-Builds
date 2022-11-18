### These builds are for ARM64 Android 

Add [TUR](https://github.com/termux-user-repository/tur) repo in termux (google `python3.9` is must be installed from TUR repo as its most stable & default Termus repo is rolling release mode & packages will have to bre recompiled again & again <br>
Install a good **gcc compiler with fortran**
`pkg i gcc-11` 
For clever symlinking
`pkg i gcc-default-11`

### Symlinks
`ln -s /apex/com.android.runtime/lib64/bionic/libm.so $PREFIX/lib/`
this math lib is very important for many engineering stuff
So after above command you will have following symlink
`/data/data/com.termux/files/usr/lib/libm.so`

### Bin folder
```
/data/data/com.termux/files/usr/local/bin
```
### Include folders
```
/data/data/com.termux/files/usr/local/include
/data/data/com.termux/files/usr/local/include/coin
/data/data/com.termux/files/usr/local/include/coin/ThirdParty
/data/data/com.termux/files/usr/local/include/coin-or
/data/data/com.termux/files/usr/local/include/coin-or/asl
/data/data/com.termux/files/usr/local/include/coin-or/mumps
/data/data/com.termux/files/usr/local/include/coin-or/hsl
```
### Library folders
```
/data/data/com.termux/files/usr/local/lib
```
### How to use Include / Library folders while compiling stuff
https://stackoverflow.com/a/7619133/1162750


### Troubleshooting
Checkout libraries dependancies
`ldd <some lib> (.so* file extension)`

Checkout build architecture
`objdump -a <some lib/bin>`

Each folder will have its own README.txt
