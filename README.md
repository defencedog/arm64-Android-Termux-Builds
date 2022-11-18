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

Search for package in repo
`pkg search <keyword to search>`

Discover all file installed by some package
`dpkg -L <package name already installed>`

Search which package provides a named file
`apt-file find <some .so or .h file>`

Check system environment varibles & where they point
`env`

Add environment vars, lib paths, include paths, bin paths
Caution:Why LD_LIBRARY_PATH is bad http://xahlee.info/UnixResource_dir/_/ldpath.html
```
nano ~/.bash_profile
after modifying file, update environment
source ~/.bash_profile
e.g
add these lines at top
PATH=$PATH:$PREFIX/bin:$PREFIX/local/bin
export LD_LIBRARY_PATH=$PREFIX/lib:$PREFIX/local/lib
export PKG_CONFIG_PATH=$PREFIX/lib/pkgconfig:$PREFIX/local/lib/pkgconfig
export C_INCLUDE_PATH=$PREFIX/include:$PREFIX/local/include:$PREFIX/local/include/coin:$PREFIX/local/include/coin-or:$PREFIX/local/include/coin-or/asl:$PREFIX>
export CPLUS_INCLUDE_PATH=$PREFIX/include:$PREFIX/local/include:$PREFIX/local/include/coin:$PREFIX/local/include/coin-or:$PREFIX/local/include/coin-or/asl:$PR>
```
Each folder will have its own `README.txt`

